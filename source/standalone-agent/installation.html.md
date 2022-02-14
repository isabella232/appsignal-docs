---
title: "Standalone AppSignal Agent installation"
---

In order to provide a full picture of your infrastructure, you should monitor not just application servers, but any server your application depends on (eg. database servers). In order to do this, we offer a standalone mode in our AppSignal agent. This allows you to run the AppSignal agent without having to start it from a Ruby/Elixir process.

## Installation

We are in the process of testing and packaging the standalone agent on Linux distributions that are most popular with our customers. [Let us know](mailto:support@appsignal.com) if you want to run the agent on a distribution that is not supported yet.

### Debian and Ubuntu

The agent has been tested on Ubuntu LTS 14.04/16.04/18.04/20.04 and Debian 8/9/10.
First make sure the following packages are installed. All the following commands need root permissions, you might have to use `sudo`.

```bash
apt-get install curl gnupg apt-transport-https
```

Then import the GPG key from [Packagecloud](https://packagecloud.io). This is a service we use to host the repositories for us.

```bash
curl -L https://packagecloud.io/appsignal/agent/gpgkey | sudo apt-key add -
```

Create a file named `/etc/apt/sources.list.d/appsignal_agent.list` that contains the repository configuration below for the OS you use.

#### Ubuntu

Make sure to replace `focal` in the config below with your Ubuntu release's name. Use `xenial` for 16.04, `bionic` for 18.04  or `trusty` for 14.04.

```bash
deb https://packagecloud.io/appsignal/agent/ubuntu/ focal main
deb-src https://packagecloud.io/appsignal/agent/ubuntu/ focal main
```

#### Debian

Make sure to replace `buster` in the config below with your Debian release's name. Use `stretch` for 9 and `jessie` for 8.

```bash
deb https://packagecloud.io/appsignal/agent/debian/ buster main
deb-src https://packagecloud.io/appsignal/agent/debian/ buster main
```

Then run `apt update` to get the newly added packages and install the agent.

```bash
apt-get update
apt-get install appsignal-agent
```

The agent has now been installed. Next up is configuring it to report to the correct app in AppSignal.

### Redhat Enterprise Linux/Centos

At the moment EL version 7 and 8 are supported. First make sure the following packages are installed. All the following commands need root permissions, you might have to use `sudo`.

```bash
yum install pygpgme yum-utils
```

Create a file named `/etc/yum.repos.d/appsignal_agent.repo` that contains the repository configuration below for EL7:

```
[appsignal_agent]
name=appsignal_agent
baseurl=https://packagecloud.io/appsignal/agent/el/7/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/appsignal/agent/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```

Or this config for EL8:

```
[appsignal_agent]
name=appsignal_agent
baseurl=https://packagecloud.io/appsignal/agent/el/8/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/appsignal/agent/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```

Update your local yum cache by running:

```bash
yum -q makecache -y --disablerepo='*' --enablerepo='appsignal_agent'
```

And you can then install the agent:

```bash
yum install appsignal-agent
```

## Configuration

The standalone agent configuration can be found at `/etc/appsignal-agent.conf`. For a list of all the available options, see the [configuration options](/standalone-agent/configuration/options.html) page for the standalone agent.

The required Push API key can be found in the ["Push & Deploy" section](https://appsignal.com/redirect-to/app?to=info) for any app in your organization, and in the ["Add app" wizard](https://appsignal.com/redirect-to/organization?to=sites/new) for your organization.

When configuring the standalone agent, pick an app name and environment that works for you. It can either be a separate app, or you can configure it for an existing app so that it reports as a new host to that app.

```conf
# /etc/appsignal-agent.conf
push_api_key = "<YOUR PUSH API KEY>"
app_name = "<YOUR APP NAME>"
environment = "<YOUR APP ENVIRONMENT>"
hostname = "<YOUR HOST NAME>"
```

For example:

```conf
# /etc/appsignal-agent.conf
push_api_key = "0000-0000-0000-000"
app_name = "My app name"
environment = "production"
hostname = "hostname"
```

Once you edit the configuration file you need to restart the agent.

- On Ubuntu 14.04 use `service appsignal-agent restart`
- On Centos/Redhat and Ubuntu 16.04 and up use `systemctl restart appsignal-agent`

## Collected metrics

The agent collects [host metrics](/metrics/host.html) by default. You can also send [StatsD](/standalone-agent/statsd.html) messages to it that will be processed as custom metrics.

We are eager to get feedback on this, let us know via the chat in the app or [support@appsignal.com](mailto:support@appsignal.com).
