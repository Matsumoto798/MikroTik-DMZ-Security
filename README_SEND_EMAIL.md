###Considerations for successful sending of logs, backups, configurations, etc. by email:

* The SMTP server must be correctly configured, in my case I used Zoho and associated it to my corporate gmail account.

* You must generate a unique password for the service, in this case only for the service associated to alerts in Mikrotik.

* You must place in the firewall filter the output of the corresponding ports, I am using Start-TLS, so I open the port 587, I will place below the sentence of that.



```bash
add action=accept chain=output comment="PORTS TO SEND EMAIL" dst-port=587,25,465 protocol=tcp

```
 
```bash
/system logging
add action=EMAILSEND topics=system
/system note
set show-at-login=no
/system scheduler
add interval=45m name=EMAIL-LOGS on-event=":local filename ([/system identity get name] . \"_logs_\" . [/system clock get date] . \".txt\
    \");\
    \n\
    \n/log print file=\$filename;\
    \n:delay 10s;\
    \n/tool e-mail send to=\"UREMAIL@GMAIL.COM\" subject=\"Logs de \$[/system identity get name]\" \\\
    \nbody=\"Mikrotik \$[/system identity get name]: archivo de logs generado el \$[/system clock get date] - \$[/system clock get time]\
    \" \\\
    \nfile=\$filename;\
    \n:delay 45s;\
    \n/file remove \$filename;\
    \n" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
add interval=23h name=BACKUP-CONFIG on-event="# Obtener la fecha actual (formato \"YY/MM/DD\")\r\
    \n:local dateStr [/system clock get date];\r\
    \n\r\
    \n# Crear el nombre del archivo de exportaci\F3n con fecha en formato YY-MM-DD\r\
    \n:local CONFIG ([/system identity get name] . \"_export_\" . [/system clock get date]);\r\
    \n\r\
    \n/export file=\$CONFIG;\r\
    \n:delay 7s;\r\
    \n/tool e-mail send to=\"UREMAIL@GMAIL.COM\" subject=\"\$[/system identity get name] export\" \\\r\
    \nbody=\"Mikrotik \$[/system identity get name]: configuration file on \$[/system clock get date] - \$[/system clock get time]\" file\
    =\$CONFIG;\r\
    \n\r\
    \n# Crear el nombre del archivo de backup con fecha en formato YY-MM-DD\r\
    \n:local backup ([/system identity get name] . \"_backup_\" . [/system clock get date]);\r\
    \n\r\
    \n/system backup save name=\$backup;\r\
    \n:delay 7s;\r\
    \n/tool e-mail send to=\"UREMAIL@GMAIL.COM\" subject=\"\$[/system identity get name] backup\" \\\r\
    \nbody=\"Mikrotik \$[/system identity get name]: backup file on \$[/system clock get date] - \$[/system clock get time]\" file=\$back\
    up;\r\
    \n\r\
    \n:delay 5s;\r\
    \n/file remove \$CONFIG;\r\
    \n/file remove \$backup;\r\
    \n\r\
    \n" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
/tool e-mail
set from=UREMAIL@GMAIL.COM port=587 server=smtp.zoho.com tls=starttls user=UREMAIL@GMAIL.COM
```
