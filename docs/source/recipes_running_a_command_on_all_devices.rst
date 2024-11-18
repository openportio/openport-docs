Running a command on all devices
==========================

You can use the following bash script to run a command on all devices that have opened port 22. This script uses
the Openport API to get a list of all devices, and then runs a command
on each of them.

The script below will download and install the latest version of the Openport client. You can modify the script to run any command you want on the devices.

To use this script, you will need to replace the ``APITOKEN`` variable with your Openport API token, and the ``keyid`` variable with the key ID of the device you want to run the command on.

This script uses the `jq <https://stedolan.github.io/jq/>`_ command-line JSON processor to parse the JSON output from the Openport API.
You can install jq on most Linux distributions using your package manager.

To avoid having to type your ssh password multiple times, you can use ssh keys to authenticate with the devices.
See `this guide <https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2>`_ for more information on how to set up ssh keys.

.. code-block::


    #!/bin/bash

    APITOKEN=${APITOKEN:-}  # your api token here
    USER=root  # the user to ssh as
    DRYRUN=1  # set to 1 to do a dry run, set to 0 to execute commands

    if [ $DRYRUN -eq 1 ]; then
      echo "Dry run, not executing commands"
      DRYRUN=echo
    else
      DRYRUN=
    fi

    sessions=$(curl -q "https://api.openport.io/api/v2/sessions/?token=$APITOKEN" | jq -c '.results[] | select ( .local_port == 22)')

    for session in $sessions ; do
      echo $session

      link=$(echo $session | jq -r .open_port_for_ip_link)
      port=$(echo $session | jq -r .port)
      server=$(echo $session | jq -r .server)

      downloadlink=https://openport.io/download/debian64/latest.deb

      $DRYRUN curl $link
      $DRYRUN ssh $USER@$server -p $port wget $downloadlink
      $DRYRUN ssh $USER@$server -p $port sudo -H dpkg -i latest.deb
      $DRYRUN ssh $USER@$server -p $port sudo -H reboot
    done


