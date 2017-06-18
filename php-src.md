# Compiling PHP

`git clone https://github.com/php/php-src.git`

`cd php-src`

`git checkout tags/php-7.1.4` change tag to latest

`./buildconf --force`

Either run `./configure` with options or use a `./config.nice` like below:

```
#! /bin/sh
#
# Created by configure

'./configure' \
'--prefix=/usr/local/php714' \
'--with-config-file-scan-dir=/usr/local/php714/etc/conf.d' \
'--without-pear' \
'--enable-bcmath' \
'--with-bz2' \
'--with-gettext' \
'--with-gd' \
'--with-jpeg-dir=/usr' \
'--with-png-dir=/usr' \
'--with-mhash' \
'--enable-calendar' \
'--enable-intl' \
'--enable-exif' \
'--enable-zip' \
'--enable-dba' \
'--enable-ftp' \
'--enable-mbstring' \
'--enable-libxml' \
'--enable-libgcc' \
'--enable-mysqlnd' \
'--with-mysql-sock=/var/run/mysqld/mysqld.sock' \
'--with-mysqli=mysqlnd' \
'--with-pdo-mysql=mysqlnd' \
'--with-kerberos' \
'--with-openssl' \
'--enable-pcntl' \
'--enable-shmop' \
'--enable-soap' \
'--enable-sockets' \
'--enable-sysvmsg' \
'--enable-sysvsem' \
'--enable-sysvshm' \
'--enable-wddx' \
'--enable-xmlreader' \
'--enable-json' \
'--enable-phar' \
'--enable-dom' \
'--with-xsl' \
'--with-pspell' \
'--with-zlib' \
'--with-zlib-dir=/usr' \
'--with-readline' \
'--with-curl' \
'--enable-fpm' \
'--with-fpm-user=www-data' \
'--with-fpm-group=www-data' \
'--enable-debug' \
'--enable-maintainer-zts' \
"$@"
```
and permit x `chmod +x config.nice` before running `./config.nice`

`make`

`make install`

#### Create config files

`nano /usr/local/php714/etc/php-fpm.d/www.conf`

```
[www]

user = www-data
group = www-data

listen = /var/run/php714-fpm/php714-fpm.sock

listen.owner = www-data
listen.group = www-data

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3
```

`cp php.ini-production /usr/local/php714/lib/php.ini`

`nano /usr/local/php714/etc/php-fpm.conf`

```
[global]

pid = /var/run/php714-fpm.pid
error_log = /var/log/php714-fpm.log

include=/usr/local/php714/etc/php-fpm.d/*.conf
```

`mkdir /usr/local/php714/etc/conf.d/`

`nano /usr/local/php714/etc/conf.d/modules.ini`

```
# Zend OPcache
zend_extension=opcache.so

# PThreads v3
#extension=pthreads.so

# APCu
#extension=apcu.so

# php-uv
extension=uv.so
```

---

Add php-fpm to init.d `nano /etc/init.d/php714-fpm`

```
#!/bin/sh
### BEGIN INIT INFO
# Provides:          php714-fpm
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts php714-fpm
# Description:       Starts The PHP FastCGI Process Manager Daemon
### END INIT INFO

# Author: Ondrej Sury <ondrej@debian.org>
# Adjusted for PHP7 by Kaspars Dambis <hi@kaspars.net>

PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/local/php714/sbin
DESC="PHP7 FastCGI Process Manager"
NAME=php714-fpm
DAEMON=/usr/local/php714/sbin/php-fpm
CONFFILE=/usr/local/php714/etc/php-fpm.conf
DAEMON_ARGS="--daemonize --fpm-config $CONFFILE"
CONF_PIDFILE=$(sed -n 's/^pid[ =]*//p' $CONFFILE)
PIDFILE=${CONF_PIDFILE:-/var/run/php714-fpm.pid}
TIMEOUT=30
SCRIPTNAME=/etc/init.d/$NAME

# Exit if the package is not installed
[ -x "$DAEMON" ] || exit 0

# Read configuration variable file if it is present
[ -r /etc/default/$NAME ] && . /etc/default/$NAME

# Load the VERBOSE setting and other rcS variables
. /lib/init/vars.sh

# Define LSB log_* functions.
# Depend on lsb-base (>= 3.0-6) to ensure that this file is present.
. /lib/lsb/init-functions

# Don't run if we are running upstart
if init_is_upstart; then
    exit 1
fi

#
# Function to check the correctness of the config file
#
do_check()
{
    # Run php-fpm with -t option to check the configuration file syntax
    errors=$($DAEMON --fpm-config $CONFFILE -t 2>&1 | grep "\[ERROR\]" || true);
    if [ -n "$errors" ]; then
        echo "Please fix your configuration file..."
        echo $errors
        return 1
    fi
    return 0
}

#
# Function that starts the daemon/service
#
do_start()
{
	# Return
	#   0 if daemon has been started
	#   1 if daemon was already running
	#   2 if daemon could not be started
	start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON --test > /dev/null \
		|| return 1
	start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON -- \
		$DAEMON_ARGS 2>/dev/null \
		|| return 2
	# Add code here, if necessary, that waits for the process to be ready
	# to handle requests from services started subsequently which depend
	# on this one.  As a last resort, sleep for some time.
}

#
# Function that stops the daemon/service
#
do_stop()
{
	# Return
	#   0 if daemon has been stopped
	#   1 if daemon was already stopped
	#   2 if daemon could not be stopped
	#   other if a failure occurred
	start-stop-daemon --stop --quiet --retry=QUIT/$TIMEOUT/TERM/5/KILL/5 --pidfile $PIDFILE --name $NAME
	RETVAL="$?"
	[ "$RETVAL" = 2 ] && return 2
	# Wait for children to finish too if this is a daemon that forks
	# and if the daemon is only ever run from this initscript.
	# If the above conditions are not satisfied then add some other code
	# that waits for the process to drop all resources that could be
	# needed by services started subsequently.  A last resort is to
	# sleep for some time.
	start-stop-daemon --stop --quiet --oknodo --retry=0/30/TERM/5/KILL/5 --exec $DAEMON
	[ "$?" = 2 ] && return 2
	# Many daemons don't delete their pidfiles when they exit.
	rm -f $PIDFILE
	return "$RETVAL"
}

#
# Function that sends a SIGHUP to the daemon/service
#
do_reload() {
	#
	# If the daemon can reload its configuration without
	# restarting (for example, when it is sent a SIGHUP),
	# then implement that here.
	#
	start-stop-daemon --stop --signal USR2 --quiet --pidfile $PIDFILE --name $NAME
	return 0
}

case "$1" in
    start)
	[ "$VERBOSE" != no ] && log_daemon_msg "Starting $DESC" "$NAME"
	do_check $VERBOSE
	case "$?" in
	    0)
		do_start
		case "$?" in
		    0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
		    2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
		esac
		;;
	    1) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
	esac
	;;
    stop)
	[ "$VERBOSE" != no ] && log_daemon_msg "Stopping $DESC" "$NAME"
	do_stop
	case "$?" in
		0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
		2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
	esac
	;;
    status)
        status_of_proc "$DAEMON" "$NAME" && exit 0 || exit $?
        ;;
    check)
        do_check yes
	;;
    reload|force-reload)
	log_daemon_msg "Reloading $DESC" "$NAME"
	do_reload
	log_end_msg $?
	;;
    reopen-logs)
	log_daemon_msg "Reopening $DESC logs" $NAME
	if start-stop-daemon --stop --signal USR1 --oknodo --quiet \
	    --pidfile $PIDFILE --exec $DAEMON
	then
	    log_end_msg 0
	else
	    log_end_msg 1
	fi
	;;
    restart)
	log_daemon_msg "Restarting $DESC" "$NAME"
	do_stop
	case "$?" in
	  0|1)
		do_start
		case "$?" in
			0) log_end_msg 0 ;;
			1) log_end_msg 1 ;; # Old process is still running
			*) log_end_msg 1 ;; # Failed to start
		esac
		;;
	  *)
	  	# Failed to stop
		log_end_msg 1
		;;
	esac
	;;
    *)
	echo "Usage: $SCRIPTNAME {start|stop|status|restart|reload|force-reload}" >&2
	exit 1
    ;;
esac

:

```
`chmod +x /etc/init.d/php714-fpm`

`update-rc.d php714-fpm defaults`

`ln -s /usr/local/php714/bin/php /usr/local/bin/php714`

`chmod +x /usr/local/bin/php714`

`mkdir /var/run/php714-fpm`

`chown -R www-data:www-data /var/run/php714-fpm`

`service php714-fpm start`

---

Do the nginx magic, mainly it is having the below directive on sites-* :

`fastcgi_pass unix:/var/run/php714-fpm/php714-fpm.sock;`

Then `service nginx start`.

Profit!
