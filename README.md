# Universal DDI Collection for Ansible

This repo hosts the `infoblox.universal_ddi` Ansible Collection. 

The collection provides modules to automate your Universal DDI (DHCP, DNS and IPAM) and Infoblox Cloud objects hosted in the Infoblox Portal.

> **Note:** This collection replaces the legacy [B1DDI collection](https://github.com/infobloxopen/bloxone-ansible). The Universal DDI collection is a complete rewrite of the B1DDI collection and provides a more robust and feature-rich set of modules to manage Infoblox Universal DDI resources.

### Migration from Legacy Modules

We recommend migrating from the legacy B1DDI Collection as it will be deprecated in the near future. Please expect a few breaking changes while migrating from legacy modules to the new collection modules. To help you with the migration process, refer to our comprehensive [Migration Guide](https://github.com/infobloxopen/universal-ddi-ansible/blob/master/docs/MIGRATION.md).

### Plugin Documentation 

The complete information regarding the collection and the supported modules can be found in the [documentation](https://galaxy.ansible.com/ui/repo/published/infoblox/universal_ddi/docs/).

## Installation

**Installing the Collection from Ansible Galaxy**

To install the Universal DDI Collection, you can use the `ansible-galaxy` command line tool:

```bash
ansible-galaxy collection install infoblox.universal_ddi
```

You can also include it in a `requirements.yml` file and install it via `ansible-galaxy collection install -r requirements.yml` using the format:

```yaml
collections:
- name: infoblox.universal_ddi
```

You can also clone the git repository and use the collection directly:

```bash
mkdir -p ~/ansible_collections/infoblox/universal_ddi
git clone https://github.com/infobloxopen/universal-ddi-ansible.git ~/ansible_collections/infoblox/universal_ddi
```

## Requirements

- Required Ansible version: 2.15 or later
- Required Python version: 3.9 or later

The collection also requires the following Python Libraries to be installed:

- requests >= 2.32.0
- [universal-ddi-python-client](https://pypi.org/project/universal-ddi-python-client/) >= 0.1.0

### Installing required libraries and SDK

Installing collection does not install any required third party Python libraries or SDKs. The user needs to install the required Python libraries using following command:

```bash
pip install -r requirements.txt
```

If you are working on developing and/or testing Infoblox Universal DDI collection, you may want to install additional requirements using following command:

```bash
pip install -r test-requirements.txt
```

## Configuration

### Portal URL

The default URL for the Cloud Services Portal is `https://csp.infoblox.com`. If you need to change this, you can pass the `portal_url` as a part of `modules_defaults` in the playbook. For example:

```yaml
module_defaults:
  group/infoblox.universal_ddi.all:
    portal_url: "{{portal_url_value}}"
```

You can also set the URL using the environment variable `INFOBLOX_PORTAL_URL` or `BLOXONE_CSP_URL`.

> **Note:** `BLOXONE_CSP_URL` is deprecated and will be removed in future releases. It is recommended to use `INFOBLOX_PORTAL_URL` instead.


### Authorization

An API key is required to access Infoblox API. You can obtain an API key by following the instructions in the guide for [Configuring User API Keys](https://docs.infoblox.com/space/BloxOneCloud/35430405/Configuring+User+API+Keys).

To use an API key with Infoblox API, you can pass the `portal_key` as a part of `modules_defaults` in the playbook . For example:

```yaml
module_defaults:
  group/infoblox.universal.all:
    portal_key: "{{portal_key_value}}"
```

Alternatively, You can also set the API key using the environment variable `INFOBLOX_PORTAL_KEY` or `BLOXONE_API_KEY` .

> **Note:** `BLOXONE_API_KEY` is deprecated and will be removed in future releases. It is recommended to use `INFOBLOX_PORTAL_KEY` instead.

> **Note:** The API key is a secret and should be handled securely. Hardcoding the API key in your code is not recommended.

## Usage

The following example demonstrates how to use the `infoblox.universal_ddi` collection to create a DNS Auth Zone inside a View.

```yaml
- hosts: localhost
  gather_facts: no
  module_defaults:
    group/infoblox.universal_ddi.all:
      portal_url: "{{portal_url_value}}"
      portal_key: "{{portal_key_value}}"
  tasks:
    - name: Create a DNS View
      infoblox.universal_ddi.dns_view:
        name: "example_view"
        state: present
      register: dns_view

    - name: Create a new DNS Auth Zone
      infoblox.universal_ddi.dns_auth_zone:
        fqdn: "example.com"
        primary_type: "cloud"
        view: "{{ dns_view.id }}"
        state: present
```

Demo usage for each module can be found in the `Examples` section of the module documentation.

## Debugging

### Verbose Logging

Ansible provides increasing levels of verbosity to help with debugging. The verbosity level can be controlled by appending `-v` to the ansible-playbook command.

```bash
ansible-playbook playbook.yml -vvv
```

Adjust the number of `v`'s to modify the verbosity level.

### Using debug module

Ansible's built-in debug module can be used to print variable values or message outputs during playbook execution.
You can use the `debug` module to print the output of a task. For example:

```yaml
- name: Create a DNS View
  infoblox.universal_ddi.dns_view:
    name: "example_view"
    state: present
  register: dns_view
  
- name: Print the output of the DNS View
  ansible.builtin.debug:
    var: dns_view
    verbosity: 2
```

Please refer to the [Ansible Debug Module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/debug_module.html) for more information.

## Release Notes

For detailed information about the latest updates, new features, bug fixes, and improvements, please visit our [Changelog](https://github.com/infobloxopen/universal-ddi-ansible/blob/master/CHANGELOG.rst)

## Contributing

We welcome your contributions to the Universal DDI Collection! Please check out our [Contribution Guide](https://github.com/infobloxopen/universal-ddi-ansible/blob/master/docs/CONTRIBUTING.md) for guidelines on how to submit pull requests, report issues, and contribute to the project.

## Support

If you have any questions or issues, you can reach out to us using the following channels:

- Github Issues:
  - Submit your issues or requests for enhancements on the [Github Issues Page](https://github.com/infobloxopen/universal-ddi-ansible/issues)
- Infoblox Support:
  - For any questions or issues, please contact [Infoblox Support](https://info.infoblox.com/contact-form/).

## Related Information

- Official Infoblox [Documentation](https://docs.infoblox.com/)
- Infoblox API [Documentation](https://csp.infoblox.com/apidoc)
