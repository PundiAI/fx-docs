# Cosmovisor Integration - Binaries

> `cosmovisor` is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, cosmovisor can automatically download the new binary, stop the current binary, switch from the old binary to the new one, and finally restart the node with the new binary.

## 1. Setup Cosmovisor

Installing cosmovisor:

```
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

Set up the Cosmovisor environment variables. We recommend setting these in your `.profile` so it is automatically set in every session.

```
echo "# Setup Cosmovisor" >> ~/.profile
echo "export DAEMON_NAME=fxcored" >> ~/.profile
echo "export DAEMON_HOME=$HOME/.fxcore" >> ~/.profile
source ~/.profile
```

After this, you must make the necessary folders for `cosmosvisor` in your `DAEMON_HOME` directory (`~/.fxcore`) and copy over the current binary.

```
mkdir -p ~/.fxcore/cosmovisor
mkdir -p ~/.fxcore/cosmovisor/genesis
mkdir -p ~/.fxcore/cosmovisor/genesis/bin
mkdir -p ~/.fxcore/cosmovisor/upgrades/fxv2
```

## 2. Download the fxcore release

{% hint style="info" %}
Releases can be found here [https://github.com/FunctionX/fx-core/releases](https://github.com/FunctionX/fx-core/releases)
{% endhint %}

Manually download the binary and extract it to folder:

```
wget https://github.com/FunctionX/fx-core/releases/download/v3.1.0/fx-core_3.1.0_Linux_x86_64.tar.gz && tar -xvf fx-core_3.1.0_Linux_x86_64.tar.gz -C ~/.fxcore/cosmovisor/genesis/

wget https://github.com/FunctionX/fx-core/releases/download/v2.4.2/fx-core_2.4.2_Linux_x86_64.tar.gz && tar -xvf fx-core_2.4.2_Linux_x86_64.tar.gz -C ~/.fxcore/cosmovisor/upgrades/fxv2/
```

To check that you did this correctly, ensure your versions of `cosmovisor` and `fxcored` are the same:

```
cosmovisor version
fxcored version
```

## 3. Start your node

To keep the process always running. If you're on linux, you can do this by creating a service.

```
sudo tee /etc/systemd/system/fxcored.service > /dev/null <<EOF
[Unit]
Description=Fxcore Daemon
After=network-online.target

[Service]
User=$USER
ExecStart=/root/go/bin/cosmovisor run start --home=/root/.fxcore
Restart=always
RestartSec=3
LimitNOFILE=infinity

Environment="DAEMON_HOME=/root/.fxcore"
Environment="DAEMON_NAME=fxcored"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="UNSAFE_SKIP_BACKUP=false"

[Install]
WantedBy=multi-user.target
EOFd
```

Reload, enable and restart the node with daemon service file

```
sudo -S systemctl daemon-reload
sudo -S systemctl enable fxcored
sudo -S systemctl restart fxcored
```

