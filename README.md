# urt-service
Run Urban Terror server using systemd, screen and mail applications on a Linux operating system.

> **Note:** Tested to work on a Debian Wheezy/Jessie operating system, may require alterations for it to work on other operating systems.
> **Note:** It is assumed email is already configured and required packages are installed on the Linux system being used.

## Features

- Urban Terror starts up automatically at system start up (runs as a service/daemon).
- Notification of Urban Terror service failures are sent via email.

## Required Packages

- git
- mailutils (/usr/bin/mail)
- nano
- screen
- systemd

## Sending an Email When a Unit Fails Works as Follows...

- Two files will be required to achieve this: an executable for sending the email and a .service for starting the executable.
- Add "OnFailure=status-send-email@%n.service" without double quotes to the [Unit] section of any unit file to receive emails on failures.
- %n passes the unit's name to the template. %i is the instance name. Refer to the man page systemd.unit for further details.

## Installation Instructions

> **Note:** Anything proceeding the "$" is to be entered in the Linux operating system terminal/console.

1. $ git clone https://github.com/thewarden/urt-service.git
2. $ cd urt-service
3. Change paths to match the system.
    1. $ nano urbanterror.service
4. Set desired email address.
    1. $ nano status-send-email@.service
5. Install service unit files and email script.
    1. $ sudo mv urbanterror.service status-send-email@.service /etc/systemd/system
    2. $ sudo mv systemd-send-email /usr/local/sbin/
    3. $ sudo chmod +x /usr/local/sbin/systemd-send-email
6. Test service units. Correct any erorrs that may occur and the repeat the tests until successful.
    1. $ sudo systemctl start urbanterror.service
    2. $ sudo systemctl status urbanterror.service
    3. $ sudo systemctl start status-send-email@dbus.service
    4. $ sudo systemctl status status-send-emaild@.service 
       1. Check for an email message.
7. Enable service unit.
    1. $ sudo systemctl enable urbanterror.service

## Useful Commands

Description          |Command Example
---------------------|-----------------------
Reload all unit files|systemctl daemon-reload
Restart unit         |systemctl restart urbanterror.service
Start unit           |systemctl start urbanterror.service
Stop unit            |systemctl stop urbanterror.service
View unit status     |systemctl status urbanterror.service
View unit messages   |journalctl -u urbanterror.service
