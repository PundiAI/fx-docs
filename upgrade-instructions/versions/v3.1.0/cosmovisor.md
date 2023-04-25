# Cosmovisor Integration - Binaries

> `cosmovisor` is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, cosmovisor can automatically download the new binary, stop the current binary, switch from the old binary to the new one, and finally restart the node with the new binary.

## 1. Setup Cosmovisor

Installing cosmovisor:

```sh
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@latest
```

Set up the Cosmovisor environment variables. We recommend setting these in your `.profile` so it is automatically set in every session.

```sh
echo "# Setup Cosmovisor" >> ~/.profile
echo "export DAEMON_NAME=fxcored" >> ~/.profile
echo "export DAEMON_HOME=$HOME/.fxcore" >> ~/.profile
source ~/.profile
```

After this, you must make the necessary folders for `cosmosvisor` in your `DAEMON_HOME` directory (`~/.fxcore`) and copy over the current binary.

```sh
mkdir -p ~/.fxcore/cosmovisor
mkdir -p ~/.fxcore/cosmovisor/genesis/bin
mkdir -p ~/.fxcore/cosmovisor/upgrades/fxv3/bin
```

## 2. Download the fxcore release

{% hint style="info" %}
Releases can be found here [https://github.com/FunctionX/fx-core/releases](https://github.com/FunctionX/fx-core/releases)
{% endhint %}

Manually download the binary and extract it to folder:

{% tabs %}
{% tab title="Build from source" %}
```sh
git clone https://github.com/functionx/fx-core.git
```

```sh
git checkout release/v2.4.x
make build
cp ./build/bin/fxcored ~/.fxcore/cosmovisor/genesis/bin/
```

```sh
git checkout release/v3.1.x
make build
cp ./build/bin/fxcored ~/.fxcore/cosmovisor/upgrades/fxv3/bin/
```
{% endtab %}
{% endtabs %}

To check that you did this correctly, ensure your versions of `cosmovisor` are the same:

```sh
cosmovisor version
```

```
cosmovisor version: devel-4846b9b49cbaac9da2c5df74391b9d30bfc4242e
app version: HEAD-cbba9a3bd29cfff6b9b1a7b8154922e8bc9027e8
```

The directory structure after correctly configuring cosmovisor should be like this

```sh
tree ~/.fxcore/cosmovisor
```
```
cosmovisor
├── current -> /root/.fxcore/cosmovisor/genesis
├── genesis
│   ├── bin
│   │   └── fxcored
│   └── upgrade-info.json
└── upgrades
    └── fxv3
        └── bin
            └── fxcored

6 directories, 3 files
```

## 3. Start your node

To keep the process always running. If you're on linux, you can do this by creating a service.

```sh
sudo tee /etc/systemd/system/fxcorevisor.service > /dev/null <<EOF
[Unit]
Description=Fxcore Daemon
After=network-online.target

[Service]
User=root
ExecStart=/root/go/bin/cosmovisor run start --home=/root/.fxcore
Restart=always
RestartSec=3
LimitNOFILE=infinity

Environment="DAEMON_HOME=/root/.fxcore"
Environment="DAEMON_NAME=fxcored"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="UNSAFE_SKIP_BACKUP=true"

[Install]
WantedBy=multi-user.target
EOF
```

{% hint style="info" %}
⚠️Before this, please make sure you have stopped `fxcored` and deleted the old `fxcored.service` file, if not, please execute the following command:
{% endhint %}

```sh
sudo systemctl stop fxcored
sudo rm -rf /etc/systemd/system/fxcored.service
```

Reload, enable and restart the node with daemon service file

```sh
sudo -S systemctl daemon-reload
sudo -S systemctl enable fxcorevisor
sudo -S systemctl restart fxcorevisor
```

Accessing logs

{% tabs %}
{% tab title="Entire Log" %}
```
journalctl -u fxcorevisor -f
```
{% endtab %}
{% endtabs %}
