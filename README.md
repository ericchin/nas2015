# Liferay NAS 2015 - ELK: Unlocking YourData's Potential

## Logstash Configuration
Copy the conf files in the respective folders over to your logstash folder and start up logstash using the below command (run this for each of the different logstash configuration files).

`./bin/logstash agent -f logstash.conf`

## Apache Configuration
Add the below in the log\_config\_module with the other other log configurations.

`LogFormat "%h %l %u %t %m %f \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%U\"  \"%q\" %T %D" liferay`
