# Cron Syntax

Cron is a time-based job scheduling format used in Unix-like operating systems. It consists of five fields that specify when a command should run. These fields are:

- **Minute (0 - 59)**: Specifies the minute of the hour when the command should execute.
- **Hour (0 - 23)**: Specifies the hour of the day when the command should execute.
- **Day of the Month (1 - 31)**: Specifies the day of the month when the command should execute.
- **Month (1 - 12)**: Specifies the month when the command should execute.
- **Day of the Week (0 - 6)**: Specifies the day of the week when the command should execute (Sunday to Saturday; 7 is also Sunday on some systems).



Here is an example of the cron syntax:

┌────────────────────── minute (0 - 59) <br>
│  ┌──────────────────── hour (0 - 23) <br>
│  │ ┌───────────────────  day of the month (1 - 31) <br>
│  │ │ ┌──────────────────  month (1 - 12)<br>
│  │ │ │ ┌─────────────────  day of the week (0 - 6) (Sunday to Saturday; 7 is also Sunday on some systems)<br>
│  │ │ │ │<br>
│  │ │ │ │<br>
`* * * * *`  command to execute

`1 0 * * * printf "" > /var/log/apache/error_log` 
`45 23 * * 6 /home/oracle/scripts/export_dump.sh` 
`*/5 1,2,3 * * * echo hello world` 
 


| Entry          | Description                                    | Equivalent to    |
|----------------|------------------------------------------------|------------------|
| @yearly        | Run once a year at midnight of 1 January     | `0 0 1 1 *`      |
| @monthly       | Run once a month at midnight of the first day of the month | `0 0 1 * *`  |
| @weekly        | Run once a week at midnight on Sunday         | `0 0 * * 0`      |
| @daily (or @midnight) | Run once a day at midnight               | `0 0 * * *`      |
| @hourly        | Run once an hour at the beginning of the hour | `0 * * * *`      |
| @reboot        | Run at startup                                | —                |


