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


.. toctree::
    recipes_automate_installation
    recipes_create_a_forward_tunnel
    recipes_forward_to_another_server
    recipes_forwarding_udp
    recipes_share_a_file
    recipes_testing_facebook_buttons
    recipes_using_openport_with_nodejs
    recipes_blocking_keys
