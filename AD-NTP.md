# Time Configuration (NTP) in Active Directory

Quick snippets for time service management in an Active Directory forest.

## Check time against all Domain Controllers

- w32tm /monitor

## Test external NTP target

- w32tm /stripchart /computer:0.pool.ntp.org /samples:5 /dataonly
- w32tm /stripchart /computer:1.pool.ntp.org /samples:5 /dataonly
- w32tm /stripchart /computer:2.pool.ntp.org /samples:5 /dataonly

## Set external NTP on Forest PDC

- w32tm /config /manualpeerlist:"0.pool.ntp.org,0x8 1.pool.ntp.org,0x8 2.pool.ntp.org,0x8" /syncfromflags:manual /update
- w32tm /config /reliable:yes
- net stop w32time && net start w32time
- w32tm /resync
- w32tm /stripchart /computer:0.pool.ntp.org /samples:5 /dataonly

## Set NTP to sync from AD (all except Forest PDC)

- w32tm /config /syncfromflags:domhier /update
- net stop w32time && net start w32time
- w32tm /query /peers

## Clear NTP settings

- net stop w32time 
- w32tm /unregister 
- w32tm /register 
- net start w32time

# Best Practise 

- Before moving Forest PDC FSMO test external NTP sync from new PDC target
- Set NTP with GPO to cover FSMO moves and new server and DC builds (use WMI Filter to include or exclude PDC)
- - https://theitbros.com/configuring-dc-for-sync-time-with-external-ntp-server/
- - https://theitbros.com/time-configuration-for-virtualized-domain-controllers/


