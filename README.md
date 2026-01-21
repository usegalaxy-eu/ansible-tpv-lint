# TPV Auto Lint
Ansible role to create a script that automatically lints all relevant YAML files in the `tpv_mutable_dir` that are present in the TPV environment definition in Galaxy's `job_conf.yml`.
The name of the environment is expected to be "tpv_dispatcher"; otherwise, use the `tpv_env_name` variable.
All these YAML files are linted together (requires total-perspective-vortex v3.1.3 or higher), and if the process passes, the files are copied to the TPV rules directory (`tpv_config_dir`), updating existing files.
This leads to an automatic reload if Galaxy watches this directory. With this script, TPV in Galaxy's job handlers can't break anymore. Which would otherwise lead to mostly undetected destination mapping failures.

## Requirements
 - Ansible >= 2.11  
 - [galaxyproject.galaxy](https://galaxy.ansible.com/galaxyproject/galaxy) for TPV and the `galaxy_*` vars

## Role Variables
See [defaults/main](./default/main.yml) or galaxy's [default/main](https://github.com/galaxyproject/ansible-galaxy/blob/main/defaults/main.yml)
## Playbook Example
Include role in your Galaxyserver Playbook **after** the galaxyproject.galaxy role (the dirs have to exist already)
## License
GPLv3
## Author Information
[Galaxy Europe](https://galaxyproject.org/eu/)
