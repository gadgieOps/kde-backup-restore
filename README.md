gadgieOps.kde_backup_restore
=========

This role backups up KDE customizations through config files and restores onto different machines. This is useful for when spinning desktop environments up and down or even as a disaster recovery method. Additional backup directories can be provides and you can also override with your own list of directories.

Limitations
-----------

At this time, this project is limited to backing up and restoring data in the users home folder only. 

Role Variables
--------------

# mode
~~~
mode: backup/restore
~~~
values:
- "backup"
- "restore" 

When set to 'backup', the directories contained in 'backup_directories' and'additional_directories' or 'override_backup_directories' will be backed up.

When set to 'restore', the contents of a backup will be restored to the target system(s)

~~~
backup_dest: "{{ playbook_dir }}"
~~~
Set the destination on the ansible controller where the backup will be located. Defaults to the playbook directory.

~~~
backup_format: gz
~~~
values:
- "gz"
- "bz2"
- "tar"
- "xz"
- "zip"

Sets the compression type.

~~~
backup_name: "kde-config-files-{{ ansible_date_time.iso8601 }}"
~~~
Sets the name of the backup archive. Defaults to a unique name based on the current time. Backups will be overwritten without warning if the file all ready exists.

~~~
additional_directories: []
~~~
A list of additional directories to backup on top of the preset ones. See vars/main.yml for the preset values.

~~~
override_backup_directories: []
~~~
A list of directories to back up. If supplied, the only directories that will be backed up are the ones included in this list.

~~~
restore_file: null
~~~
The archive on the ansible controller that will be unarchived onto the target(s)

Example Playbook
----------------

# Backup
~~~
---
- name: Backup kubuntu KDE config files
  hosts: kubuntu-backup
  become: true
  roles:
    - role: gadgieops.kde_backup_restore
      vars:
        mode: backup
        backup_name: kde_backup
        additional_directories:
          - /home/{{ ansible_user}}/.git*
          - /home/{{ansible_user }}/.bashrc
~~~

# Restore
~~~
- name: Backup kubuntu KDE config files
  hosts: kubuntu-restore
  become: true
  roles:
    - role: gadgieops.kde_backup_restore
      vars:
        mode: restore
        restore_file: kde_backup.gz
~~~

License
-------
MIT

Contributing
------------
Please feel free to contribute to this project by raising issues and opening pull requests.

Author Information
------------------
gadgieOps
https://github.com/gadgieOps

Special Thanks
--------------
Special thank you to @Prayag2 for their konsave project. This project was inspired by the konsave project which is a python based method of backing up and restoring KDE config. Please check it out: https://github.com/Prayag2/konsave