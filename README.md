afsbotcfg - OpenAFS buildbot master configuration
=================================================

This is the [buildbot master][1] configuration for [OpenAFS][2]. This
configuration is compatible with buildbot version 1.3.

**Note: We are transitioning from the obsolete buildbot version 0.8 to the
current supported version 1.3. This will allow new buildbot 1.x workers to
participate as builders as well as existing 0.8 version build slaves.  The old
master will continue to run during this transition period. New workers should
be configured to use port 9986 to connect to the new master.**

Buildbot master setup
---------------------

The following instructions describe how to use `pip` to install the buildbot
master in a Python virtual environment.  First, with sudo access, install
Python3 and the development packages for it on the system to host the buildbot
master. Ensure TCP ports 9986 and 8011 are open. (These are the ports used
during the transition.) Create a `buildbot` user on the system.  The remaining
steps to not require sudo access and should be run as the `buildbot` user.

Create a Python virtual environment:

    mkdir buildbot13
    cd buildbot13
    python3 -m venv venv
    source venv/bin/activate

Install buildbot and it's dependencies:

    pip install --upgrade pip
    pip install 'buildbot[bundle]'

Create the buildbot master instance:

    buildbot create-master master
    deactivate

Download the buildbot master configuration:

    git clone https://github.com/openafs-contrib/afsbotcfg.git
    cd afsbotcfg
    git checkout buildbot-13x
    cd ..

Create a symlink to the master.cfg file in the master's base directory.

    cd master
    ln -s ../afsbotcfg/master.cfg
    cd ..

Create `settings.ini` file in the master base directory which contains
settings we do not track with git. The settings file contains three
sections:

* local - settings specific to the host
* admins - list of user emails and passwords for authentication
* workers - list of worker names and passwords

Example:

    cat master/settings.ini
    [local]
    buildbotURL = http://buildbot.openafs.org:8011
    
    [admins]
    tycobb@yoyodyne.com = password
    
    [workers]
    example-worker1 = secret1
    example-worker2 = secret2

Check the buildbot master configuration with the command:

    make check

The buildbot master can be started and stopped with:

    make start

Check the log file `master/twisted.log` to verify the buildbot master started.
The master can be stopped with the command:

    make stop

Adding buildbot workers
-----------------------

todo



[1]: http://buildbot.openafs.org:8011
[2]: https://openafs.org
