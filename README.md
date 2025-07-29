# Data Tailor Ansible Role

This repository contains a configuration template 
(i.e. an [Ansible Role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)) 
to customize your environment in the
[European Weather Cloud (EWC)](https://europeanweather.cloud/).
The template is designed to:
* Configure pre-existing virtual machines such that they:
  * Are able to run [EUMETSAT Data Tailor standalone](https://user.eumetsat.int/resources/user-guides/data-tailor-standalone-guide) and [EUMETSAT Data Access
  Client (EUMDAC)](https://pypi.org/project/eumdac/).


## Copyright and License
>💡 No dependencies are distributed as part of this repository.

See the [LICENSE](./LICENSE) file for licensing information as it pertains to
files in this repository.

## Usage

The step-by-step described below assume your local file system follows the 
example structure below, with `ewc-ansible-role-data-tailor` being a clone of this
repository:
```
.
├── roles
│   └── ewc-ansible-role-data-tailor
├── inventory.yml
└── playbook.yml
```

### 1. Specify the target host and SSH credentials
Create an inventory file to specify address/credentials that Ansible should use
to reach the virtual machine you wish to configure:
```yaml
# inventory.yml
---
ewcloud:
  hosts:
    data_tailor:
      ansible_python_interpreter: /usr/bin/python3
      ansible_host: <add the IPV4 address of the target host>
      ansible_ssh_private_key_file: <add the path to local SSH RSA private key file>
      ansible_user: <add the username which owns the SSH RSA private key >
```
### 2. Customize the template

Edit input values for the template [variables](./vars/main.yml) as needed (see
[Inputs](#inputs) section for details).
Then, proceed to create an Ansible Playbook file to load your customizations: 

```yaml
# playbook.yml
---
- name: Data Tailor Library Item Automation Script 
  hosts: data_tailor
  become: true
  become_user: root
  become_method: ansible.builtin.sudo

  roles:
    - ewc-ansible-role-data-tailor
```

### 3. Apply the template

You can apply changes on the target host by running:
```bash
ansible-playbook -i inventory.yml playbook.yml
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| data_tailor_env_wipe | flag to delete existing conda environment where data tailor was previously installed. Only `yes` will be accepted to approve | `string` | n/a | yes |
| data_tailor_env_name | name of conda environment where data tailor will be installed. Example: `epct-desktop` | `string` | n/a | yes |
| conda_prefix | prefix where conda will be installed. Example: `/opt/conda` | `string` | n/a | yes |
| conda_user | user that will own the conda installation. Example: `root` | `string` | n/a | yes |

## Final Environment

Applying this template will trigger the installation of the following 
open-source packages onto your desired target host:

| Name | Version | License | Package Info |
|------|---------|---------|--------------|
| python | 3.9 | PSF | https://docs.python.org/3/license.html |
| epct | 3.5 | Apache License 2.0  | https://anaconda.org/eumetsat/epct |
| epct_webui | 3.5 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_webui |
| epct_restapi | 3.5 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_restapi |
| epct_plugin_gis | 3.4 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_plugin_gis |
| msg-gdal-driver | 0.4 | Apache License 2.0  | https://anaconda.org/eumetsat/msg-gdal-driver |
| epct_plugin_ncarrays | 1.2 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_plugin_ncarrays |
| epct_plugin_netcdf_generator | 3.2 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_plugin_netcdf_generator |
| epct_plugin_umarf | 3.3 | Apache License 2.0  | https://anaconda.org/eumetsat/epct_plugin_umarf |
| eumdac | 3.0 | MIT | https://anaconda.org/eumetsat/eumdac |

## Changelog
All notable changes (i.e. fixes, features and breaking changes) are documented 
in the [CHANGELOG.md](./CHANGELOG.md).

## Contributing

Thanks for taking the time to join our community and start contributing!
Please make sure to:
* Familiarize yourself with our [Code of Conduct](./CODE_OF_CONDUCT.md) before 
contributing.
* See [CONTRIBUTING.md](./CONTRIBUTING.md) for instructions on how to request 
or submit changes.

## Authors

[European Weather Cloud](http://support.europeanweather.cloud/) 
<[support@europeanweather.cloud](mailto:support@europeanweather.cloud)>
