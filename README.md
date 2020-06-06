# Ansible Role: `sweet_home_3d`

Installs or uninstalls [Sweet Home 3D](http://www.sweethome3d.com/) - a free
interior design application that helps you draw the plan of your house, arrange
furniture on it and visit the results in 3D. Creates or removes XDG desktop
file to launch the application.

## Requirements

- A recent version of Ansible. Tested on 2.9. It might work on previous versions.
- Fedora 31. It might work on other versions.
- Access to an archive of Sweet Home 3D release. Either URL to download or path
  to a file.
- Packages `desktop-file-utils` and `rsync` installed or role installs them
  from configured repositories which might connect to internet.

## Role Variables

All variables which can be overridden are stored in
[defaults/main.yml](defaults/main.yml) file and listed in the table bellow as
well.

| Name                                   | Default Value | Description  |
| -------------------------------------- | ------------- | ------------ |
| `sweet_home_3d_archive_src`            | ""            | Path to an release archive of Sweet Home 3D on a control host or URL to download the archive from. |
| `sweet_home_3d_state`                  | present       | By default installs the program. Set to "absent" to uninstall. |
| `sweet_home_3d_top_dest_name_override` | SweetHome3D   | Base name of installation directory and name part of a desktop file. If set to empty, top level of release archive is used instead. |
| `sweet_home_3d_install_parent_dir`     | /opt          | Parent directory in which program will be installed. See also `sweet_home_3d_top_dest_name_override`. |
| `sweet_home_3d_desktop_parent_dir`     | /usr/local/share/applications | Parent directory in which desktop file will be installed. See also `sweet_home_3d_top_dest_name_override`. |
| `sweet_home_3d_force_remove`           | no            | If set to "yes", contents of installation directory and desktop file are not compared against contents of `sweet_home_3d_archive_src` when uninstall, upgrade or downgrade. |

## Example Playbook

### Install

To install the program, define URL to download Sweet Home 3D release archive
from the internet. The easiest way is to use latest URL:

```yaml
- hosts: all
  roles:
    - role: scrool.sweet_home_3d
      vars:
        sweet_home_3d_archive_src: https://sourceforge.net/projects/sweethome3d/files/latest/download
```

There is a caveat in this approach - upgrade won't work automatically.

Use of the URL might end up on a mirror which download speed is rather slow. It
could take couple of minutes to download the archive and ansible playbook might
timeout on receiving data.

### Uninstall

To uninstall, set same values as for install. By default role removes content
only if installed directory and desktop file matches what it would install.
Finally set state variable to "absent".

Here as an example `sweet_home_3d_archive_src` points to an archive downloaded
on the control machine.

```yaml
- hosts: all
  roles:
    - role: scrool.sweet_home_3d
      vars:
        sweet_home_3d_archive_src: /tmp/SweetHome3D-6.3-linux-x64.tgz
        sweet_home_3d_state: absent
```

Alternatively you can enable option `sweet_home_3d_force_remove` and again set
state variable to "absent". In this case role won't need nor even check
`sweet_home_3d_archive_src`. This effectively removes installation dir and
desktop entry file. Example:

```yaml
- hosts: all
  roles:
    - role: scrool.sweet_home_3d
      vars:
        sweet_home_3d_state: absent
        sweet_home_3d_force_remove: yes
```

### Upgrade or downgrade

To upgrade or downgrade you could start with uninstall first and then install
new version. See corresponding sections above.

Alternatively you can enable force remove and set state variable to present
which will effectively ensure application is always installed from the archive.
Example together with the URL to the latest release:

```yaml
- hosts: all
  roles:
    - role: scrool.sweet_home_3d
      vars:
        sweet_home_3d_archive_src: https://sourceforge.net/projects/sweethome3d/files/latest/download
        sweet_home_3d_force_remove: yes
```

## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.

## Author Information

- [Pavol Babinčák](https://github.com/scrool)
