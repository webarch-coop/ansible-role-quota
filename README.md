# Webarchitects Ansible Quota Role

[![pipeline status](https://git.coop/webarch/quota/badges/master/pipeline.svg)](https://git.coop/webarch/quota/-/commits/master)

This role currently only works with users quotas, not groups or projects, using `aquota`, `quota` version 2 on `ext3` and `ext4` filesystems, support for `journald` quotas should be added in the future.

This role is designed to configure one directory with users quotas, the `quota_dir` has to be a partition and this role will fail if it isn't, it defaults to `/home`.

You can set users quotas by using the [users role](https://git.coop/webarch/users), the tasks for this in that role should be rewritten and moved to this role in the future.

## Tags and check mode

The first time this role is run it needs to not be in check mode and as a minimum the `quota_req`, quota requirememts tag should be used so that the tasks in [tasks/requirements.yml](tasks/requirements.yml) are run.

Once the requirements are in place then the role can be run in `--check` mode.

To only run the checks you can use the `quota_check` tag.

To run everything use the `quota` tag.

## Defaults

| Variable             | Default value                                                                                    | Comments                                            |
|----------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| quota                | `true`                                                                                           | Run the tasks in this role.                         |
| quota_dir            | `/home`                                                                                          | The directory that quotas are set below.            |
| quota_name           | The `quota_dir` with the first `/` removed, subsequent ones and spaces replaced with underscores | The name for the quota directory.                   |
| quota_grace_block    | `86400`                                                                                          | The quota grace time for blocks.                    |
| quota_grace_inode    | `86400`                                                                                          | The quota grace time for inodes.                    |
| quota_repquota       | `/usr/sbin/repquota`                                                                             | The path to repquota.                               |
| quota_quotatool      | `/usr/sbin/quotatool`                                                                            | The path to quotatool.                              |
| quota_tune2fs        | `/sbin/tune2fs`                                                                                  | The path to tune2fs.                                |
| quota_warn           | `true`                                                                                           | Warn when quota are reached.                        |
| quota_warn_from      | `root@localhost`                                                                                 | From address for the quota warning emails.          |
| quota_warn_subject   | `Over quota`                                                                                     | Subject line for the quota warning emails.          |
| quota_warn_cc        | `root@localhost`                                                                                 | CC address for the quota warning emails.            |
| quota_warn_support   | `root@localhost`                                                                                 | Support email address for the quota warning emails. |
| quota_warn_phone     |                                                                                                  | Phone number for the quota warning emails.          |
| quota_warn_message   |                                                                                                  | Message for the quota warning emails.               |
| quota_warn_signature |                                                                                                  | Signature for the quota warning emails.             |

See the [defaults/main.yml](defaults/main.yml) file for the default settings.

## Role repo

The primary URL of this repo is [`https://git.coop/webarch/quota`](https://git.coop/webarch/quota) however it is also [mirrored to GitHub](https://github.com/webarch-coop/ansible-role-quota) and [available via Ansible Galaxy](https://galaxy.ansible.com/chriscroome/quota).

If you use this role please use a tagged release, see [the release notes](https://git.coop/webarch/quota/-/releases).

## Local facts

This role installs three Bash scripts in `/etc/ansible/facts.d/` which generate JSON that is available under `facts_local`, for example for a `/home` directory:

### facts_local.quotatool_home

* [templates/quotatool.fact.j2](templates/quotatool.fact.j2)

```bash
/etc/ansible/facts.d/quotatool_home.fact | yq -P
```
```yaml
state: present
block_grace: 86400
inode_grace: 86400
```

### facts_local.repquota_home

* [templates/repquota.fact.j2](templates/repquota.fact.j2)

```bash
/etc/ansible/facts.d/repquota_home.fact | yq -P
```
```yaml
- User: root
  BlockStatus: ok
  FileStatus: ok
  BlockUsed: "872"
  BlockSoftLimit: "0"
  BlockHardLimit: "0"
  BlockGrace: ""
  FileUsed: "219"
  FileSoftLimit: "0"
  FileHardLimit: "0"
  FileGrace: ""
- User: chris
  BlockStatus: ok
  FileStatus: ok
  BlockUsed: "35420"
  BlockSoftLimit: "0"
  BlockHardLimit: "0"
  BlockGrace: ""
  FileUsed: "805"
  FileSoftLimit: "0"
  FileHardLimit: "0"
  FileGrace: ""
- User: wiki
  BlockStatus: ok
  FileStatus: ok
  BlockUsed: "324080"
  BlockSoftLimit: "4772185"
  BlockHardLimit: "5242880"
  BlockGrace: ""
  FileUsed: "24452"
  FileSoftLimit: "954437"
  FileHardLimit: "1048576"
  FileGrace: ""
```
### facts_local.tune2fs_home

* [templates/tune2fs.fact.j2](templates/tune2fs.fact.j2)

```bash
/etc/ansible/facts.d/repquota_home.fact | yq -P
```
```yaml
Filesystem volume name: <none>
Last mounted on: /home
Filesystem UUID: 5fb78e1a-b214-44e2-a309-8e35116d8dd6
Filesystem magic number: "0xEF53"
'Filesystem revision #': 1 (dynamic)
Filesystem features: has_journal ext_attr resize_inode dir_index filetype needs_recovery extent 64bit flex_bg sparse_super large_file huge_file dir_nlink extra_isize metadata_csum
Filesystem flags: signed_directory_hash
Default mount options: user_xattr acl
Filesystem state: clean
Errors behavior: Continue
Filesystem OS type: Linux
Inode count: "3932160"
Block count: "15728640"
Reserved block count: "786432"
Free blocks: "15067559"
Free inodes: "3842395"
First block: "0"
Block size: "4096"
Fragment size: "4096"
Group descriptor size: "64"
Reserved GDT blocks: "1024"
Blocks per group: "32768"
Fragments per group: "32768"
Inodes per group: "8192"
Inode blocks per group: "512"
Flex block group size: "16"
Filesystem created: Mon Apr  6 15:10:37 2020
Last mount time: Sun Oct  9 21:21:26 2022
Last write time: Sun Oct  9 21:21:26 2022
Mount count: "15"
Maximum mount count: "-1"
Last checked: Fri Apr  8 15:24:22 2022
Check interval: 0 (<none>)
Lifetime writes: 204 GB
Reserved blocks uid: 0 (user root)
Reserved blocks gid: 0 (group root)
First inode: "11"
Inode size: "256"
Required extra isize: "32"
Desired extra isize: "32"
Journal inode: "8"
Default directory hash: half_md4
Directory Hash Seed: 67d5358d-723d-4ce3-b3c0-30ddb433ad9e
Journal backup: inode blocks
Checksum type: crc32c
Checksum: "0xfba01192"
```
