---
description: You may setup the node monitoring device
---

# Node Monitoring Device

## Prerequisites&#x20;

We recommend the following for running a node monitoring device:

* 2 or more CPU cores
* At least 40G of disk storage
* At least 4G of memory
* At least 10mbps network bandwidth
* Have to be setup in a separate environment from validator nodes/sentry nodes

{% hint style="info" %}
Before setting up a node monitoring device, you may take a look at the [f(x)Core](https://functionx.gitbook.io/home/f-x-core/installation)[installation](https://functionx.gitbook.io/home/f-x-core/installation) setup to setup the f(x)Core CLI.&#x20;
{% endhint %}

## Prometheus metrics

fxcore also supports the use of Prometheus metrics. This monitoring device allows you to keep up to date with you validator nodes especially the status and performance of your validator nodes.

> More information on the list of available metrics and useful queries can be found [here](https://docs.tendermint.com/master/nodes/metrics.html).

## Deploy and Configure Monitoring Services

{% hint style="info" %}
Before deploying monitoring program, install docker following the [official docs](https://docs.docker.com/compose/install/). &#x20;
{% endhint %}

### Configure node services

To enable the Prometheus metrics, set `prometheus=true` in your config file i.e.`$HOME/.fxcore/config/config.toml`. Through setting the `prometheus_listen_addr` in the config file, you may choose the port for you to monitor your node. It is defaulted to port `26660`.

In the file `./fx-core/develop/prometheus/prometheus.yml` you can configure the target node(s) IP address, multiple nodes can be added in the following format.

{% hint style="info" %}
note the config file is in the **.fxcore** directory and the prometheus.yml file is in \*\*fx-core \*\*directory
{% endhint %}

```yaml
EXAMPLE
static_configs:
      - targets: [ "<IP_ADDRESS_1>:26660"]
        labels:
          name: Mainnet-Validator-01
          chain_id: fxcore
      - targets: [ "<IP_ADDRESS_2>:26660"]
        labels:
          name: Mainnet-Validator-02
          chain_id: fxcore
      - targets: [ "<IP_ADDRESS_3>:26660"]
        labels:
          name: Mainnet-Validator-03
          chain_id: fxcore
```

### Telegram Administrator and Bot Configuration

in the file `./fx-core/develop/docker-compose.yaml` under `alertmanager-bot` - `environment` are the variables `TELEGRAM_ADMIN` and `TELEGRAM_TOKEN`:

*   example:

    ```yaml
    alertmanager-bot:
        container_name: alertmanager-bot
        image: metalmatze/alertmanager-bot:0.4.3
        command:
          - '--alertmanager.url=http://fx:fxcore@alertmanager:9093/'
          - '--store=bolt'
          - '--bolt.path=/data/bot.db'
          - '--template.paths=/templates/default.tmpl'
          - '--listen.addr=0.0.0.0:9091'
        environment:
          TELEGRAM_ADMIN: "XXXXXX\nAdmin1USERID\nAdmin2USERID"
          TELEGRAM_TOKEN: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    ```
* `TELEGRAM_ADMIN`: The Telegram user id for the admin (not the bot itself, you, the user). The bot will only reply to messages sent from an admin. All other messages are dropped and logged on the bot's console. Your can get your user id from `@userinfobot`.
* `TELEGRAM_TOKEN`: Token you get from `@botfather`
* \n is to input multiple admin user
* for more information with regards to the telegram bot, see [here](https://core.telegram.org/bots#creating-a-new-bot) and [here](https://github.com/metalmatze/alertmanager-bot):

### Access the monitoring services

Should you not want to change the default username and password, you can start the monitoring service by using the following command:

```yaml
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor up -d
```

*   open port `:9095` (For eg. `http:// <your_IP_address>:9095`) and you will see the prometheus page. Here you can see all the defined alarm rules. You can change these rules in the file `./fx-core/develop/prometheus/rules/fx-chain-alerts.yml`

    The default username and password are `fx` and `fxcore` respectively.
*   open port `:9093` (For eg. `http:// <your_IP_address>:9093`) and you will see the `alertmanager` page. You can manage alarm notifications here.

    The default username and password are `fx` and `fxcore` respectively.
*   open port `:3000` (For eg. `http:// <your_IP_address>:3000`) and you will see the `grafana` page.

    The default username and password are both `admin`, once you have logged in you will be asked to set a new password.

    After setting a new password, you can go into Dashboards > Manage and select 'Fx Chain Dashboard'. Here you can see a dashboard of various indicators and information of a selected node.
* You may find out the details of [`<your_IP_address>`](https://www.google.com/search?q=what+is+my+ip+address\&rlz=1C5CHFA\_enSG996SG996\&oq=what+is\&aqs=chrome.0.69i59j69i57j69i59j35i39j0i433i512j69i60l3.1255j0j7\&sourceid=chrome\&ie=UTF-8)`.`
* Authorise inbound traffic for the  following ports ranges **9091, 9093, 3000** for  `<your_IP_address>` in node monitoring device.  you can also allow the port range **26660** for \<node\__monitoring\_public\_ip_> in the validator instance.

### Changing The Default Passwords for Prometheus and Alertmanager

{% hint style="info" %}
Do not use `$` in any of your passwords, as it will not work with the `alertmanager.url`
{% endhint %}

You can change the default username and password in the file `./fx-core/develop/prometheus/web-config.yml` with the following format:

```yaml
basic_auth_users:
  <username>: <password_hashed_with_bcrypt>

EXAMPLE
basic_auth_users:
  fx: $2y$10$xCpE/Q5UGHxO1qKR5av2DOJGqTkb6E5G/Dc9VT1AZQxNlQJwQpb0q
```

How to hash with bcrypt

1. install apache2
2. input this command `htpasswd -nBC 10 "" | tr -d ':\\n'`
3. type in password
4. copy hash

For more info on prometheus web-configuration see this [link](https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md#about-bcrypt).

{% hint style="info" %}
If you changed the username and password in the `web-config.yaml`file there are 3 other areas where you need to update as well
{% endhint %}

#### Grafana

For `grafana` to be able to get data from `prometheus` you will need to update the username and password in the file `./fx-core/develop/grafana/provisioning/datasources/datasource.yml`

{% hint style="info" %}
The password here is in text format and does not need to be hashed
{% endhint %}

example:

```yaml
# <string> basic auth username, if used
basicAuthUser: fx
# <string> basic auth password, if used
basicAuthPassword: fxcore
```

#### Prometheus

For `prometheus` to be able to send alerts to `alertmanager` you will need to update the username and password in the file `./fx-core/develop/prometheus/prometheus.yml`

example:

{% hint style="info" %}
The password here is in text format and does not need to be hashed
{% endhint %}

```yaml
# Alertmanager configuration
alerting:
  alertmanagers:
    # Sets the `Authorization` header on every request with the
    # configured username and password.
    # password and password_file are mutually exclusive.
    - scheme: http
      basic_auth:
        username: fx
        password: fxcore
      static_configs:
        - targets:
            - alertmanager:9093
```

#### Alertmanager-bot

For the telegram bot to be able to obtain information from the `alertmanager` you will need to update the username and password within the `--alertmanager.url` in the `./fx-core/develop/docker-compose.yaml` file. Also you may [update](https://functionx.gitbook.io/home/f-x-core/setup-node/node-monitor#updating-node-monitoring-services) the alert-manager-bot.&#x20;

example:

{% hint style="info" %}
The password here is in text format and does not need to be hashed
{% endhint %}

under `alertmanager-bot`, `command` you will find `--alertmanager.url`

```yaml
command:
	-'--alertmanager.url=http://<username>:<password>@alertmanager:9093/'

EXAMPLE
alertmanager-bot:
    container_name: alertmanager-bot
    image: metalmatze/alertmanager-bot:0.4.3
    command:
      - '--alertmanager.url=http://fx:fxcore@alertmanager:9093/'
```

{% hint style="info" %}
Do not use `$` in any of your passwords, as it will not work with the `alertmanager.url`
{% endhint %}

#### Commands

start monitoring service

```bash
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor up -d
```

restart monitoring service

```bash
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor restart
```

stop monitoring service

```bash
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor stop
```

### Updating Node Monitoring Services

{% hint style="info" %}
Important: Do a update by pulling the latest code with the below command, whenever&#x20;

you are making changes to the telegram configuration under `./fx-core/develop/docker-compose.yaml`
{% endhint %}

1. Pull the latest code base

```
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor pull
```

&#x20;2\. Start the monitoring device:

```
docker-compose -f ./fx-core/develop/docker-compose.yaml -p fx-node-monitor up -d
```

{% hint style="info" %}
Ensure you have changed your passwords and also that your data source is configured correctly
{% endhint %}

## Prometheus Rules

| Metric                                              | Rule                                                                                                                                        | Threshold | explain                                                                                                   |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | --------- | --------------------------------------------------------------------------------------------------------- |
| `tendermint_consensus_height`                       | tendermint\_consensus\_height - (tendermint\_consensus\_height offset 1m) == 0                                                              | 0         | The node did not produce blocks in 1 minute                                                               |
| `tendermint_consensus_validators`                   | avg((tendermint\_consensus\_validators{kind="val-node"} - tendermint\_consensus\_validators{kind="val-node"} offset 1m) > 0) by (chain\_id) | 0         | The number of validators has increased compared to the number of validators a minute ago                  |
| `tendermint_consensus_validators`                   | avg((tendermint\_consensus\_validators{kind="val-node"} offset 1m - tendermint\_consensus\_validators{kind="val-node"}) > 0) by (chain\_id) | 0         | The number of validators is reduced compared to the number of validators one minute ago                   |
| `tendermint_consensus_latest_block_height`          | tendermint\_consensus\_latest\_block\_height - (tendermint\_consensus\_latest\_block\_height offset 2m)                                     | 0         | The height of the node does not increase in 2 minutes                                                     |
| `tendermint_consensus_validator_last_signed_height` | tendermint\_consensus\_validator\_last\_signed\_height - (tendermint\_consensus\_validator\_last\_signed\_height offset 2m) == 0            | 0         | The verifier did not sign in 2 minutes                                                                    |
| `tendermint_consensus_validator_missed_blocks`      | tendermint\_consensus\_validator\_missed\_blocks - (tendermint\_consensus\_validator\_missed\_blocks offset 2m) >= 3                        | 3         | The total number of blocks with the verifier address not participating in the signature is greater than 3 |
| `tendermint_consensus_missing_validators`           | tendermint\_consensus\_missing\_validators > 10                                                                                             | 10        | The number of verifiers not participating in the signature exceeds the threshold of 10                    |
| `tendermint_consensus_byzantine_validators`         | tendermint\_consensus\_byzantine\_validators > 0                                                                                            | 0         | The number of Byzantine validators exceeds the threshold 0                                                |
| `tendermint_consensus_byzantine_validators`         | tendermint\_consensus\_byzantine\_validators > 0                                                                                            | 0         | The number of Byzantine validators exceeds the threshold 0                                                |
| `tendermint_consensus_block_interval_seconds_sum`   | tendermint\_consensus\_block\_interval\_seconds\_sum / tendermint\_consensus\_block\_interval\_seconds\_count > 7                           | 7         | The block generation interval exceeds 7 seconds                                                           |
| `tendermint_consensus_rounds`                       | tendermint\_consensus\_rounds != 0                                                                                                          | 0         | Consensus round is not equal to 0                                                                         |
| `tendermint_consensus_num_txs`                      | tendermint\_consensus\_num\_txs > 100                                                                                                       | 100       | The number of block packaging transactions exceeds the threshold of 100                                   |
| `tendermint_mempool_size`                           | tendermint\_mempool\_size > 100                                                                                                             | 100       | The number of unchained transactions in the memory pool exceeds the threshold of 100                      |
| `tendermint_mempool_failed_txs`                     | tendermint\_mempool\_failed\_txs - (tendermint\_mempool\_failed\_txs offset 1m) > 10                                                        | 10        | The number of failed transactions in the memory pool has increased by more than 10 in 1 minute            |
| `tendermint_consensus_fast_syncing`                 | tendermint\_consensus\_fast\_syncing - (tendermint\_consensus\_fast\_syncing offset 5m) != 0                                                | 0         | The current synchronization status of the node is not 0                                                   |
| `tendermint_p2p_peers`                              | tendermint\_p2p\_peers < 5                                                                                                                  | 5         | The number of connected nodes is below the threshold 5                                                    |
| `tendermint_p2p_peers`                              | (tendermint\_p2p\_peers offset 30s) - tendermint\_p2p\_peers > 1                                                                            | 1         | The number of currently connected nodes decreases for 1 minute                                            |
