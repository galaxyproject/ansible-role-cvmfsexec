galaxyproject.cvmfsexec
=======================

An [Ansible][ansible] role for building the `cvmfsexec` and `cvmfsexec` commands from the [cvmfsexec][cvmfsexec] package. The cvmfsexec utilities assist in performing unprivileged user-namespace [CVMFS][cvmfs] mounts, optionally in an [Apptainer (Singularity)][apptainer] container, even if the host does not have CVMFS or a new enough version of FUSE 3 installed. See the [cvmfsexec][cvmfsexec] documentation for details.

[ansible]: https://www.ansible.com/
[cvmfsexec]: https://github.com/cvmfs/cvmfsexec
[cvmfs]: https://cernvm.cern.ch/fs/
[apptainer]: https://apptainer.org/

Requirements
------------

None

Role Variables
--------------

Variable | Type | Description
--- | --- | ---
`cvmfsexec_path` | string | Path where `cvmfsexec` or `singcvmfs` should be installed (the value of the last path element determines which command is built)
`cvmfsexec_source` | string | RPM source argument to `makedist -s` (default: `default`)
`cvmfsexec_local_options` | list of item dictionaries | Variables to set in `/etc/cvmfs/default.local`
`cvmfsexec_files` | list of dictionaries | Additional files to install relative to the package `dist/`
`cvmfsexec_force_build` | boolean | Force build even if `cvmfsexec` already exists

See [the defaults](defaults/main.yml) for a full list of variables and default values.

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: all
  vars:
    cvmfsexec_files:
      - src: files/cvmfsexec/example.org.pub
        dest: /etc/cvmfs/keys/example.org/example.org.pub
        mode: 0644  # optional, defaults to 0644
      - content: |
          CVMFS_SERVER_URL="http://stratum1-east.example.org/cvmfs/@fqrn@;http://stratum1-west.example.org/cvmfs/@fqrn@"
          CVMFS_KEYS_DIR="/etc/cvmfs/keys/example.org"
        dest: /etc/cvmfs/domain.d/example.org.conf
    cvmfsexec_local_options:
      - key: CVMFS_QUOTA_LIMIT
        value: "-1"
      - key: CVMFS_SHARED_CACHE
        value: "no"
      - key: CVMFS_ALIEN_CACHE
        value: "/scratch/cvmfs-cache"
      - key: CVMFS_CLAIM_OWNERSHIP
        value: "yes"
      - key: CVMFS_HTTP_PROXY
        value: DIRECT
  roles:
    - galaxyproject.cvmfsexec
```

License
-------

MIT

Author Information
------------------

[View contributors on GitHub](https://github.com/galaxyproject/ansible-role-cvmfsexec/graphs/contributors)
