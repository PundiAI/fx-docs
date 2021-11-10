# Cloud Setup

### Simple SSH

* Choose a cloud instance based on the requirements specified in [Hardware Requirements](broken-reference)
* The estimated cost is about 80-100 USD per month
* f(x)Core is system agnostic but Ubuntu is preferred

Remoting into the cloud server

* For windows users, download [Gitbash](https://www.educative.io/edpresso/how-to-install-git-bash-in-windows) or [Putty](https://www.putty.org) (Gitbash is preferred).
* For Mac and linux users, you can connect via the terminal More instructions below: https://kinsta.com/blog/how-to-use-ssh/
* Once setup, ssh into the cloud instance ssh `<ssh_id>@<IP address>` eg.

```bash
ssh root@47.211.41.82
```

### Connecting your localhost to the cloud instance for a specific port

Running the command `ssh -L 127.0.0.1:26657:127.0.0.1:26657<ssh_id>@<IP address>` connects your local port 26657 with your cloud's port 26657. This will be needed for those who want to connect their HD wallets. The command should look something like:

```
ssh -L 127.0.0.1:26657:127.0.0.1:26657 root@47.211.41.82
```
