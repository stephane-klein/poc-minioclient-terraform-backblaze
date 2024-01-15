# Backblaze + terraform + minio client POC

```sh
$ cp .envrc.skel .envrc
```

Edit `.envrc`.

```sh
$ rxt install
$ terraform init
$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # b2_bucket.pgbackrest_backup will be created
  + resource "b2_bucket" "pgbackrest_backup" {
      + account_id  = (known after apply)
      + bucket_id   = (known after apply)
      + bucket_name = "pgbackrest-backup-bucket"
      + bucket_type = "allPrivate"
      + id          = (known after apply)
      + options     = (known after apply)
      + revision    = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

b2_bucket.pgbackrest_backup: Creating...
b2_bucket.pgbackrest_backup: Creation complete after 1s [id=46edf6c8e6fa0bd883d50512]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

```sh
$ mc ls backblaze
[2023-05-12 16:03:23 CEST]     0B ...
[2023-07-16 21:45:02 CEST]     0B ...
[2024-01-15 17:24:08 CET]     0B pgbackrest-backup-bucket/  # <= new bucket
```

```sh
$ mc cp README.md backblaze/pgbackrest-backup-bucket/folder1/
...form-backblaze/README.md: 1.29 KiB / 1.29 KiB ━━━━━━━━━━━━━━━━━ 8.21 KiB/s 0s
```

```sh
$ mc tree backblaze/pgbackrest-backup-bucket/
backblaze/pgbackrest-backup-bucket/
└─ folder1
```

```sh
$ mc ls backblaze/pgbackrest-backup-bucket/folder1/
[2024-01-15 17:28:57 CET] 1.3KiB STANDARD README.md
```

```
$ mc rm --versions -r --force backblaze/pgbackrest-backup-bucket/
Created delete marker `backblaze/pgbackrest-backup-bucket/folder1/README.md` (versionId=4_z46edf6c8e6fa0bd883d50512_f429eb8f66228b517_d20240115_m163102_c003_v7007000_t0000_u01705336262970).
```
