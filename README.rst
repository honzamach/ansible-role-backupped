.. _section-role-backupped:

Role **backupped**
================================================================================

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/accounts>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-accounts>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-accounts>`__

Main purpose of this role is to configure and perform regular backups to remote
servers using the `Duplicity <http://duplicity.nongnu.org/>`_ utility. The
`rsync <https://linux.die.net/man/1/rsync>`__ is used as communication protocol
for transfering backups to remote backup server.

**Table of Contents:**

* :ref:`section-role-backupped-installation`
* :ref:`section-role-backupped-dependencies`
* :ref:`section-role-backupped-usage`
* :ref:`section-role-backupped-variables`
* :ref:`section-role-backupped-files`
* :ref:`section-role-backupped-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-backupped-installation:

Installation
--------------------------------------------------------------------------------

To install the role `honzamach.backupped <https://galaxy.ansible.com/honzamach/backupped>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.backupped

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-backupped <https://github.com/honzamach/ansible-role-backupped>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-backupped.git honzamach.backupped

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


.. _section-role-backupped-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is not dependent on any other role.

No other roles have direct dependency on this role.


.. _section-role-backupped-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers]
    your-server

    [servers_backupped]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_backupped
      remote_user: root
      roles:
        - role: honzamach.backupped
      tags:
        - role-backupped

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml

It is recommended to follow these configuration principles:

* Create/edit file ``inventory/group_vars/all/vars.yml`` and within define some sensible
  defaults for all your managed servers::

        # You will probably use the same passphrase for all your backups.
        hm_backupped__duplicity_passphrase: "{{ vault_hm_backupped__duplicity_passphrase }}"

        # You will probably backup to the same server.
        hm_backupped__backup_target: ssh.datastorage.domain.org

        # You will probably backup to the same location.
        hm_backupped__backup_path: /var/backups

* Create/edit :ref:`vault <section-overview-vault>` encrypted file ``inventory/group_vars/all/vault.yml``
  and within store your backup encryption password::

        vault_hm_backupped__duplicity_passphrase: something-so-secret-no1-is-gonna-guess

* Use files ``inventory/host_vars/[your-server]/vars.yml`` to customize settings
  for particular servers. Please see section :ref:`section-role-backupped-variables`
  for all available options. You must for example define backup account name
  or could exclude certain directories from backup::

        hm_backupped__backup_account: bck_your_server
        hm_backupped__backup_excludes:
          - /var/lib/postgresql
          - /var/tmp


.. _section-role-backupped-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_backupped__package_list

    List of packages defined separately for each linux distribution and package manager,
    that MUST be present on target system. Any package on this list will be installed on
    target host. This role currently recognizes only ``apt`` for ``debian``.

    * *Datatype:* ``dict``
    * *Default:* (please see YAML file ``defaults/main.yml``)
    * *Example:*

    .. code-block:: yaml

        hm_backupped__install_packages:
          debian:
            apt:
              - duplicity
              - ...

.. envvar:: hm_backupped__duplicity_passphrase

    Passphrase for backup encryption. Intentionally without default value so that
    user is forced to set it.

    * *Datatype:* ``string``
    * *Default:* ``null``

.. envvar:: hm_backupped__backup_target

    Remote backup server. Intentionally without default value so that the user is
    forced to set it. It should be a hostname or URL.

    * *Datatype:* ``string``
    * *Default:* ``null``

.. envvar:: hm_backupped__backup_path

    Absolute path to backup directory on remote backup server.

    * *Datatype:* ``string``
    * *Default:* ``/var/backups``

.. envvar:: hm_backupped__archive_dir

    Name of the directory to which Duplicity should put backup files (local,
    on backupped server).

    * *Datatype:* ``string``
    * *Default:* ``/var/cache/duplicity``

.. envvar:: hm_backupped__temp_dir

    Working directory for temporary files (local, on backupped server).

    * *Datatype:* ``string``
    * *Default:* ``/var/tmp/``

.. envvar:: hm_backupped__backup_excludes

    List of files/directories excluded from backup process.

    * *Datatype:* ``list of strings``
    * *Default:* ``empty list``


.. envvar:: hm_backupped__cron_backup

    Cron timing specification for backup operation. Default is every day at 02:00am.

    * *Datatype:* ``string``
    * *Default:* ``0 2 * * *``

.. envvar:: hm_backupped__cron_backup_status

    Cron timing specification for backup status operation. Default is every monday at 08:00am.

    * *Datatype:* ``string``
    * *Default:* ``0 8 * * 1``


Built-in Ansible variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:envvar:`ansible_lsb['codename']`

    Linux distribution codename. It is used for :ref:`template customizations <section-overview-role-customize-templates>`.


.. _section-role-backupped-files:

Managed files
--------------------------------------------------------------------------------

.. note::

    This role supports the :ref:`template customization <section-overview-role-customize-templates>` feature.

This role manages content of following files on target system:

* ``/root/host-backup.sh`` *[TEMPLATE]*
* ``/root/host-backup-status.sh`` *[TEMPLATE]*
* ``/root/host-backup-exclude.txt`` *[TEMPLATE]*
* ``/etc/cron.d/host-backup`` *[TEMPLATE]*
* ``/etc/cron.d/host-backup-status`` *[TEMPLATE]*


.. _section-role-backupped-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Honza Mach <honza.mach.ml@gmail.com>
| *Author:* Honza Mach <honza.mach.ml@gmail.com>
| Use of this role is governed by the MIT license, see LICENSE file.
|
