## Ansible Quota Role

The `quota_dir` has to be a partition and this role will fail if it isn't, it defaults to `/home`.

See the [defaults/main.yml](defaults/main.yml) file for the default settings.

Set users quotas by using the [users role](https://git.coop/webarch/users):

```yml
  vars:
    users_quota: 1G
```
