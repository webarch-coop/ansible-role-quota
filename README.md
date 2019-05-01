## Ansible Quota Role

The `quota_dir` has to be a partition and this role will fail if it isn't, it defaults to `/home`.

You can set these optional variables:

 * quota_dir
 * quota_name
 * quota_grace_block
 * quota_grace_inode
 * quota_warn_from
 * quota_warn_subject
 * quota_warn_cc
 * quota_warn_support
 * quota_warn_phone
 * quota_warn_message
 * quota_warn_signature

Example:

```yml
  vars:
    quota_dir: /home
    quota_name: home
    quota_grace_block: 86400
    quota_grace_inode: 86400
    quota_warn_from: support@webarchitects.coop
    quota_warn_subject: "[webarchitects] Webhosting disk quota usage warning"
    quota_warn_cc: root@localhost
    quota_warn_support: support@webarchitects.coop
    quota_warn_phone: "+44 114 276 9709"
    quota_warn_message: "Hi %i,||You appear to be over your soft limit for disk usage on %h.||There is a danger that your website will start to malfunction as a result of this.||Please either reduce your disk space usage or get in contact to have your limits raised.|"
    quota_warn_signature: " -- | Webarch Co-operative Limited| https://www.webarchitects.coop/| Contact: https://webarch.net/contact| Help:    https://docs.webarch.net/| Status:  https://www.webarch.info/|"
```

Set users quotas by using the [users role](https://git.coop/webarch/users):

```yml
  vars:
    users_quota: 1G
```
