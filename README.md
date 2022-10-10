# Ansible Quota Role

This role currently only works with users quotas, not groups or projects, using aquota, quota version 2 on non-XFS filesystems, support for journald quotas should be added in the future.

This role is designed to configure one directory with users quotas, the `quota_dir` has to be a partition and this role will fail if it isn't, it defaults to `/home`.

You can set users quotas by using the [users role](https://git.coop/webarch/users), the tasks for this in that role should be moved to this role in the future.

## Defaults

See the [defaults/main.yml](defaults/main.yml) file for the default settings.

| Variable             | Default value                                                                                   | Comments                                            |
|----------------------|-------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| quota                | `true`                                                                                          | Run the tasks in this role.                         |
| quota_dir            | `/home`                                                                                         | The directory that quotas are set below.            |
| quota_name           | `"{{ quota_dir | regex_replace('^\/') | regex_replace('\/', '_') | regex_replace(' ', '_') }}"` | The name for the quota directory.                   |
| quota_grace_block    | `86400`                                                                                         | The quota grace time for blocks.                    |
| quota_grace_inode    | `86400`                                                                                         | The quota grace time for inodes.                    |
| quota_repquota       | `/usr/sbin/repquota`                                                                            | The path to repquota.                               |
| quota_quotatool      | `/usr/sbin/quotatool`                                                                           | The path to quotatool.                              |
| quota_tune2fs        | `/sbin/tune2fs`                                                                                 | The path to tune2fs.                                |
| quota_warn           | `true`                                                                                          | Warn when quota are reached.                        |
| quota_warn_from      | `root@localhost`                                                                                | From address for the quota warning emails.          |
| quota_warn_subject   | `Over quota`                                                                                    | Subject line for the quota warning emails.          |
| quota_warn_cc        | `root@localhost`                                                                                | CC address for the quota warning emails.            |
| quota_warn_support   | `root@localhost`                                                                                | Support email address for the quota warning emails. |
| quota_warn_phone     |                                                                                                 | Phone number for the quota warning emails.          |
| quota_warn_message   |                                                                                                 | Message for the quota warning emails.               |
| quota_warn_signature |                                                                                                 | Signature for the quota warning emails.             |

## Tags and check mode

The first time this role is run it needs to not be in check mode and as a minimum the `quota_req`, quota requirememts tag should be used so that the tasks in [tasks/requirements.yml](tasks/requirements.yml) are run.

Once the requirements are in place then the role can be run in check mode.

To only run the checks you can use the `quota_check` tag.
