# TPV Auto Lint
Ansible role to create a script that automatically lints all YAML files in the `tpv_mutable_dir` and checks for their existence in Galaxy's `job_conf.yml`. If both applies, it copies the file to the TPV rules directory, updating existing files.
This leads to a automatic reload, if Galaxy watches this directory.
With this script, TPV in Galaxy's job handlers can't break anymore. Which would otherwise lead to mostly undetected destination mapping failures.

## Requirements
 - Ansible >= 2.11  
 - [galaxyproject.galaxy](https://galaxy.ansible.com/galaxyproject/galaxy) for TPV and the `galaxy_*` vars

## Role Variables
See [defaults/main](./default/main.yml) or galaxy's [default/main](https://github.com/galaxyproject/ansible-galaxy/blob/main/defaults/main.yml)

| Variable | Default | Description |
|---|---|---|
| `tpv_configs` | `[]` | List of TPV config file paths in the playbook to deploy (can be templates with `.j2` suffix) |
| `tpv_mutable_dir` | `{{ galaxy_mutable_data_dir }}/total_perspective_vortex` | Directory where TPV config files are staged |
| `tpv_config_dir_name` | `TPV_DO_NOT_TOUCH` | Name of the deployed TPV config directory |
| `tpv_config_dir` | `{{ galaxy_config_dir }}/{{ tpv_config_dir_name }}` | Full path to the deployed TPV config directory |
| `galaxy_job_config_file` | `{{ galaxy_config_dir }}/job_conf.yml` | Path to Galaxy's job configuration file |
| `tpv_privsep` | `false` | If `true`, sets ownership of copied files to `root:<galaxy_user_group>` |
| `tpv_dry_run` | `false` | If `true`, runs `tpv dry-run` before copying. Aborts if any dry-run fails. Requires `galaxy_job_config` to be set (the job config dict), which is written to a temp file since `tpv dry-run --job-conf` requires a standalone file with `runners:` at the root. |
| `tpv_dry_run_all_tools` | `false` | If `false` (default), only runs dry-run for tools that are new in the staged configs compared to what is deployed. If `true`, runs dry-run for every non-abstract tool in all local and remote configs. |
| `tpv_dry_run_remote_configs` | `[]` | List of remote TPV config URLs (e.g. from `tpv-shared-database`) to include when `tpv_dry_run_all_tools: true`. |
| `tpv_dry_run_user` | `""` | Email of the Galaxy user to impersonate during `tpv dry-run`. Required if your TPV config has a `requires_login` rule (i.e. `if: not user`). |
| `tpv_dry_run_ignore_errors` | `true` | If `true` (default), dry-run failures are non-fatal and the rest of the role continues. Set to `false` to abort on any dry-run failure. |

## Playbook Example
Include role in your Galaxyserver Playbook **after** the galaxyproject.galaxy role (the dirs have to exist already)
## License
GPLv3
## Author Information
[Galaxy Europe](https://galaxyproject.org/eu/)