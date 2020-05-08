How to build docker containers with devpi
=========================================

Install devpi server
--------------------

.. code-block:: shell

    pip install devpi

Create local index (required if you want to upload wheels):

.. code-block:: shell

    devpi use http://10.0.31.41:3141
    devpi login root
    devpi index -c root/local bases=root/pypi volatile=False


Launch devpi at startup on Mac OS
---------------------------------

Docker for Mac OS doesn't support host networking. So we have to start server
on IP that is not recognised by container as local (i.e. container itself).

Create an alias IP for loopback interface:

.. code-block:: shell

    sudo cp mac-os/net.devpi.lo0-alias.plist /Library/LaunchDaemons/
    sudo launchctl load /Library/LaunchDaemons/net.devpi.lo0-alias.plist
    sudo launchctl start net.devpi.lo0-alias

Adjust paths to ``devpi-server`` and ``log.txt`` in ``mac-os/net.devpi.plist``
to match your environment and setup a launch agent:

.. code-block:: shell

    cp mac-os/net.devpi.plist ~/Library/LaunchAgents/
    launchctl load ~/Library/LaunchAgents/net.devpi.plist
    launchctl start net.devpi

For other platforms see devpi's
`permanent install docs <https://devpi.net/docs/devpi/devpi/latest/+d/quickstart-server.html#quickstart-server>`_.


Setup environment variables
---------------------------

Add to ``~/.profile``:

.. code-block:: shell

    export PIP_INDEX_URL=http://10.0.31.41:3141/root/local/+simple/
    export PIP_TRUSTED_HOST=10.0.31.41:3141


Build and upload wheels
-----------------------

Simple case (all packages are from index):

.. code-block:: shell

    docker-compose run test devpi-builder /opt/test/requirements.txt $PIP_INDEX_URL

Filter out references to git repositories:

.. code-block:: shell

    docker-compose run test bash -c "devpi-builder <(grep -vF git+ /opt/test/requirements.txt) $PIP_INDEX_URL"
