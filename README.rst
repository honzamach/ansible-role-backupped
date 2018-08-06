.. _section-role-backupped:

Role **backupped**
================================================================================

Ansible role for configuring periodical backup with Duplicity on all target systems.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/accounts>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-accounts>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-accounts>`__


Description
--------------------------------------------------------------------------------

Main purpose of this role is to configure and perform regular backups to remote
hosts using the `Duplicity <http://duplicity.nongnu.org/>`_ utility.

.. note::

    This role requires the :ref:`secure registry <section-overview-secure-registry>` feature.

    This role supports the :ref:`template customization <section-overview-customize-templates>` feature.


Requirements
--------------------------------------------------------------------------------

This role does not have any special requirements.


Dependencies
--------------------------------------------------------------------------------

This role is not dependent on any other role.

No other roles have direct dependency on this role.


Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/root/host-backup.sh``
* ``/root/host-backup-status.sh``
* ``/root/host-backup-exclude.txt``
* ``/etc/cron.d/host-backup``
* ``/etc/cron.d/host-backup-status``


Role variables
--------------------------------------------------------------------------------

There are following internal role variables defined in ``defaults/main.yml`` file,
that can be overriden and adjusted as needed:


.. envvar:: hm_backupped__package_list

    List of role-related packages, that will be installed on target system.

    * *Datatype:* ``string``
    * *Default:* (please see YAML file ``defaults/main.yml``)

.. envvar:: hm_backupped__duplicity_passphrase

    Passphrase for backup encryption, intentionally without default value so that
    user is forced to set it.

    * *Datatype:* ``string``
    * *Default:* ``null``

.. envvar:: hm_backupped__backup_target

    Remote host for backup, intentionally without default value so that user is
    forced to set it.

    * *Datatype:* ``string``
    * *Default:* ``null``

.. envvar:: hm_backupped__backup_path

    Path to backup directory on remote host, intentionally without default value
    so that user is forced to set it.

    * *Datatype:* ``string``
    * *Default:* ``null``

.. envvar:: hm_backupped__archive_dir

    Name of the directory to which Duplicity should put backup files.

    * *Datatype:* ``string``
    * *Default:* ``/var/cache/duplicity``

.. envvar:: hm_backupped__temp_dir

    Working directory for temporary files.

    * *Datatype:* ``string``
    * *Default:* ``/var/tmp/``

.. envvar:: hm_backupped__backup_excludes

    List of files/directories excluded from backup process.

    * *Datatype:* ``list of strings``
    * *Default:* ``empty list``


.. envvar:: hm_backupped__cron_backup

    Cron specification for backup operation. Default is every day at 02:00am.

    * *Datatype:* ``string``
    * *Default:* ``0 2 * * *``

.. envvar:: hm_backupped__cron_backup_status

    Cron specification for backup status operation. Default is every monday at 08:00am.

    * *Datatype:* ``string``
    * *Default:* ``0 8 * * 1``


Usage and customization
--------------------------------------------------------------------------------

This role is (attempted to be) written according to the `Ansible best practices <https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html>`__. The default implementation should fit most users,
however you may customize it by tweaking default variables and providing custom
templates.


Variable customizations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Most of the usefull variables are defined in ``defaults/main.yml`` file, so they
can be easily overridden almost from `anywhere <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`__.


Template customizations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This roles uses *with_first_found* mechanism for all of its templates. If you do
not like anything about built-in template files you may provide your own custom
templates. For now please see the role tasks for list of all checked paths for
each of the template files.


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


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers-backupped]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers-backupped
      remote_user: root
      roles:
        - role: honzamach.backupped
      tags:
        - role-backupped

Example usage::

    ansible-playbook -i inventory playbook.yml


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <honza.mach.ml@gmail.com>
