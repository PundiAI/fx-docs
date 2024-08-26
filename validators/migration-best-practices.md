# Migration Best Practices

When we run a mainnet validator for a network, we always try to run a fully-synced non-validator node as backup. Below is the details about practicing validator swap with a fully synced node.

#### Scenario

We have two nodes: Node A (validator) and Node B (fully-synced non-validator). Both are running under the management of cosmovisor. We have a backup copy of `priv_validator_key.json` from Node A. We want to move the validator role from Node A to Node B.

#### Best Practice

1. **Double Check**: Double check that we have the correct backup for `priv_validator_key.json`.
2. **Stop Node A**: Stop `cosmovisor` service on Node A. Double check the service status to make sure it is off. On [pundiscan](https://pundiscan.io/validators) explorer, you should see the validator missing blocks.
3. **Restart Node A As Non-Validator**: Delete `priv_validator_key.json` from Node A and restart `cosmovisor` service. Check 3 places to ensure no double-signing. First, the newly generated `priv_validator_key.json` should be different from the one on the backup. Second, on [pundiscan](https://pundiscan.io/validators) explorer, you should see the validator missing blocks. Third, run a `BINARY status` command and make sure to see `VotingPower` as 0.
4. **Change Validator Role to Node B**: Stop Node B's `cosmovisor` service. Replace Node B's `priv_validator_key.json` with the backup copy from A. Restart B's `cosmovisor` service. Make sure it is making blocks on [pundiscan](https://pundiscan.io/validators) explorer.
5. **House Clean**: The specifics only apply to how we set up the cluster. You should adapt according to your setup.
   * Swap the server name with "sudo hostname xxx" on each server (for example, between "fxcore\_mainnet" and "fxcore\_mainnet\_backup")
   * Swap the server shortcut in \~/.ssh/config file for each ssh login
   * Swap the log name in the promtail.yml file on each server and restart promtail.
   * Swap the server hosts in the monitoring server deployment script so Prometheus and Grafana get the servers right. Redeploy the monitoring script
   * Update the server names on the server provider (Hetzner, Contabo, AWS, GCP, Alicloud) to avoid confusion
   * Update the inventory file in the cosmos server deployment script. This is not very important since the deployment of both servers are completed. However, it is so to avoid future confusions because both servers will still be running, although in the opposite role of validator vs. non-validator.
