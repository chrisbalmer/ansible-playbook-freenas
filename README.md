# FreeNAS Playbook

This is a work in progress. It covers some basic settings to make sure a FreeNAS install can send you alerts and is monitoring your disk drives properly. It uses the FreeNAS REST API extensively.

This will eventually be converted into a role and also a module to clean up the API calls into something more friendly for Ansible.

## Required Variables

```yaml
ansible_ssh_user: root
```
This is the login user for the API. Unfortunately FreeNAS requires you to use the root account for this. You will need to specify the password via an AWX credential or `-k`.

```yaml
api:
  email: /api/v1.0/system/email/
  services: /api/v1.0/services/services/
  smart_service_settings: /api/v1.0/services/smart/
  users: /api/v1.0/account/users/
```
These are the API paths and are grouped together for easy updating later if they change.

```yaml
email:
  fromemail: "nas@example.com"
  outgoingserver: ops-smtp-1.example.com
  pass: ""
  port: 25
  security: plain
  smtp_auth: false
  user: ""
```
These are the email settings for outgoing email. [FreeNAS Documentation](https://www.ixsystems.com/documentation/freenas/11.2-U4/system.html#email) 

```yaml
services:
  smartd: true
  ssh: true
  cifs: true
```
These are the services you want to make sure are on or off. `True` means it will be verified to be enabled and running. These are example services, the full set of options can be found in the [FreeNAS documenation](https://www.ixsystems.com/documentation/freenas/11.2-U4/services.html).

```yaml
smart_service:
  interval: 30               # How often to check SMART data
  powermode: never           # Power saving mode
  difference: 5              # Alert if the temperature changes more than this
  informational: 40          # Warning temperature alert
  critical: 45               # Critical temperature alert
  email: chris@example.com   # Email to send alerts to
```
This is the SMART hard drive service (`smartd`). [FreeNAS Documentation](https://www.ixsystems.com/documentation/freenas/11.2-U4/services.html#s-m-a-r-t)


```yaml
users:
  root:
    id: 1
        email: chris@example.com
```
The root email address should be set so that you get important emails from the systems. [FreeNAS Documentation](https://www.ixsystems.com/documentation/freenas/11.2-U4/accounts.html#users)

```yaml
network:
  nameserver1: 172.21.14.2
  nameserver2: 172.21.14.4
  nameserver3: 172.21.7.5
```
These are the DNS servers to use for the system.

```yaml
storage:
  scrubs:
    - daymonth: "*"
      dayweek: "7"
      description: ""
      enabled: true
      hour: "02"
      minute: "00"
      month: "*"
      threshold: 6
      volume: "POOL_A"
```
This is a list of scrubs configured for your volumes (pools). This does not include the boot volume.