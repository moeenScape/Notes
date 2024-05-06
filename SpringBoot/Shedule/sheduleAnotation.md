# Cron Syntax

Cron is a time-based job scheduling format used in Unix-like operating systems. It consists of five fields that specify when a command should run. These fields are:

- **Minute (0 - 59)**: Specifies the minute of the hour when the command should execute.
- **Hour (0 - 23)**: Specifies the hour of the day when the command should execute.
- **Day of the Month (1 - 31)**: Specifies the day of the month when the command should execute.
- **Month (1 - 12)**: Specifies the month when the command should execute.
- **Day of the Week (0 - 6)**: Specifies the day of the week when the command should execute (Sunday to Saturday; 7 is also Sunday on some systems).



Here is an example of the cron syntax:
```
┌───────────── second (0-59)
│ ┌───────────── minute (0-59)
│ │ ┌───────────── hour (0-23)
│ │ │ ┌───────────── day of the month (1-31)
│ │ │ │ ┌───────────── month (1-12 or JAN-DEC)
│ │ │ │ │ ┌───────────── day of the week (0-7 or SUN-SAT; 0 or 7 = Sunday)
│ │ │ │ │ │
│ │ │ │ │ │
* * * * * *
```

Breakdown of the Fields:
1. Second:
Specifies the second when the job should run (0-59).
2. Minute:
Specifies the minute when the job should run (0-59).
3. Hour:
Specifies the hour when the job should run (0-23).
4. Day of the Month:
Specifies the day of the month when the job should run (1-31).
5. Month:
Specifies the month when the job should run (1-12 or JAN-DEC).
6. Day of the Week:
Specifies the day of the week when the job should run (0-7 or SUN-SAT).
Special Characters:
- ```*``` (Asterisk): Represents all values. For example, * in the minute field means every minute.
- ? (Question Mark): Represents no specific value. Used in the day of the month and day of the week fields to avoid conflicts.
- ```-``` (Hyphen): Specifies a range of values. For example, 1-5 in the day of the week field means Monday to Friday.
- , (Comma): Specifies multiple values. For example, MON,WED,FRI means Monday, Wednesday, and Friday.
- / (Slash): Specifies increments. For example, 0/15 in the minute field means every 15 minutes starting from the top of the hour.
- L (Last): Used to specify the last occurrence. For example, L in the day of the month field means the last day of the month. L in the day of the week field means the last Friday, and so on.
- W (Weekday): Represents the nearest weekday. For example, 15W in the day of the month field means the nearest weekday to the 15th of the month.
- ```#``` (Number Sign): Specifies the nth occurrence of a day. For example, 2#3 in the day of the week field means the third Monday (2) of the month.
