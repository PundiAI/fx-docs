# Node Metrics

## Prometheus metrics

fxcore also supports the use of Prometheus metrics. This monitoring device allows you to keep up to date with you validator nodes especially the status and performance of your validator nodes.

> More information on the list of available metrics and useful queries can be found [here](https://docs.tendermint.com/master/nodes/metrics.html).

## Deploy and Configure Monitoring Services

{% hint style="info" %}
Before deploying monitoring program, docker needs to be installed
{% endhint %}

### Configure node services

To enable the Prometheus metrics, set `prometheus=true` in your config file i.e.`$HOME/.fxcore/config/config.toml`. Through setting the `prometheus_listen_addr` in the config file, you may choose the port for you to monitor your node. It is defaulted to port `26660`.

In the file `./fx-core/develop/prometheus/prometheus.yml` you can configure the target node(s) IP address, multiple nodes can be added in the following format.

{% hint style="info" %}
note the config file is in the **.fxcore** directory and the prometheus.yml file is in **fx-core **directory
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
          TELEGRAM_ADMIN: "XXXXXX"
          TELEGRAM_TOKEN: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    ```
* `TELEGRAM_ADMIN`: The Telegram user id for the admin (not the bot itself, you, the user). The bot will only reply to messages sent from an admin. All other messages are dropped and logged on the bot's console. Your can get your user id from `@userinfobot`.
* `TELEGRAM_TOKEN`: Token you get from `@botfather`
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

For the telegram bot to be able to obtain information from the `alertmanager` you will need to update the username and password within the `--alertmanager.url` in the `./fx-core/develop/docker-compose.yaml` file.

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
