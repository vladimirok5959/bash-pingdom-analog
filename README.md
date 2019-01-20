# bash-pingdom-analog
Let your server monitor other your servers like pingdom

If you have two or more server in different regions, you can simple setup this cron script to monitor any servers or sites and send notifications to your email. Why? Simple, it's free.

## Install sendmail
**Skip this step if you already have worked sendmail on server**
```
apt-get update
apt-get install sendmail
```
or remove first and install
```
apt-get purge sendmail && apt-get update
apt-get install sendmail
```
<br/>If need, install `sendmail-bin` too
```
apt-get install sendmail-bin
```
<br/>Correctly set server host name
```
hostnamectl set-hostname myhostname.com
hostname myhostname.com
```
where `myhostname.com` - is your real server host name
<br/><br/>Get your current ip and write it to `hosts` file

`ip -d address | grep -v "inet6" | grep "inet" | grep "scope" | grep "global" | grep "brd" | awk '{print $2}' | awk -F"/" '{print $1}'`
```
192.168.0.145
```
<br/>Edit hosts file, add your server ip and real host name
```
nano /etc/hosts
127.0.0.1 localhost
192.168.0.145 myhostname.com myhostname
```
where `myhostname.com` - is your real server host name

<br/>Edit sendmail config file
```
nano /etc/mail/sendmail.mc
```
<br/>Fix hostname here for your or add this block
```
dnl #
dnl # Masquerading options
FEATURE(`always_add_domain')dnl
MASQUERADE_AS(`mydomain.com')dnl
FEATURE(`allmasquerade')dnl
FEATURE(`masquerade_envelope')dnl
dnl #
```
<br/>Also add this line
```
CLIENT_OPTIONS(`Family=inet')dnl
```
<br/>Then configure and restart sendmail service
```
sendmailconfig
service sendmail restart
```
<br/>Check if email can be sent
```
echo "Test message" | sendmail -v my@mail.com
```
where `my@mail.com` - is your real email

## Script location
```
Location: /usr/local/bin/pingdom
```

## Usage
Inside script just set your host, email and response http code, if http check is used. You can add more emails for the same host. You can check server by domain or ip, with ping/http type. Set for GMail filter to protect mail from spam.
```
# Add host name or IP with email here:

# add_host_ping "google.com" "my@email.com"
# add_host_ping "facebook.com" "my@email.com"
# add_host_http "google.com" "my@email.com" "200"

```

## Crontab
```
* *	* * *	root	/usr/local/bin/pingdom >/dev/null
```

## Quick install
wget -O /usr/local/bin/pingdom https://github.com/vladimirok5959/bash-pingdom-analog/releases/download/latest/pingdom; chmod +x /usr/local/bin/pingdom
