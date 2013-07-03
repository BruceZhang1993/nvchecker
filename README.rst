**nvchecker** (short for *new version checker*) is for checking if a new version of some software has been released.

nvchecker is now **in development**.

Dependency
==========
- Python 3
- Tornado
- All commands used in your configuration files

Running
=======
To see available options::

  ./nvchecker --help

Run with one or more configuration files::

  ./nvchecker config_file_1 config_file_2 ...

You normally will like to specify some "version files"; see below.

Version Files
=============
Version files record which version of the software you know or is available. They are simple key-value pairs of ``(name, version)`` seperated by ``:`` ::

  fcitx: 4.2.7
  google-chrome: 27.0.1453.93-200836
  vim: 7.3.1024

Say you've got a version file called ``old_ver.txt`` which records all your watched software and their versions. To update it using ``nvchecker``::

  ./nvchecker --oldverfile=old_ver.txt --verfile=new_ver.txt config.ini

Compare the two files for updates (assuming they are sorted alphabetically; files generated by ``nvchecker`` are already sorted)::

  comm -13 old_ver.txt new_ver.txt
  # or say that in English:
  comm -13 old_ver.txt new_ver.txt | sed 's/:/ has updated to version/;s/$/./'

Configuration Files
===================
The configuration files are in ini format. *Section names* is the name of the software. Following fields are used to tell nvchecker how to determine the current version of that software.

See ``sample_config.ini`` for an example.

Search in a Webpage
-------------------
Search through a specific webpage for the version string. This type of version finding has these fields:

url
  The URL of the webpage to fetch.

encoding
  (*Optional*) The character encoding of the webpage, if ``latin1`` is not appropriate.

regex
  A regular expression used to find the version string.

  It can have zero or one capture group. The capture group or the whole match is the version string.

  When multiple version strings are found, the maximum of those is chosen.

proxy
  The HTTP proxy to use. The format is ``host:port``, e.g. ``localhost:8087``.

Find with a Command
-------------------
Use a shell command line to get the version. The output is striped first, so trailing newlines do not bother.

cmd
  The command line to use. This will run with the system's standard shell (e.g. ``/bin/sh``).

Check AUR
---------
Check `Arch User Repository <https://aur.archlinux.org/>`_ for updates.

aur
  The package name in AUR. If empty, use the name of software (the *section name*).

Check GitHub
------------
Check `GitHub <https://github.com/>`_ for updates. The version returned is in date format ``%Y%m%d``, e.g. ``20130701``.

github
  The github repository, with author, e.g. ``lilydjwg/nvchecker``.

Other
-----
More to come. Send me a patch or pull request if you can't wait and have written one yourself :-)

Bugs
----
* Finish writing results even on Ctrl-C or other interruption.