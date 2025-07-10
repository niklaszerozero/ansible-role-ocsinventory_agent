# Ansible Role: OCS Inventory NG Agent

[![CI](https://github.com/niklaszerozero/ansible-role-ocsinventory_agent/actions/workflows/ci.yml/badge.svg)](https://github.com/niklaszerozero/ansible-role-ocsinventory_agent/actions/workflows/ci.yml)

Installs the OCS Inventory NG Agent.

After the role is run, a `ocsinventory-agent` will be available on the server. You can use `ocsinventory-agent` run an manual inventory.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
ocs_server: http://ocsinventory-ng/ocsinventory
```

The full URL of the OCS Inventory NG server where the agent should report. This usually ends in `/ocsinventory`.

```yaml
ocs_basedir: /var/lib/ocsinventory-agent
```

The working directory where the agent stores temporary or state files.

```yaml
ocs_configdir: /etc/ocsinventory
```

The configuration directory for the agent. Configuration files and certificates will be placed here.

```yaml
ocs_logfile: /var/log/ocsinventory-agent.log
```

The log file path where the agent will write logs.

```yaml
ocs_ssl: false
```

Whether to use SSL Verification when communicating with the OCS server. Set to true to enable SSL.  
Requires `ocs_ca_file` to be set!

```yaml
ocs_ca_file: ""
```

Path to a CA certificate file used for server verification.

```yaml
ocs_ca: "{{ ocs_configdir }}/cacert.pem"
```

Default location where the CA certificate will be copied to on the target system. Only used if ocs_ca_file is set.

```yaml
ocs_tag: ""
```

Optional tag to assign to the machine in OCS Inventory. Can be used for grouping or classification.

```yaml
ocs_nosoftware: 0
```

Set to 1 to disable software inventory. By default, all installed software is reported.

```yaml
ocs_no_ssl_verify: false
```

Set to true to disable SSL certificate verification. Not recommended for production unless using a trusted internal CA and skipping verification intentionally.

```yaml
ocs_prolog_freq: 4
```

Defines how often the agent should contact the server, in hours.  
Setting this value to 0 will disable the cron Job.  
The minute when this job will be executes is randomized to lower the strain on the network.

```yaml
ocs_now: true
```

Set to true executes an inventory task immediately after installation.

### Debian/Ubuntu specific

```yaml
ocs_apt_release: stable
```

The APT release channel to use for the OCS Inventory repository (e.g., stable, oldstable).

```yaml
ocs_apt_repository: "deb http://deb.ocsinventory-ng.org/{{ ansible_distribution | lower }} {{ ocs_apt_release }} main"
```

The full APT repository URL used to install the OCS Inventory agent.

```yaml
ocs_apt_gpg_key: https://deb.ocsinventory-ng.org/pubkey.gpg
```

URL to the GPG key used to verify the APT repository.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: server
  vars_files:
    - vars/main.yml
  roles:
    - role: niklaszerozero.ocsinventory_agent
```

_Inside `vars/main.yml`_:

```yaml
ocs_server: http://ocsinventory-ng/ocsinventory
ocs_ssl: true
ocs_ca_file: "files/cacert.pem"
ocs_prolog_freq: 6
```

## License

MIT

## Author Information

This role was created in 2025 by [Niklas Vlach](https://www.niklas-vlach.com/).
