# Set up date and time on Debian OS

## View information on local time, date, timezone,..

`timedatectl`

```
Local time: Wed 2017-08-09 14:37:36 +07
Universal time: Wed 2017-08-09 07:37:36 UTC
RTC time: Wed 2017-08-09 07:40:07
Time zone: Asia/Ho_Chi_Minh (+07, +0700)
NTP enabled: no
NTP synchronized: yes
RTC in local TZ: no
DST active: n/a
```
## Setting date and time

`timedatectl set-time ‘YYYY-MM-DD HH:MM:SS’`

`timedatectl set-time YYYY-MM-DD //setting date only`

`timedatectl set-time HH:MM:SS //setting time only`

## Timezone

`sudo dpkg-reconfigure tzdata`

Then following the step: