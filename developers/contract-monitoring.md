# Contract Monitoring

Project name: Contract Monitor

Repository: [https://github.com/FunctionX/contract-monitor](https://github.com/FunctionX/contract-monitor)&#x20;

Is it open source: Yes

Prerequisites: Familiarity with [Prometheus](https://prometheus.io/)/[Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/#alertmanager)

Overview: A general solution for smart contract monitoring that enables direct querying of on-chain smart contracts or data from subgraphs

## Introduction

As blockchain ecosystems grow more complex, robust monitoring tools are critical to ensure system reliability and performance. Recognizing this, Function X has developed a tool designed for real-time monitoring and alerting of smart contract activities. This contract monitoring tool is essential for developers and system administrators who need to keep track of smart contracts that have been deployed to production, particularly in a dynamic and decentralized environment.

The contract monitoring tool enables users and teams to directly interact with on-chain smart contracts or to fetch data from subgraphs, making it a useful and versatile asset in their toolkit. This flexibility allows users and teams to set up customized monitoring and alerting configurations that can react to changes in contract states or data flows, ensuring timely responses to potential issues.

## Methodology

Contract Monitor's configuration is straightforward yet powerful. By setting the contract address and ABI in the configuration file, users can specify the methods to call and the parameters involved. This setup is complemented by detailed monitoring rules that can trigger alerts based on the contract's return values or state changes. Here’s a snapshot of what the configuration file might look like for a typical monitoring scenario:

```
{
    "name": "PIP_ETH",
    "address": "0x430a901cCBB48F0363E9cf0756a19ff1F549Ab90",
    "network": "testchain",
    "abi_path": "./abis/testchain/OSM.json",
    "functions": [
        {
            "func_name": "peek",
            "from": "0xe196908c3641A02A4E60651427cc8A3E061b4D29",
            "input": [

            ],
            "output": [
                {
                    "metric_name": "PEEK",
                    "metric_help": "ETH-A price value",
                    "labels": {
                        "severity": "warning"
                    },
                    "alert_rule": {
                        "alert": "makerdao ilk ETH-A price more than 5%",
                        "expr": "(abs(PIP_ETH_PEEK - (PIP_ETH_PEEK offset 60m)))/1e+18  > ((PIP_ETH_PEEK offset 60m) * 0.05) / 1e+18",
                        "for": "0m",
                        "annotations": {
                            "description": "makerdao ilk ETH-A price more than 5% :{{$value}}"
                        }
                    }
                }
            ]
        }
    ]
}

```

For monitoring subgraph data, the configuration format in the configuration file adapts to the specific data structures and needs of the monitored contracts or data fields:

```
{
    "table": "systemStates",
    "metric": [
        {
            "name": "systemStates_totalDebt",
            "type": "gauge",
            "help": "Total debt issued (token.debt)）",
            "labels": {
                "source": "vat_debt"
            },
            "field": "totalDebt",
            "alert_rules": {
                "alert": " systemStates totalDebt change",
                "expr": "abs(systemStates_totalDebt-(systemStates_totalDebt offset 2m)) > systemStates_totalDebt * 0.05",
                "for": "0m",
                "annotations": {
                    "description": "systemStates totalDebt(vat_debt) have changed in two minutes change more than 5%,change data {{$value}}"
                }
            }
        }
    ]
}

```
