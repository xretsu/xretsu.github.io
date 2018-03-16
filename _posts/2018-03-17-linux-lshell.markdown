---
title:  "Limited Shell a.k.a lshell"
date:   2018-03-17 01:57:00
description: Introduction
---

Assalamulaikum.. nge-thread lagi nih...
kali ini tentang limited shell a.k.a lshell, nah apa itu lshell?

"lshell is a shell coded in Python, that lets you restrict a user's environment to limited sets of commands, choose to enable/disable any command over SSH (e.g. SCP, SFTP, rsync, etc.), log user's commands, implement timing restriction, and more. "

via : https://github.com/ghantoos/lshell

oke lanjut, installation :

{% highlight bash %}
1. Install from source
       # on Linux:
       python setup.py install --no-compile --install-scripts=/usr/bin/
       # on *BSD:
       python setup.py install --no-compile --install-data=/usr/{pkg,local}/
2. On Debian (or derivatives)
       apt-get install lshell
3.  On RHEL (or derivatives)
       yum install lshell
{% endhighlight %}

![alt text](https://image.prntscr.com/image/Yzj0t10RSC2mqUwb9DDQfg.png)

yup cara penggunaan :

Switch User to LShell dengan command chsh atau change shell, ketik sudo chsh nama_user + enter
{% highlight bash %}
┌─[geek|plankton ~]
└─$ sudo chsh geek
Changing the login shell for geek
Enter the new value, or press ENTER for the default
    Login Shell [/usr/bin/lshel]: /usr/bin/lshell
{% endhighlight %}


atau opsi lainnya kita bisa menambahkan user baru yang shell defaultnya menggunakan lshell, nih commandnya :
{% highlight bash %}
$ sudo adduser --shell /usr/bin/lshell nama_user
{% endhighlight %}

Configure LShell :
file konfigurasi lshell ada pada directory /etc/lshell.conf
nih isinya :

{% highlight bash %}
┌─[geek|plankton ~]
└─$ cat /etc/lshell.conf
# lshell.py configuration file
#
# $Id: lshell.conf,v 1.27 2010-10-18 19:05:17 ghantoos Exp $

[global]
##  log directory (default /var/log/lshell/ )
logpath         : /var/log/lshell/
##  set log level to 0, 1, 2, 3 or 4  (0: no logs, 1: least verbose,
##                                                 4: log all commands)
loglevel        : 2
##  configure log file name (default is %u i.e. username.log)
#logfilename     : %y%m%d-%u
#logfilename     : syslog

##  in case you are using syslog, you can choose your logname
#syslogname      : myapp

## include a directory containing multiple configuration files. These files
## can only contain default/user/group configuration. The global configuration will
## only be loaded from the default configuration file.
## e.g. splitting users into separate files
#include_dir     : /etc/lshell.d/*.conf

[default]
##  a list of the allowed commands or 'all' to allow all commands in user's PATH
allowed         : ['ls','echo','cd','ll']

##  a list of forbidden character or commands -- deny vim, as it allows to escape lshell
forbidden       : [';', '&', '|','`','>','<', '$(', '${']

##  a list of allowed command to use with sudo(8)
##  if set to ´all', all the 'allowed' commands will be accessible through sudo(8)
#sudo_commands   : ['ls', 'more']

##  number of warnings when user enters a forbidden value before getting
##  exited from lshell, set to -1 to disable.
warning_counter : 2

##  command aliases list (similar to bash’s alias directive)
aliases         : {'ll':'ls -l', 'vim':'rvim'}

##  introduction text to print (when entering lshell)
#intro           : "== My personal intro ==\nWelcome to lshell\nType '?' or 'help' to get the list of allowed commands"

##  configure your promt using %u or %h (default: username)
#prompt          : "%u@%h"

##  set sort prompt current directory update (default: 0)
#prompt_short    : 0

##  a value in seconds for the session timer
#timer           : 5

##  list of path to restrict the user "geographicaly"
#path            : ['/home/bla/','/etc']

##  set the home folder of your user. If not specified the home_path is set to
##  the $HOME environment variable
#home_path       : '/home/bla/'

##  update the environment variable $PATH of the user
#env_path        : ':/usr/local/bin:/usr/sbin'

##  a list of path; all executable files inside these path will be allowed
#allowed_cmd_path: ['/home/bla/bin','/home/bla/stuff/libexec']

##  add environment variables
#env_vars        : {'foo':1, 'bar':'helloworld'}

##  allow or forbid the use of scp (set to 1 or 0)
#scp             : 1

## forbid scp upload
#scp_upload       : 0

## forbid scp download
#scp_download     : 0

##  allow of forbid the use of sftp (set to 1 or 0)
##  this option will not work if you are using OpenSSH's internal-sftp service
#sftp            : 1

##  list of command allowed to execute over ssh (e.g. rsync, rdiff-backup, etc.)
#overssh         : ['ls', 'rsync']

##  logging strictness. If set to 1, any unknown command is considered as
##  forbidden, and user's warning counter is decreased. If set to 0, command is
##  considered as unknown, and user is only warned (i.e. *** unknown synthax)
strict          : 0

##  force files sent through scp to a specific directory
#scpforce        : '/home/bla/uploads/'

##  history file maximum size
#history_size     : 100

##  set history file name (default is /home/%u/.lhistory)
#history_file     : "/home/%u/.lshell_history"

##  define the script to run at user login
#login_script     : "/path/to/myscript.sh"

Basic Configuration:


[default] Settingan untuk menetapkan nilai default yang diterapkan pada semua pengguna dan grup. Pengaturan bagian ini dapat diganti dengan pengaturan khusus pengguna dan kelompok.
Code:
[default]
##  a list of the allowed commands or 'all' to allow all commands in user's PATH
allowed         : ['ls','echo','cd','ll']

##  a list of forbidden character or commands -- deny vim, as it allows to escape lshell
forbidden       : [';', '&', '|','`','>','<', '$(', '${']
{% endhighlight %}

------skip------

note : dibagian allowed kita bisa menambahkan command yang (bisa) digunakan. jadi hanya perintah yang didaftarkan saja yang bisa digunakan, yang tidak ditambahkan tidak dapat digunakan.

![alt text](https://image.prntscr.com/image/pswPHDzkR-GkIUtZQT2Bcw.png)

atau bisa lihat dsini [ASCIINEMA](https://asciinema.org/a/ZqbniPbF6ym33uQ7PbnCIIxY8)

yarp sebenarnya masih banyak fungsi lshell yang lain, silahkan di explore lebih jauh lagi....
btw kita bisa memanfaatkan lshell untuk default login shell di ssh pada beberapa kondisi seperti untuk soal-soal remote easy CTF dan untuk nge-troll user-user nakal.. asal jangan di troll balik saja... tinggal pakai perintah chsh saja untuk user yang di inginkan...


BONUS :

Bypassing Shell :

{% highlight bash %}
python -c 'import pty; pty.spawn("/bin/sh")'

echo os.system('/bin/bash')

perl —e 'exec "/bin/sh";'

perl: exec "/bin/sh";

ruby: exec "/bin/sh"

lua: os.execute('/bin/sh')

-----------------------------

(From within IRB)

exec "/bin/sh"

-----------------------------
(From within vi)

:!bash

-----------------------------
(From within vi)

:set shell=/bin/bash:shell

-----------------------------
(From within nmap)

!sh
{% endhighlight %}

semoga aja settingan lshell atau other shell bisa di spawn.. :p


Referensi :
*https://packages.debian.org/wheezy/lshell
*https://tecadmin.net/how-to-limit-user-a...ted-shell/
*https://www.aldeid.com/wiki/Lshell
*https://netsec.ws/?p=337
