# TCAM-Overflow

The goal of this script is to monitor the TCAM usage on the switch and rollback configuration if a configuration change causes the TCAM to overflow. 

- The event options runs the script every one minute
- The script checks the tcamlog file to see if there is  the message “No space available in tcam for” in the tcamlog file
- If the message is found in the log file, the script rolls back the configuration by 1. So, the previous change that triggered the tcam to go over will be removed
- This log file is cleared after the rollback

Here is the configuration required on the switch to run it.
- set event-options generate-event TCAM_CUSTOM_EVENT time-interval 60
- set event-options policy CHECKTCAMSTATUS events TCAM_CUSTOM_EVENT
- set event-options policy CHECKTCAMSTATUS then event-script tcam_rollback.slax
- set event-options event-script file tcam_rollback.slax
- set system syslog file tcamlog any any

 
- Copy the script to “/var/db/scripts/event” directory
- Configure tcamlog file as one of the syslog targets.
