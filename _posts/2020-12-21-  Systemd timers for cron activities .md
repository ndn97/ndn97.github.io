---
layout: post
title:  Systemd timers for cron activities
tags: systemd linux cron 
---

Cron utility in Linux has been helpful to schedule tasks to run at a scheduled time.  A few of the disadvantages of cron are 
1.	If Instance/server is not available/down at the cron schedule, cron doesn't run those tasks until the next event is triggered
2.	If the cron task need to run other tasks after running the main task at the scheduled time, there are only hacks to create this dependency
3.	If the cron task needs to run within a given time interval to avoid running at the same time 
4.	If a cron task needs to run after an event, such as 3 minutes after the system restarted. Cron doesn't support this functionality
5.	To check the status of the last cron task to see if it ran/failed with logs

Most of the above issues require an alternative to cron. the above issues are fixed using system timer in Linux  as follows:

Example Systemd Timer unit file

    # test.timer
    [Unit] 
    Description=Runs the test.service 5 minutes after boot-up & at/around 6 PM every weekday.
    [Timer] 
    OnCalendar=Mon..Fri 18:00
    ## supports multiple time formats
    Persistent=True
    ##If the machine is powered down and missed Oncalendar event, Persistent=true triggers if time has elapsed.
    OnBootSec=5 m 
    ## Event-Based, triggers the event 5 minutes after the machine is rebooted
    RandomizedDelaySec=1800
    ## Randomly add up to 30 mins to the schedule. This is to ensure the task runs between 6 PM to 6.30 PM.

    Unit=test.service 
    ## service/task which needs to be run when the event/time occurs
    [Install] 
    WantedBy=basic.target


Example Systemd service unit file
    # test.service
    [Unit] 
    Description= test.service that needs to be run

    [Service]
    ExecStartPre=<some executable>
    ExecStart=<some executable>
    ExecStartPost=<some executable>
    ## ExectStart runs only if ExecStartPre is successful and ExecStartPost runs only if ExecStart is successful
    [Install] 
    WantedBy=basic.target


To list all the system timers,last/next schedule

    ubuntu@ip-11-223-3-40:/tmp$ systemctl list-timers --all
    NEXT                        LEFT          LAST                        PASSED       UNIT                         ACTIVATES                     
    Tue 2020-12-22 00:00:00 IST 1h 19min left Mon 2020-12-21 16:06:50 IST 6h ago       logrotate.timer              logrotate.service             
    Tue 2020-12-22 00:00:00 IST 1h 19min left Mon 2020-12-21 16:09:06 IST 6h ago       man-db.timer                 man-db.service                
    Tue 2020-12-22 01:02:49 IST 2h 22min left Mon 2020-12-21 15:04:47 IST 7h ago       motd-news.timer              motd-news.service             
    Tue 2020-12-22 03:01:02 IST 4h 20min left Mon 2020-12-21 15:05:43 IST 7h ago       certbot.timer                certbot.service               
    Tue 2020-12-22 04:58:50 IST 6h left       Mon 2020-12-21 14:54:08 IST 7h ago       apt-daily.timer              apt-daily.service             

To Check the syntax of calendar event

    ubuntu@ip-11-223-3-40:~/tmp$ systemd-analyze calendar "Mon..Fri 06:01"
      Original form: Mon..Fri 06:01             
    Normalized form: Mon..Fri *-*-* 06:01:00    
        Next elapse: Tue 2020-12-22 06:01:00 IST
           (in UTC): Tue 2020-12-22 00:31:00 UTC
           From now: 7h left    


