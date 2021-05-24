# Ansible role Red Hat Satellite managed host

Manage membership of managed host for Red Hat Satellite.

_Please keep in mind, the current ansible-lint warnings can be ignored. The tasks are made idempotent by working around them with conditionals based on an API call to the Red Hat Satellite server._

[![Build Status](https://travis-ci.org/Caseraw/ansible_role_rh_satellite_managed_host.svg?branch=master)](https://travis-ci.org/Caseraw/ansible_role_rh_satellite_managed_host) [<img src="https://img.shields.io/ansible/role/47149">](https://galaxy.ansible.com/caseraw/ansible_role_rh_satellite_managed_host) [<img src="https://img.shields.io/ansible/role/d/47149">](https://galaxy.ansible.com/caseraw/ansible_role_rh_satellite_managed_host) [<img src="https://img.shields.io/ansible/quality/47149">](https://galaxy.ansible.com/caseraw/ansible_role_rh_satellite_managed_host)

- [Ansible role Red Hat Satellite managed host](#ansible-role-red-hat-satellite-managed-host)
  - [License](#license)
  - [Author Information](#author-information)
  - [Requirements](#requirements)
  - [Dependencies](#dependencies)
  - [Compatibility](#compatibility)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [Useful shell commands](#useful-shell-commands)
  - [Additional documentation resources](#additional-documentation-resources)

## License

MIT / BSD

## Author Information

- Made and maintained by: [Kasra Amirsarvari](https://www.linkedin.com/in/caseraw)
- Ansible Galaxy community author: <https://galaxy.ansible.com/caseraw>
- Dockerhub community user: <https://hub.docker.com/u/caseraw>

## Requirements

- Ensure privileged permissions are set for the user executing this role to:
  - Install yum packages.
  - Manage yum repositories.
- Ensure network traffic to the Red Hat Satellite server is allowed.
  - Outbound port 80 and 443.
- Ensure to have access to the Red Hat Satellite server with a service account.
  - Have permissions to manage hosts, for example "Site Manager".
  - Encrypt username and password of the service account.

## Dependencies

N/A

## Compatibility

Compatible with the following list of operating systems:

- CentOS 7
- CentOS 8
- RHEL 7.x
- RHEL 8.x

## Role Variables

| Variable name | Description |
|---------------|-------------|
| rh_satellite_service_account_user | Username of the Red Hat Satellite service account (vault encrypted). |
| rh_satellite_service_account_pass | Password of the Red Hat Satellite service account (vault encrypted). |
| rh_satellite_managed_host_satellite_server_fqdn | The FQDN of the satellite/capsule server. |
| rh_satellite_managed_host_satellite_server_ip | The IP address of the satellite/capsule server. |
| rh_satellite_managed_host_satellite_server_insecure | Whether the certificate is self signed, therefore untrusted. |
| rh_satellite_managed_host_katello_ca_consumer_rpm_url | The URL (http/80) of the katello certificate rpm. |
| rh_satellite_managed_host_organization | The organisation to subscribe to. |
| rh_satellite_managed_host_activation_key | The activation key to use when subscribing. |
| rh_satellite_managed_host_check_membership | Check if host is already registered to the satellite/capsule server. |
| rh_satellite_managed_host_cleanup_prior_registration | Whether to clean up before registering to the satellite/capsule server. |
| rh_satellite_managed_host_force_registration | Whether to force the registration (not idempotent). |
| rh_satellite_managed_host_unregister | Whether to unregister/decommission the registered host. |
| rh_satellite_managed_host_required_packages | A list of required packaged to install. |
| rh_satellite_managed_host_insights_register | Whether to register the host as an insights client. |
| rh_satellite_managed_host_insights_force_registration | Whether to force register the insights client (not idempotent, will create duplicaties if registerred). |
| rh_satellite_managed_host_insights_conf_obfuscate_ip | Whether to obfuscate the host IP address. |
| rh_satellite_managed_host_insights_conf_obfuscate_hostname | Whether to obfuscate the hostname. |
| rh_satellite_managed_host_molecule_dummy | Dummy switch to bypass entire converge playbook. |

## Example Playbook

```yaml
---
- name: Manage membership of the Red Hat Satellite managed host
  become: True
  gather_facts: True
  vars_files:
    - /path/to/vault/satellite.yml
  tasks:
    - import_role:
        name: ansible_role_rh_satellite_managed_host
      vars:
         rh_satellite_managed_host_satellite_server_fqdn: satellite.example.com
         rh_satellite_managed_host_satellite_server_ip: '192.168.122.252'
         rh_satellite_managed_host_satellite_server_insecure: True
         rh_satellite_managed_host_katello_ca_consumer_rpm_url: http://{{ rh_satellite_managed_host_satellite_server_fqdn }}/pub/katello-ca-consumer-latest.noarch.rpm
         rh_satellite_managed_host_organization: Some_Organization
         rh_satellite_managed_host_activation_key: Some_Activation_Key
         rh_satellite_managed_host_check_membership: True
         rh_satellite_managed_host_cleanup_prior_registration: True
         rh_satellite_managed_host_force_registration: False
         rh_satellite_managed_host_unregister: False
         rh_satellite_managed_host_required_packages:
            - katello-agent
            - katello-host-tools
            - katello-host-tools-tracer
            - insights-client
         rh_satellite_managed_host_insights_register: False
         rh_satellite_managed_host_insights_force_registration: False
         rh_satellite_managed_host_insights_conf_obfuscate_ip: True
         rh_satellite_managed_host_insights_conf_obfuscate_hostname: True
         rh_satellite_managed_host_molecule_dummy: False

...
```

## Useful shell commands

```shell
subscription-manager list
subscription-manager status
subscription-manager repos
subscription-manager refresh
subscription-manager clean
```

## Additional documentation resources

The following links provide more information about **Red Hat Satellite** and it's usage.

- <https://access.redhat.com/documentation/en-us/red_hat_satellite>
- <https://access.redhat.com/documentation/en-us/red_hat_insights>
