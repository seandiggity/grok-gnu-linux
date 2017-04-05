<!-- $theme: gaia -->
# &nbsp;
# grok ==GNU/Linux==

# ![65% center](sandbox/gnu-linux.png)
<!-- Digital security icon -->
##### Task-based learning materials by Sean O'Brien

##### sean@webio.me | sean.obrien@yale.edu

###### <span style="float:right;">[![](sandbox/cc-by-sa.svg)](http://creativecommons.org/licenses/by-sa/4.0/)</span>
<!-- Creative Commons Attribution-ShareAlike -->

---
<!-- *template: invert -->

### CLI Basics &amp; System Administration

These materials were written for a [Debian GNU/Linux](https://debian.org) system or its popular derivative [Ubuntu](https://ubuntu.com) but should be broadly applicable to other [distros](https://en.wikipedia.org/wiki/Linux_distribution).

It's best to have an environment you don't mind messing up, such as a [Live USB](https://duckduckgo.com/?q=debian+live+usb), a [virtual machine](https://duckduckgo.com/?q=debian+install+virtualbox), or some old hardware you can wipe easily.

The goal is to finish the tasks without having to consult the Web or exit the [command line](https://wiki.debian.org/CommandLineInterface) (CLI).

---
<!-- *template: gaia -->

### Before You Begin

* Grab the files from the [git repository](https://github.com/seandiggity/grok-gnu-linux).

* The tasks require superuser access ([sudo](https://wiki.debian.org/sudo) or root).

* Try to go through the tasks in order, without skipping around. They build upon each other.

* The first time you go through, feel free to looks things up.  However, you need to [learn by doing](https://en.wikipedia.org/wiki/Constructionism_(learning_theory)).

* Your username is "GROK" in the examples.

* The tasks use files in _sandbox_ or _sandbox.tar.gz_ Start the tasks inside the _sandbox_ dir, which is best placed in your [home](https://wiki.debian.org/home_directory) ~/ or /home/GROK

---
<!-- *template: gaia -->

### Tips &amp; Tricks

* Make sure you know how to interrupt a command in your terminal emulator (usually CTRL+C).

* Use the up arrow to pull up previous commands from your command history and TAB to autocomplete command, file, and dir names.

* Need help? Try _--help_ after the command in question or _man_ before it. Learn to [RTFM](https://en.wikipedia.org/wiki/RTFM).

* Keyboard shortcuts like paste (SHIFT+INSERT) are different in the terminal emulator. Learn them, but don't become Pete the Repeat Parrot.


---
<!-- *template: invert -->

> <small>I think the major good idea in Unix was its clean and simple interface: open, close, read, and write.</small>
<small><span style="margin-left: 40%;">&mdash; [Ken Thompson](http://ieeexplore.ieee.org/document/762801)</span></small>

![28% center](sandbox/gnu-meditate.png)

> <small>...the Linux philosophy is 'laugh in the face of danger'.  Oops.  Wrong one. 'Do it yourself'. That's it.</small>
<small><span style="margin-left: 40%;">&mdash; [Linus Torvalds](https://groups.google.com/forum/#!msg/linux.dev.kernel/qeeP584Ny08/CM0gZB0L7nQJ)</span></small>

---
<!-- *template: gaia -->

# Ready, Steady, Go!

---
<!-- page_number: true -->

* Clear your terminal emulator's command history.
```
$ history -c
```


* Read your OS version and write it to a file named _distro-version_ in the directory _current-distro_.
```
$ mkdir current-distro
$ cat /etc/lsb-release > current-distro/distro-version
<!-- or, alternatively: -->
$ lsb_release -d > current-distro/distro-version
```

* Change into _current-distro_. What's in it?
```
$ cd current-distro
$ ls
```

---

* Create a hidden file _.hidden_ in _current-distro_. Write a detailed list of _current-distro_ to _.hidden_
```
$ touch .hidden
$ ls -lah > .hidden
```

* View a detailed list of the filesystem root.
```
$ ls -lah /
```

* Append a list of your home directory to _.hidden_
```
$ ls -lah ~/ >> .hidden
<!-- or, alternatively: -->
$ ls -lah /home/GROK >> .hidden
```

---

* Create a user named _rms_. Give _rms_ the password _FreeAsInFreedom_.
```
$ useradd -D
$ sudo useradd rms
$ sudo passwd rms
```

* Create a user named _linus_ with no home directory and the password _MonolithicKernelOrDie_.
```
$ sudo useradd -M linus
$ sudo passwd linus
```

---

* Create the group _sflc_.  Create the directory _/home/sflc_. Give the group _sflc_ read/write permissions on _/home/sflc_.

```
$ sudo groupadd sflc
$ sudo mkdir /home/sflc
$ chown -R :sflc /home/sflc
$ chmod -R g+rw /home/sflc
```

* Create the file _/home/sflc/README_.  Give the owner read/write/execute permissions and both the group and public read/execute permissions.

```
$ touch /home/sflc/README
$ chmod 755 /home/sflc/README
```

---

* Create a user named _eben_ with home directory _/home/sflc_. A group named _eben_ should not be created along with the user. The user _eben_ should expire on New Year's Eve, 2021.
```
$ sudo useradd -e 2021-12-31 -N -d /home/sflc eben
```

* Add _eben_ to the group _sflc_. Give _eben_ the password _GPLorGTFO_. 
```
$ sudo usermod -a -G sflc eben
$ sudo passwd eben
```

---

* Set inactive user accounts to expire in 30 days.

```
$ sudo nano /etc/default/useradd
<!-- or, alternatively: -->
$ sudo vi /etc/default/useradd
<!-- set value -->
INACTIVE=30
```

* Log in as _linus_, say something, then log out.
```
$ su linus
$ echo "I like kernels"
$ exit
```

* Disable the account _linus_.
```
$ sudo passwd -l linus
```

---

* Enable the _root_ account. Set the _root_ password to _DangerZone_.
```
$ sudo passwd -l root
$ sudo passwd root
```

* Log in as _root_, say something, then log out.
```
$ su root
<!-- or, alternatively: -->
$ sudo -i 
$ echo "wow I just pwned the system"
$ exit
```

* Disable the _root_ account.
```
$ sudo passwd -l root
```

---

* Send a message to the user named _rms_
```
$ write rms
```

* Send a message to all users on the system that contains the current operating system version.
```
$ wall < current_distro/distro-version
```

* View the command history, then save it to the file _recent_commands_.
```
$ history
$ history > recent_commands
```

---
<!-- *template: invert -->

# Warning!
## ![60% center](sandbox/hardhat-tux.png)
#### Potential Trouble Ahead
###### These tasks alter fundamental parts of the system, require the machine to shut down or restart, or could change the system in ways that might make it inoperable or otherwise annoy you.
### However, it's really important stuff you should know how to do.
---
* Shut down the system
```
$ sudo poweroff
<!-- or, alternatively: -->
$ sudo shutdown -h now
```

* Shut down the system in 10 mins and send the users a warning message.
```
$ sudo shutdown -h +10 "Giving HAL 10 more mins to live."
```

---

* Reboot the system.
```
$ sudo reboot
<!-- or, alternatively: -->
$ sudo shutdown -r
```

* Reboot in five minutes and warn the users. Then decide to cancel shutdown.
```
$ sudo shutdown -r +5 "Ship is going down in 5, pls save."
$ sudo shutdown -c "Nev mind, sorry for the scare."
```

* Reset the system forcibly.
```
$ sudo reboot -f
```
