Shares not restarting on CentOS
===============================

If you notice that shares are not restarting for users other than root on Red Hat or one of its derivatives (CentOs, Scientific Linux, ...), you should remove following line from /etc/sudoers

.. code-block::

    defaults requiretty

The reason is that openport will sudo into the user to restart its shares, but cannot because of this rule.

The line is completely save to remove, as is stated
`here <https://bugzilla.redhat.com/show_bug.cgi?id=1020147>`_.