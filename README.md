galaxyproject.singcvmfs
=======================

An [Ansible][ansible] role for building the `singcvmfs` command from the [cvmfsexec][cvmfsexec] package. `singcvmfs` is a utility that makes it possible to mount [CVMFS][cvmfs] in an [Apptainer (Singularity)][apptainer] container, even if the host does not have CVMFS or a new enough version of FUSE 3 installed. See the [cvmfsexec][cvmfsexec] documentation for details.

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
`singcvmfs_path` | string | Path where `singcvmfs` should be installed
`singcvmfs_local_options` | list of item dictionaries | Variables to set in `/etc/cvmfs/default.local`
`singcvmfs_files` | list of dictionaries | Additional files to install relative to the package `dist/`
`singcvmfs_force_build` | boolean | Force build even if `singcvmfs` already exists

See [the defaults](defaults/main.yml) for a full list of variables and default values.

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: all
  vars:
    singcvmfs_files:
      - src: files/singcvmfs/example.org.pub
        dest: /etc/cvmfs/keys/example.org/example.org.pub
        mode: 0644  # optional, defaults to 0644
    singcvmfs_local_options:
      - key: CVMFS_QUOTA_LIMIT
        value: "-1"
      - key: CVMFS_SHARED_CACHE
        value: "no"
      - key: CVMFS_ALIEN_CACHE
        value: "/cvmfs-cache"
      - key: CVMFS_CLAIM_OWNERSHIP
        value: "yes"
      - key: CVMFS_SERVER_URL
        value: "http://stratum1-east.example.org/cvmfs/@fqrn@;http://stratum1-west.example.org/cvmfs/@fqrn@"
      - key: CVMFS_KEYS_DIR
        value: "/etc/cvmfs/keys/example.org"
      - key: CVMFS_HTTP_PROXY
        value: DIRECT
  roles:
    - galaxyproject.singcvmfs
```

License
-------

MIT

Author Information
------------------

[View contributors on GitHub](https://github.com/galaxyproject/ansible-role-singcvmfs/graphs/contributors)
