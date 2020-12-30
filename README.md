# auto-update-wsl
Commands and XML for automatically updating WSL on Windows 10

Original [article on RioSec blog](https://www.riosec.com/articles/automatingwindowssubsystemforlinuxupdates)

Each Linux distribution will have their own method for updating; refer to your distribution's documentation. This article will use the Apt system which works with Ubuntu, Debian, and Pengwin, among others.

Below are the updated steps. Note that these should work from both PowerShell or command prompt.

First, list the available Linux distributions on your computer:

    PS > wsl --list
    Windows Subsystem for Linux Distributions:
    docker-desktop-data (Default)
    Ubuntu
    docker-desktop
    WLinux

For the following steps I will be specifying the distribution Ubuntu. These commands work as well for WLinux (Pengwin) just by changing the name. Other distributions may need the commands modified.

To start, enable the default user to run commands as root without entering a password.

`PS > wsl -d Ubuntu /bin/sh -c "echo '%sudo ALL=NOPASSWD:/usr/bin/apt-get' | sudo EDITOR='tee -a' visudo"`

Following that, I will run an update manually to make sure it is working:

`PS > wsl -d Ubuntu /bin/sh -c "sudo apt-get update && sudo apt-get dist-upgrade -y"`

Once that was confirmed to work, I added a scheduled task. You can do this either through the Task Scheduler GUI, or the command line. I used the following command line and XML configuration so that I can reproduce it reliably in the future as needed.

[Task XML definition](Task-LinuxUpgrade.xml)

To schedule the task, run the following:

`PS > schtasks /create /xml Task-LinuxUpgrade.xml /tn UpdateLinux`

Finally, I tested this scheduled task by running it manually:

`PS > schtasks /run /i /tn "UpdateLinux"`

As I mentioned in the original article, Windows Subsystem for Linux is an excellent feature of Windows 10, and hopefully this will help others who want to keep it up to date with security patches.
