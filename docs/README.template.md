# ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) Important!! RogerThat has been rewritten for MQTT!
## Setup has completely changed to support MQTT with Hummingbot's new MQTT Bridge
## RogerThat currently works on Linux/Mac Intel and Windows. Mac M1 is not currently supported. It has been tested on Mac Intel and Ubuntu 18.04, 20.04, 22.04.

# RogerThat

**RogerThat** is a standalone python web server designed to receive **TradingView** alerts (or similar) and forward them to **Hummingbot**.

**TradingView** is a widely used market market tracker that allows users to create, share and use custom strategies but is quite limited in how it can be "plugged in" or connected to other services. It does however allow the creation of "**Alerts**" which can send data to a Webhook (requires a public-facing URL).

**RogerThat** facilitates the collection and forwarding of these **TradingView Alerts** to **Hummingbot** via the **MQTT Bridge** module, which listens to **RogerThat** via subscribed MQTT topics.

Whilst RogerThat's purpose is to bridge **TradingView** and **Hummingbot**, it can work as a gateway / bridge between any service that sends data via Webhooks (to a public URL) and serve / route them to *multiple* **Hummingbot** instances or connected MQTT clients.

##### Menu

- [Docker](#docker)
  * [Installation](#installation)
  * [Install Docker Engine](#install-docker-engine)
  * [Running](#running)
  * [Stopping](#stopping)
  * [Configuration](#configuration)
    + [Public access](#public-access)
- [Usage](#usage)
  * [TradingView Webhooks](#tradingview-webhooks)
  * [Hummingbot Connection](#hummingbot-connection)
  * [Test Connection](#test-connection)
- [Source Installation (ADVANCED!!!)](#source-installation-advanced)

# Docker
## Installation

[**Install Docker Desktop**](https://www.docker.com/products/docker-desktop/)

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **Docker Desktop only works if you have a graphical user interface (GUI). For headless servers, use the installation method: `Install Docker Engine`.**

---
## Install Docker Engine

<details>
 <summary>General Steps</summary>

[**Install Docker Engine (Headless servers)**](https://docs.docker.com/engine/install/)

[**Fix Docker Permissions**](https://docs.docker.com/engine/install/linux-postinstall/)

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **When running on Linux be sure to apply the post install steps or you WILL run into permission errors.**
 
 </details>

<details>
 <summary>Debian/Ubuntu Steps</summary>
 
 First, you need to make sure your system is up-to-date. Do this by running the following commands:
 ```bash
 sudo apt-get update && sudo apt-get upgrade
 ```
 Next, uninstall any existing versions:
 ```bash
 sudo apt-get remove docker docker-engine docker.io containerd runc
 ```
 Next, install some necessary packages that Docker needs:
 ```bash
 sudo apt-get install apt-transport-https ca-certificates curl software-properties-common gnupg
 ```
 Add the GPG key for Docker's official repository to your system:
 ```bash
 sudo install -m 0755 -d /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 sudo chmod a+r /etc/apt/keyrings/docker.gpg
 ```
 Add Docker's repository to your APT sources:
 ```bash
 echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 ```
 Update the package database with Docker packages from the newly added repo:
 ```bash
 sudo apt-get update
 ```
 Now you can install Docker:
 ```bash
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 ```
 Docker should now be installed, the daemon started, and the process should run on boot. You can verify this by checking the service's status:
 ```bash
 sudo systemctl status docker
 ```

 Use this command to create the Docker group if it doesn't exist:
 ```bash
 sudo groupadd docker
 ```
 
 Use this command to add the current user to the Docker group:
 ```bash
 sudo usermod -aG docker $USER
 ```
 This command will make the changes effective without having to log out and log back in:
 ```bash
 newgrp docker
 ```
 
 </details>

---
## Install RogerThat

Download and extract this [**whole repository**](https://github.com/{{repository.name}}/archive/refs/heads/{{current.readme_install_branch}}.zip).

<details>
<summary>Linux/Mac</summary>

```bash
wget https://github.com/{{repository.name}}/archive/refs/heads/{{current.readme_install_branch}}.zip
unzip {{current.readme_install_branch}}.zip
```

Change directory:
```bash
cd RogerThat-{{current.readme_install_branch}}
```
![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) **Important! If you encounter an error message when you start it for the first time: `Error - Failed to create tables` you can resolve it by following these steps. Press `Ctrl + C` to exit the program, and then start it again. This should help resolve the issue.**

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **You must always run scripts from the main RogerThat directory, do not switch to the `scripts` directory**

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **Do not run as root!!!**

</details>
<details>
<summary>Windows</summary>

Manually download and extract the [**repository zip file**](https://github.com/{{repository.name}}/archive/refs/heads/{{current.readme_install_branch}}.zip).

Open up Windows CMD and **switch directory to the extracted zip folder**.

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **You must always run scripts from the main project directory, do not switch to the `scripts` directory**

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **If using Ubuntu WSL with Docker for Windows, you must enable permissions first, [see here](https://stackoverflow.com/a/50856772/16574146)**

</details>

Then run the following commands to launch the docker image:

<details>
<summary>Linux/Mac</summary>

```bash
./scripts/start_docker.sh
```
</details>
<details>
<summary>Windows</summary>

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **If using windows, make sure to run the .bat scripts using windows CMD prompt, not "Git Bash".**

Use git bash only for `git` commands, do not run RogerThat scripts from git bash as they will not work.

```bat
scripts\start_docker.bat
```
</details>

___

## Running

<details>
<summary>Linux/Mac</summary>

```bash
./scripts/start_docker.sh
```

OR as daemon (in background):

```bash
./scripts/start_docker.sh -d
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\start_docker.bat
```

OR as daemon (in background):

```bat
scripts\start_docker.bat -d
```
</details>

Upon successful launch, navigating to `http://localhost/` (or your public domain) in your browser will show a 404 not found page.

___

## Stopping

Press ctrl+c if running interactively, or run the following command to stop RogerThat:
```bash
docker compose stop
```

___

## Configuration

Edit the configuration files in `./configs` as needed.

___

### MQTT

<details>
<summary>Localhost, Non-Docker Broker</summary>

If running a MQTT broker locally (not docker) you should be able to use `localhost` as your `mqtt_host`:

```yaml
...
mqtt_host: localhost
...
```

</details>

<details>
<summary>Localhost, Docker Desktop based Broker</summary>

Running a MQTT broker via Docker Desktop (not advised) you should be able to use `host.docker.internal` as your `mqtt_host`:

```yaml
...
mqtt_host: host.docker.internal
...
```

</details>

<details>
<summary>Localhost, Docker Engine based Broker</summary>

Use the setup command as below to automagically find your EMQX hostname and update compose and config files:

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --setup-emqx-docker-hostname
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --setup-emqx-docker-hostname
```
</details>

<details>
<summary>Manual Steps if script fails</summary>

Running a MQTT broker via Docker in a Linux box on the same host (e.g. the default hummingbot EMQX setup) you'll need to add rogerthat to the same docker network.

To find the name of the docker network run the command:
```bash
docker network ls
```

You should see something like:
```
e872661fddcc   hummingbot_broker_emqx-bridge   bridge    local
```

The network name in this example is `hummingbot_broker_emqx-bridge`.

Edit the `docker-compose.yml` file in the root directory.

Add the emqx network to the network list at the bottom like this:

```yaml
...
networks:
  rogerthat-bridge:
    driver: bridge
  hummingbot_broker_emqx-bridge:
    external: true

```

Add the emqx network to the rogerthat service like this:
```yaml
    ...
    entrypoint: ["/home/rogerthat/docker_compose_entrypoint.sh"]
    networks:
      - rogerthat-bridge
      - hummingbot_broker_emqx-bridge
    ...
```

Now run the following command to find the hostname of your MQTT broker replacing `hummingbot_broker_emqx-bridge` as needed:
```bash
docker network inspect hummingbot_broker_emqx-bridge
```

You will see the hostname of the MQTT broker in the output like this:

```json
    "Containers":
    {
        "eb1d17a525cb06a863d10f227cdf7edcd713371fe3f699921360b7b23c512c78":
        {
            "Name": "hummingbot_broker-emqx1-1",
            "EndpointID": "8d8c81332a0284e76246bf0bb19d25987e255d9cf9c43cdeed7df9f5ea436cde",
            "MacAddress": "02:42:ac:13:00:02",
            "IPv4Address": "172.19.0.2/16",
            "IPv6Address": ""
        }
    }
```

Where `hummingbot_broker-emqx1-1` is the hostname of the MQTT broker in this example.

You can then edit your `configs/gateway_mqtt.yml` file and add the service name e.g. `hummingbot_broker-emqx1-1` as your `mqtt_host`:

```yaml
...
mqtt_host: hummingbot_broker-emqx1-1
...
```

</details>

</details>

___

### Special Tasks

For certain special tasks, you can use the config setup script via docker by running the following commands:

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --help
```

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **Do not run the setup script via python, always run it via `scripts/setup_config.sh`.**

</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --help
```

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **Do not run the setup script via python, always run it via `scripts\setup_config.bat`.**

</details>

___

### Public access

<details>
<summary>Expand ...</summary>

Since **TradingView** requires a publicly accessible URL for webhook alerts, you'll need to use your own domain name, or your public IP address.

You'll also need to open up (and forward) port **80** (or **443** if using HTTPS) in your firewall/router to the machine running **RogerThat**.

It is recommended to use **Cloudflare** proxied with **HTTPS** to mask your IP address.

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **You must change/set your hostname before enabling HTTPS with letsencrypt**

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **(Do NOT open up port 10073 externally)**

#### Change Hostname

<details>
<summary>Expand ...</summary>

Change the hostname to listen on for the public **TradingView** webhook with the following commands:

(Do not use a full URL here, the hostname is the part of the URL after https:// and before any other slashes)

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --hostname yourhostname.com
scripts/setup_config.sh --hostname 1.2.3.4
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --hostname yourhostname.com
scripts\setup_config.bat --hostname 1.2.3.4
```
</details>

If using your own domain name, it is recommended to use a long and not obvious subdomain as the hostname eg: `thereisnotraderhere.mydomain.com`.

</details>

#### Cloudflare (Recommended)

<details>
<summary>Expand ...</summary>

It is recommended to use [Cloudflare](https://www.cloudflare.com/) to proxy and mask your IP address in production since public access must be exposed.

You can use services like [DNS-o-matic](https://dnsomatic.com/) with your home dynamic IP to keep it updated and proxied with [Cloudflare](https://www.cloudflare.com/).

[More information in the help article here](https://support.cloudflare.com/hc/en-us/articles/360020524512-Manage-dynamic-IPs-in-Cloudflare-DNS-programmatically#h_161458650101544484552881)

</details>

#### Dynamic Domain Names (Optional)

<details>
<summary>Expand ...</summary>

Services you can use for dynamic DNS with a non-static public IP address are:

* [DNS-O-matic](https://dnsomatic.com/) (Recommended, with Cloudflare)
* [No-IP](https://www.noip.com/)
* [Afraid](https://afraid.org/)
* [Duck DNS](https://duckdns.org/)
* [Dynu](http://www.dynu.com/)

</details>

#### Enabling HTTPS (Recommended, required for Cloudflare)

<details>
<summary>Expand ...</summary>

To setup ([LetsEncrypt](https://letsencrypt.org/getting-started/) run the following commands.

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **You must set your hostname first and forward port 80 on your firewall!**

<details>
<summary>Linux/Mac</summary>

```bash
scripts/generate_cert_letsencrypt.sh
```

</details>
<details>
<summary>Windows</summary>

```bat
scripts/generate_cert_letsencrypt.bat
```
</details>

Or run the following command to generate a self-signed key pair:

<details>
<summary>Linux/Mac</summary>

```bash
scripts/generate_cert_self_signed.sh
```

</details>
<details>
<summary>Windows</summary>

```bat
scripts\generate_cert_self_signed.bat
```
</details>

After enabling HTTPS you can now forward port 443, close port 80 and start RogerThat.

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **It is recommended to close port 80**

</details>

</details>

___

# Usage

## TradingView Webhooks

Set up your **TradingView** alert URL like this:

```html
http://<public-ip-or-domain-name>/api/tv_webhook/?api_key=<tradingview-apikey>
```

Where `<public-ip-or-domain-name>` is your domain name or public IP address and `<tradingview-apikey>` is the generated api key found in `web_server.yml` under `api_allowed_keys_tv`.

### JSON Data for TradingView alerts.

Alerts must be formatted as JSON, the only required parameter is `topic`.

The `topic` key must be present in the JSON data to be accepted.

<details>
<summary>Example alert data:</summary>

Adjusting the Bid and Ask Spreads

```json
{
    "topic": "hbot/hummingbot_instance_1/command_shortcuts",
    "params": [
        ["spreads", "1", "1"]
    ]
}
```

Simple Start command

```json
{
    "topic": "hbot/hummingbot_instance_1/start",
    "log_level": "DEBUG"
}
```

Simple Stop command

```json
{
    "topic": "hbot/hummingbot_instance_1/stop",
    "skip_order_cancellation": false
}
```

Advanced alert with all fields using Pine variables

```json
{
    "topic": "hbot/hummingbot_instance_1/external/events/my_event",
    "type": "external_event",
    "timestamp": "{{timenow}}",
    "sequence": "{{timenow}}",
    "data": {
        "exchange": "{{exchange}}",
        "symbol": "{{ticker}}",
        "interval": "{{interval}}",
        "price": "{{close}}",
        "volume": "{{volume}}",
        "position": "{{strategy.market_position}}",
        "inventory": "{{strategy.order.comment}}"
    }
}
```

</details>

___

## Hummingbot Connection

Connect the **Hummingbot** _MQTT Bridge_ and **RogerThat** to the same MQTT Broker.

### Example Config

<details>
<summary>Example Config ...</summary>

Use something like the following config to connect **RogerThat** to **Hummingbot** via the **MQTT Bridge**.

This config is found inside your main hummingbot folder then `conf\conf_client.yml`

```yaml
# Remote commands
mqtt_bridge:
  mqtt_host: localhost
  mqtt_port: 1883
  mqtt_autostart: true
```

</details>


### Command Shortcuts

![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) :warning: **This isn't yet implemented.**

<details>
<summary>Expand ...</summary>

Command shortcuts can be defined in Hummingbot's `conf_global.yml`, for more information see here: https://docs.hummingbot.io/operation/config-files/#create-command-shortcuts

</details>

___

## Test Connection

<details>
<summary>Test Connection ...</summary>

To test basic connection, use any MQTT client and connect to the same broker as RogerThat, then subscribe to the `rogerthat/#` topic.

There is also a small python script in the `examples/` folder which can be used to mimic a TradingView alert. You'll then see the MQTT message if you subscribe to your chosen topic.

</details>

___

# License
[MIT](https://choosealicense.com/licenses/mit/)