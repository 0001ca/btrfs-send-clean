```markdown
# Btrfs Timeline Backup Script

This bash script is designed to create a timeline backup of Btrfs subvolumes by sending the differences from a base subvolume to a specified host server or laptop. It is intended for use with cron jobs to automate the backup process.

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Crontab Example](#crontab-example)

## Features

- Creates timeline backups of Btrfs subvolumes.
- Supports sending differences to a remote host server or laptop.
- Automated backup scheduling with cron jobs.
- Highly customizable for various subvolumes and backup destinations.

## Requirements

- Btrfs filesystem must be in use on your system.
- The `btrfs-send-clean.sh` script should be located at `/usr/local/bin/`.
- Properly configured SSH access to the host server or laptop (if sending backups remotely).

## Installation

1. Copy the `btrfs-send-clean.sh` script to `/usr/local/bin/`:
   ```bash
   sudo cp path/to/btrfs-send-clean.sh /usr/local/bin/
   ```

2. Make sure the script is executable:
   ```bash
   sudo chmod +x /usr/local/bin/btrfs-send-clean.sh
   ```

3. Ensure that your Btrfs subvolumes are properly set up.

## Usage

To create timeline backups of your Btrfs subvolumes, you can use the following command:

```bash
btrfs-send-clean.sh BASE_SNAPSHOT SOURCE_PATH TIMELINE_SNAPSHOT SUBVOLUME_NAME DAYS_TO_KEEP
```

- `BASE_SNAPSHOT`: The base snapshot to use as a reference.
- `SOURCE_PATH`: The source path of the subvolume to back up.
- `TIMELINE_SNAPSHOT`: The destination for the timeline backup.
- `SUBVOLUME_NAME`: A name for the subvolume being backed up.
- `DAYS_TO_KEEP`: The number of days to keep the backup before pruning old backups.

## Crontab Example

You can automate backups by adding the following lines to your crontab configuration. Edit the crontab using `crontab -e`:

```bash
# Example crontab entries for Btrfs timeline backups

# Daily backups at midnight
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/rootfs_2023-09-08_13:57:08 / /mnt/btrfs/snapshot/timeline rootfs 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/home_2023-09-08_13:57:08 /home /mnt/btrfs/snapshot/timeline home 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/files_2023-09-08_13:57:08 /mnt/files /mnt/btrfs/snapshot/timeline files 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/distfiles_2023-09-08_13:57:08 /var/cache/distfiles /mnt/btrfs/snapshot/timeline distfiles 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/binpkgs_2023-09-08_13:57:08 /var/cache/binpkgs /mnt/btrfs/snapshot/timeline binpkgs 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/portage_2023-09-08_13:57:08 /var/db/repos/gentoo /mnt/btrfs/snapshot/timeline portage 31
@midnight /bin/sh /usr/local/bin/btrfs-send-clean.sh /mnt/btrfs/snapshot/base/images_2023-09-08_13:57:08 /var/lib/libvirt/images /mnt/btrfs/snapshot/timeline images 31
```

Make sure to customize the paths and backup settings according to your system's configuration.

Now, your Btrfs subvolumes will be automatically backed up on a daily basis.
```

Feel free to modify the README as needed to match your specific script and system configuration. This provides a clear and detailed guide for users to understand how to use your script and set up automated backups using cron jobs.
