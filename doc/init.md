Sample init scripts and service configuration for merged
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/merged.service:    systemd service unit configuration
    contrib/init/merged.openrc:     OpenRC compatible SysV style init script
    contrib/init/merged.openrcconf: OpenRC conf.d file
    contrib/init/merged.conf:       Upstart service configuration file
    contrib/init/merged.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three startup configurations assume the existence of a "POINT" user
and group.  They must be created before attempting to use these scripts.

2. Configuration
---------------------------------

At a bare minimum, pointd requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, pointd will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that pointd and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If pointd is run with "-daemon" flag, and no rpcpassword is set, it will
print a randomly generated suitable password to stderr.  You can also
generate one from the shell yourself like this:

bash -c 'tr -dc a-zA-Z0-9 < /dev/urandom | head -c32 && echo'

Once you have a password in hand, set rpcpassword= in /etc/POINT/POINT.conf

For an example configuration file that describes the configuration settings,
see contrib/debian/examples/POINT.conf.

3. Paths
---------------------------------

All three configurations assume several paths that might need to be adjusted.

Binary:              /usr/bin/merged
Configuration file:  /etc/POINT/POINT.conf
Data directory:      /var/lib/merged
PID file:            /var/run/merged/merged.pid (OpenRC and Upstart)
                     /var/lib/merged/merged.pid (systemd)

The configuration file, PID directory (if applicable) and data directory
should all be owned by The Merge user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
POINT user and group.  Access to point-cli and other merged rpc clients
can then be controlled by group membership.

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists on just copying it to
/usr/lib/systemd/system directory, followed by the command
"systemctl daemon-reload" in order to update running systemd configuration.

To test, run "systemctl start merged" and to enable for system startup run
"systemctl enable merged"

4b) OpenRC

Rename merged.openrc to merged and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
"/etc/init.d/merged start" and configure it to run on startup with
"rc-update add merged"

4c) Upstart (for Debian/Ubuntu based distributions)

Drop merged.conf in /etc/init.  Test by running "service merged start"
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon uitility.

4d) CentOS

Copy merged.init to /etc/init.d/merged. Test by running "service merged start".

Using this script, you can adjust the path and flags to the merged program by
setting the POINTD and FLAGS environment variables in the file
/etc/sysconfig/merged. You can also use the DAEMONOPTS environment variable here.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
