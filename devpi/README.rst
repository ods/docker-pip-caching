How to build docker containers with devpi
=========================================

Install devpi server
--------------------

.. code-block:: shell

    pip install devpi


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

    export PIP_INDEX_URL=http://10.0.31.41:3141/root/pypi/+simple/
    export PIP_TRUSTED_HOST=10.0.31.41:3141
