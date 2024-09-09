# Installation f(x)Core

This guide will explain how to install the `fxcored` CLI onto your system. With this installed on a server, you can participate on the mainnet as either a [Full Node](setup-node/) or a [Validator](../validators/validator-setup.md).

Additionally, you may refer to this [<mark style="color:red;">**YouTube tutorial video**</mark>](https://www.youtube.com/watch?v=Fz0Y3qKG9og\&ab\_channel=FunctionX) to set up your validator.

## Hardware Requirements

We recommend the following for running f(x)Core:

* 4 or more CPU cores
* At least 500G of disk storage
* At least 8G of memory
* At least 10mbps network bandwidth

To see a [quick cloud setup](../fxcore-tutorials/cloud-setup.md) on how to setup and deploy it on the cloud.

## Install build requirements

Install `make` and `gcc`.

On Ubuntu this can be done with the following commands:

{% tabs %}
{% tab title="Ubuntu" %}
```
sudo apt-get update
```

```
sudo apt-get install -y make gcc
```

> Ps: `sudo apt-get install -y make gcc` may have encountered a problem with locked files, just try `sudo apt-get install -y make gcc` again.
{% endtab %}

{% tab title="Mac" %}
Ensure you have [Homebrew](https://brew.sh/) installed.

Once you have Homebrew installed, you may run the following commands to install `make` :

```
brew install make
```

and `gcc`:

```
brew install gcc
```

We'll be needing these commands later so let's install the necessary packages:

```
brew install git
```

```
brew install wget
```
{% endtab %}

{% tab title="Windows" %}
You will need to run git bash. You may find the installation link [here](https://git-scm.com/download/win).

Ensure you have `make` and `gcc` installed and that the paths are set correctly for git bash.

One option for installing `gcc` can be found [here](https://jmeubank.github.io/tdm-gcc/articles/2021-05/10.3.0-release).

{% hint style="info" %}
You may select tdm64-gcc-10.3.0-2

Restart gitbash after installing
{% endhint %}

One option for installing `make` is using `chocolate` , more information can be found [here](https://chocolatey.org/install).

Once you have chocolate installed, run this command:

{% hint style="info" %}
make sure to run gitbash as administrator mode if the following commands do not work
{% endhint %}

```
choco install make
```

Ensure you have all the necessary dependencies and compilers.

```
gcc --version
```

will return:

```
gcc.exe (tdm64-1) 10.3.0
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE
```

and for make:

```
make --version
```

will return:

```
GNU Make 4.3
Built for Windows32
Copyright (C) 1988-2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
{% endtab %}
{% endtabs %}

## Install Go

{% tabs %}
{% tab title="All other environments" %}
Install `go` by following the [official docs](https://golang.org/doc/install). Please select your respective environment❗

> For Ubuntu environment, there may be `permissions denied` issues with unzipping the go zip file, try using `sudo su` to resolve it.
{% endtab %}

{% tab title="If you are remoting into a terminal" %}
Especially if you are remoting into a Ubuntu terminal, run this command to download the `go` installer:

```
wget -c https://dl.google.com/go/go1.20.3.linux-amd64.tar.gz
```

{% hint style="info" %}
After you have downloaded the package and you may proceed to step 2 of the [official docs](https://golang.org/doc/install). Choose your system OS and follow the instructions stated.
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Go 1.19+** or later is required for the f(x)Core. If you are remoting into a terminal, you may input the following command:
{% endhint %}

Setting environment variables:

```
mkdir -p $HOME/go/bin
echo "export PATH=$PATH:/usr/local/go/bin" >> ~/.profile
echo "export PATH=$PATH:$(go env GOPATH)/bin" >> ~/.profile
source ~/.profile
```

## Install the binaries

Next, let's install the latest version of f(x)Core. Make sure you have git installed if not you will be prompted to `install git`. Follow the instruction in the terminal.

{% tabs %}
{% tab title="Mainnet" %}
```
git clone --branch release/v3.1.x https://github.com/functionx/fx-core.git
```

```
cd fx-core
```
{% endtab %}

{% tab title="Testnet" %}
```
git clone --branch release/v3.1.x https://github.com/functionx/fx-core.git
```

```
cd fx-core
```
{% endtab %}

{% tab title="Windows" %}
{% hint style="info" %}
You should run your commands in gitbash. But open fxcored.exe file using cmd prompt

Make sure the name of your folder does not have whitespaces!
{% endhint %}

```
git clone --branch release/v3.1.x https://github.com/functionx/fx-core.git
```

```
cd fx-core
```

```
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You may find the latest branches here: [https://github.com/FunctionX/fx-core/branches](https://github.com/FunctionX/fx-core/branches)
{% endhint %}

{% tabs %}
{% tab title="All Other Environments " %}
```
make go.sum
```

```
make install
```
{% endtab %}

{% tab title="Windows " %}
```
make go.sum
```

```
make build-win
```

{% hint style="info" %}
use cmd prompt to open the fxcored.exe file

in the path ./build/bin/fxcored.exe
{% endhint %}
{% endtab %}
{% endtabs %}

That will install the `fxcored` binary. Verify version:

{% tabs %}
{% tab title="Short version" %}
```
fxcored version
```
{% endtab %}

{% tab title="Long version" %}
```
fxcored version --long
```
{% endtab %}
{% endtabs %}

`fxcored version --long` should output something similar to:

```
build_deps:
- cloud.google.com/go@v0.112.0
- cloud.google.com/go/compute/metadata@v0.2.3
- cloud.google.com/go/iam@v1.1.5
- cloud.google.com/go/storage@v1.36.0
- cosmossdk.io/api@v0.3.1
- cosmossdk.io/core@v0.5.1
- cosmossdk.io/depinject@v1.0.0-alpha.4
- cosmossdk.io/errors@v1.0.1
- cosmossdk.io/log@v1.3.1
- cosmossdk.io/math@v1.3.0
- cosmossdk.io/tools/rosetta@v0.2.1
- filippo.io/edwards25519@v1.0.0
- github.com/99designs/keyring@v1.2.1 => github.com/cosmos/keyring@v1.2.0
- github.com/ChainSafe/go-schnorrkel@v1.0.0
- github.com/DataDog/zstd@v1.5.2
- github.com/VictoriaMetrics/fastcache@v1.6.0
- github.com/armon/go-metrics@v0.4.1
- github.com/aws/aws-sdk-go@v1.44.203
- github.com/beorn7/perks@v1.0.1
- github.com/bgentry/go-netrc@v0.0.0-20140422174119-9fd32a8b3d3d
- github.com/bgentry/speakeasy@v0.1.1-0.20220910012023-760eaf8b6816
- github.com/btcsuite/btcd@v0.23.4
- github.com/btcsuite/btcd/btcec/v2@v2.3.2
- github.com/btcsuite/btcd/btcutil@v1.1.3
- github.com/btcsuite/btcd/chaincfg/chainhash@v1.0.1
- github.com/btcsuite/btcutil@v1.0.3-0.20201208143702-a53e38424cce
- github.com/cenkalti/backoff/v4@v4.1.3
- github.com/cespare/xxhash/v2@v2.2.0
- github.com/chzyer/readline@v1.5.1
- github.com/cockroachdb/apd/v2@v2.0.2
- github.com/cockroachdb/errors@v1.10.0
- github.com/cockroachdb/logtags@v0.0.0-20230118201751-21c54148d20b
- github.com/cockroachdb/pebble@v0.0.0-20230807182518-7bcdd55ef1e3
- github.com/cockroachdb/redact@v1.1.5
- github.com/coinbase/rosetta-sdk-go@v0.7.9
- github.com/cometbft/cometbft@v0.37.9
- github.com/cometbft/cometbft-db@v0.8.0
- github.com/confio/ics23/go@v0.9.0
- github.com/cosmos/btcutil@v1.0.5
- github.com/cosmos/cosmos-proto@v1.0.0-beta.5
- github.com/cosmos/cosmos-sdk@v0.47.13
- github.com/cosmos/go-bip39@v1.0.0
- github.com/cosmos/gogogateway@v1.2.0
- github.com/cosmos/gogoproto@v1.4.10 => github.com/cosmos/gogoproto@v1.4.10
- github.com/cosmos/iavl@v0.20.1 => github.com/cosmos/iavl@v0.20.1
- github.com/cosmos/ibc-go/v7@v7.6.0
- github.com/cosmos/ics23/go@v0.10.0
- github.com/cosmos/ledger-cosmos-go@v0.12.4 => github.com/cosmos/ledger-cosmos-go@v0.12.4
- github.com/cosmos/rosetta-sdk-go@v0.10.0
- github.com/creachadair/taskgroup@v0.4.2
- github.com/davecgh/go-spew@v1.1.2-0.20180830191138-d8f796af33cc
- github.com/deckarep/golang-set/v2@v2.1.0
- github.com/decred/dcrd/dcrec/secp256k1/v4@v4.1.0
- github.com/desertbit/timer@v0.0.0-20180107155436-c41aec40b27f
- github.com/dlclark/regexp2@v1.7.0
- github.com/dop251/goja@v0.0.0-20230122112309-96b1610dd4f7
- github.com/dvsekhvalnov/jose2go@v1.6.0
- github.com/edsrzf/mmap-go@v1.0.0
- github.com/ethereum/go-ethereum@v1.10.26 => github.com/functionx/go-ethereum@v1.10.20-0.20231207063621-43cf32d91c3e
- github.com/evmos/ethermint@v0.22.0 => github.com/functionx/ethermint@v0.6.1-0.20240711082233-ae8bfa5a35b3
- github.com/fbsobreira/gotron-sdk@v0.0.0-20211012084317-763989224068
- github.com/felixge/httpsnoop@v1.0.4
- github.com/fsnotify/fsnotify@v1.7.0
- github.com/gballet/go-libpcsclite@v0.0.0-20190607065134-2772fd86a8ff
- github.com/getsentry/sentry-go@v0.23.0
- github.com/go-kit/kit@v0.12.0
- github.com/go-kit/log@v0.2.1
- github.com/go-logfmt/logfmt@v0.6.0
- github.com/go-logr/logr@v1.3.0
- github.com/go-logr/stdr@v1.2.2
- github.com/go-sourcemap/sourcemap@v2.1.3+incompatible
- github.com/go-stack/stack@v1.8.1
- github.com/godbus/dbus@v0.0.0-20190726142602-4481cbc300e2
- github.com/gofrs/flock@v0.8.1
- github.com/gogo/googleapis@v1.4.1
- github.com/gogo/protobuf@v1.3.2
- github.com/golang/groupcache@v0.0.0-20210331224755-41bb18bfe9da
- github.com/golang/mock@v1.6.0
- github.com/golang/protobuf@v1.5.4
- github.com/golang/snappy@v0.0.5-0.20220116011046-fa5810519dcb
- github.com/google/btree@v1.1.2
- github.com/google/go-cmp@v0.6.0
- github.com/google/orderedcode@v0.0.1
- github.com/google/s2a-go@v0.1.7
- github.com/google/uuid@v1.6.0
- github.com/googleapis/enterprise-certificate-proxy@v0.3.2
- github.com/googleapis/gax-go/v2@v2.12.0
- github.com/gorilla/handlers@v1.5.1
- github.com/gorilla/mux@v1.8.1
- github.com/gorilla/websocket@v1.5.0
- github.com/grpc-ecosystem/go-grpc-middleware@v1.3.0
- github.com/grpc-ecosystem/grpc-gateway@v1.16.0
- github.com/gsterjov/go-libsecret@v0.0.0-20161001094733-a6f4afe4910c
- github.com/gtank/merlin@v0.1.1
- github.com/gtank/ristretto255@v0.1.2
- github.com/hashicorp/go-cleanhttp@v0.5.2
- github.com/hashicorp/go-getter@v1.7.5
- github.com/hashicorp/go-immutable-radix@v1.3.1
- github.com/hashicorp/go-safetemp@v1.0.0
- github.com/hashicorp/go-version@v1.6.0
- github.com/hashicorp/golang-lru@v0.5.5-0.20210104140557-80c98217689d
- github.com/hashicorp/golang-lru/v2@v2.0.7
- github.com/hashicorp/hcl@v1.0.0
- github.com/hdevalence/ed25519consensus@v0.1.0
- github.com/holiman/bloomfilter/v2@v2.0.3
- github.com/holiman/uint256@v1.2.2
- github.com/huandu/skiplist@v1.2.0
- github.com/huin/goupnp@v1.0.3
- github.com/improbable-eng/grpc-web@v0.15.0
- github.com/jackpal/go-nat-pmp@v1.0.2
- github.com/jmespath/go-jmespath@v0.4.0
- github.com/klauspost/compress@v1.17.0
- github.com/kr/pretty@v0.3.1
- github.com/kr/text@v0.2.0
- github.com/lib/pq@v1.10.7
- github.com/magiconair/properties@v1.8.7
- github.com/manifoldco/promptui@v0.9.0
- github.com/mattn/go-colorable@v0.1.13
- github.com/mattn/go-isatty@v0.0.20
- github.com/mattn/go-runewidth@v0.0.9
- github.com/matttproud/golang_protobuf_extensions@v1.0.4
- github.com/mimoo/StrobeGo@v0.0.0-20210601165009-122bf33a46e0
- github.com/minio/highwayhash@v1.0.2
- github.com/mitchellh/go-homedir@v1.1.0
- github.com/mitchellh/go-testing-interface@v1.14.1
- github.com/mitchellh/mapstructure@v1.5.0
- github.com/mtibben/percent@v0.2.1
- github.com/olekukonko/tablewriter@v0.0.5
- github.com/pelletier/go-toml/v2@v2.1.0
- github.com/pkg/errors@v0.9.1
- github.com/pmezard/go-difflib@v1.0.1-0.20181226105442-5d4384ee4fb2
- github.com/prometheus/client_golang@v1.14.0
- github.com/prometheus/client_model@v0.3.0
- github.com/prometheus/common@v0.42.0
- github.com/prometheus/procfs@v0.9.0
- github.com/rakyll/statik@v0.1.7
- github.com/rcrowley/go-metrics@v0.0.0-20201227073835-cf1acfcdf475
- github.com/rogpeppe/go-internal@v1.11.0
- github.com/rs/cors@v1.11.0
- github.com/rs/zerolog@v1.33.0
- github.com/sagikazarmark/slog-shim@v0.1.0
- github.com/shengdoushi/base58@v1.0.0
- github.com/shirou/gopsutil@v3.21.4-0.20210419000835-c7a38de76ee5+incompatible
- github.com/shirou/gopsutil/v3@v3.24.4
- github.com/spf13/afero@v1.11.0
- github.com/spf13/cast@v1.7.0
- github.com/spf13/cobra@v1.8.1
- github.com/spf13/pflag@v1.0.5
- github.com/spf13/viper@v1.18.2
- github.com/status-im/keycard-go@v0.2.0
- github.com/stretchr/testify@v1.9.0
- github.com/subosito/gotenv@v1.6.0
- github.com/syndtr/goleveldb@v1.0.1-0.20220721030215-126854af5e6d => github.com/syndtr/goleveldb@v1.0.1-0.20210819022825-2ae1ddf74ef7
- github.com/tendermint/go-amino@v0.16.0
- github.com/tidwall/btree@v1.7.0
- github.com/tidwall/gjson@v1.14.4
- github.com/tidwall/match@v1.1.1
- github.com/tidwall/pretty@v1.2.0
- github.com/tidwall/sjson@v1.2.5
- github.com/tklauser/go-sysconf@v0.3.12
- github.com/tklauser/numcpus@v0.6.1
- github.com/tyler-smith/go-bip39@v1.1.0
- github.com/ulikunitz/xz@v0.5.11
- github.com/zondax/hid@v0.9.2
- github.com/zondax/ledger-go@v0.14.3
- go.opencensus.io@v0.24.0
- go.opentelemetry.io/contrib/instrumentation/google.golang.org/grpc/otelgrpc@v0.46.1
- go.opentelemetry.io/contrib/instrumentation/net/http/otelhttp@v0.46.1
- go.opentelemetry.io/otel@v1.21.0
- go.opentelemetry.io/otel/metric@v1.21.0
- go.opentelemetry.io/otel/trace@v1.21.0
- golang.org/x/crypto@v0.25.0
- golang.org/x/exp@v0.0.0-20230905200255-921286631fa9 => golang.org/x/exp@v0.0.0-20230711153332-06a737ee72cb
- golang.org/x/net@v0.23.0
- golang.org/x/oauth2@v0.16.0
- golang.org/x/sync@v0.7.0
- golang.org/x/sys@v0.22.0
- golang.org/x/term@v0.22.0
- golang.org/x/text@v0.16.0
- golang.org/x/time@v0.5.0
- google.golang.org/api@v0.155.0
- google.golang.org/genproto@v0.0.0-20240123012728-ef4313101c80
- google.golang.org/genproto/googleapis/api@v0.0.0-20240123012728-ef4313101c80
- google.golang.org/genproto/googleapis/rpc@v0.0.0-20240325203815-454cdb8f5daa
- google.golang.org/grpc@v1.62.1
- google.golang.org/protobuf@v1.33.0
- gopkg.in/ini.v1@v1.67.0
- gopkg.in/yaml.v2@v2.4.0
- gopkg.in/yaml.v3@v3.0.1
- nhooyr.io/websocket@v1.8.11
- pgregory.net/rapid@v1.1.0
- sigs.k8s.io/yaml@v1.4.0
build_tags: netgo,ledger
commit: 9794ab0b8944c7bd0d7099a6eff7a2a3b4cdf0f3
cosmos_sdk_version: v0.47.13
go: go version go1.21.0 linux/amd64
name: fxcore
server_name: fxcored
version: v7.5.0
```

### Build Tags

Build tags indicate special features that have been enabled in the binary.

| Build Tag | Description                                     |
| --------- | ----------------------------------------------- |
| netgo     | Name resolution will use pure Go code           |
| ledger    | Ledger devices are supported (hardware wallets) |
