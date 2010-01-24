After Boot Executor
===================
 
Simple solution for automated tasks execution in Linux.


Rationale
---------

Automated scripts execution just after your system is ready to be used can be
very handy, especially if you want provide more or less _stable_ environment
to your tasks requiring running after boot time. ABE helps you here.


Installation
------------

Copy content of this package (excluding top files, i.e. README.md and any other
files available at this level) to root directory `/`. Add following line to
`/etc/inittab`:

    abe:23:once:/sbin/getty -inl /usr/local/sbin/abe 38400 tty8


Usage
-----

Just put your scripts in `/tasks/pending` directory and make them executable.
Next time you boot up your system, they will be executed in a consecutive
manner (alphabetically ordered). Depending on a returned code, each script will
be moved to appropriate subdirectory of `/tasks` - `success` or `failure`.
During execution you can see the results in eighth console (use Ctrl+Alt+F8 to
show it).

You can apply additional requirements to your scripts by preceding program
loader defined in shebang with path to `abe-run` and condition, which can be:

* _required-kernel-version_, tested against `uname -r`,
* `run-as-first-task`, enforcing reboot if it is not the first executed script.

If _required-kernel-version_ is other than the current running one, ABE looks
for grub entry which title ends with _required-kernel-version_ text and reboots
into it. If such entry does not exist, script will be moved to `/tasks/skipped`
directory.

### Shebang example ###

    #!/usr/local/bin/abe-run 2.6.32 /bin/sh


Notes
-----

Proper handling of kernel version dependency in scripts, i.e. automatically
rebooting into needed kernel, requires `grub` and strict naming convention
of grub menu entries - they should end with their kernel release text (the one,
which is returned by `uname -r` if you boot into them).

**USE AT YOUR OWN RISK! NO WARRANTY!**


Bugs
----

If you find any bug, then please create new issue in [GitHub][1] and describe
it there.

  [1]: http://github.com/przemoc/abe/issues


Author
------

    Przemysław Pawełczyk <przemoc@gmail.com>
