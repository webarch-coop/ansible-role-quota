## Ansible Quota Role

This role currently only works with users quotas, not groups or projects, using aquota, quota version 2 on non-XFS filesystems, support for journald quotas should be added in the future.

The `quota_dir` has to be a partition and this role will fail if it isn't, it defaults to `/home`.

See the [defaults/main.yml](defaults/main.yml) file for the default settings.

You set users quotas by using the [users role](https://git.coop/webarch/users), the tasks for this in that role should be moved to this role in the future.
