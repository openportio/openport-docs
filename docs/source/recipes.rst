Recipes
=======

Share your ssh port:

.. code-block::
    openport 22 --restart-on-reboot

Share your http port:

.. code-block::
    openport 8080 --http-forward -R

Share VNC:

.. code-block::
    openport 5900 -R

Share remote desktop:

.. code-block::
    openport 3389 -R
