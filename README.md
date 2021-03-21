# PINGJOE Documentation

[![PINGJOE Passive Monitoring](./pingjoe-logo.png?raw=true "PINGJOE Passive Monitoring")](https://www.pingjoe.com)

[PINGJOE](https://www.pingjoe.com) is a passive monitoring service for your backup tasks and other regularly executed jobs.

It does not probe your servers but rather waits for your program to PING JOE. You will receive an e-mail whenever your app misses the ping. The ping is merely a GET HTTP request to a special URL. 

> **Q**: I have a reliable backup tool with error reporting already. Why do I need PINGJOE?
> 
> **A**: PINGJOE is the best solution to cover scenarios like your backup tool failing or not running at all, whole range of network issues, backup server down, cron daemon issues, and more. PINGJOE awaits an OK result from your program. If it does not arrive, and there can be thousands of reasons, we will notify you. This solution is more robust and reliable than trying to identify separate problems. And last but not least the service is located on an independent network shielded from any local issues you might have.

![](./pingjoe-warning.png) 

A few practical examples of how to [PINGJOE](https://www.pingjoe.com) (get your own API credentials [here](https://www.pingjoe.com))

**Shell script**
```shell
rsync --archive production:/data/ /prod-backup/ && \
	curl https://user:123@api.pingjoe.com/nas-data/1d6h
```
Translation: Hey "nas-data" spotter! All is OK. Expect the next ping in 1 day and 6 hours.

**Cron**
```cron
 5 0 * * * /cloud/backup && /usr/bin/curl https://user:123@api.pingjoe.com/cloud/1d90m
```
Translation: Hey "cloud" spotter! All is OK. Expect the next ping in 1 day and 90 minutes.

**Guess**
```shell
 wget -q -O- https://user:123@api.pingjoe.com/some-job/1M
```
Translation: Hey "some-job" spotter! Expect the next ping in 1 month.

**PHP**
```php
 file_get_contents("https://user:123@api.pingjoe.com/orders-upload/1h10m");
```
You get the idea.

![](./pingjoe-emacs.png)

Syntax
------

RESTful service URL syntax:

https://{USER}:{PASSWORD}@api.pingjoe.com[/{SPOTTER}/{ACTION}][?{QUERY}]


{USER}

	The API user name defined in your account. It is different
	than your own account's login e-mail.

{PASSWORD}

	The API user password defined in your account. See {API_USER}
	for more info.

{SPOTTER}

	Any name of your choosing. Generally, any name that you can
	use as a file name will do. Example: "my-backup-2",
	"log-rotating" etc.

{ACTION}

	Specifies spotter actions. Accepts keywords "delete" to delete
	the spotter, "list" to list all spotters, "history" to see
	ping history, or {TIME} for regular ping that informs the
	spotter of the next expected ping time.

{TIME}

	Ping the spotter and notify PINGJOE that the next ping must
	happen within given {TIME}. The time can be a UNIX timestamp
	(integer) or {RELATIVE_TIME}.

{RELATIVE_TIME}

	A sequence of {NUMBER} + {UNIT} pairs. Examples: "1w2d",
	"1d12h30m"

{UNIT}

	supported time units are "m" - minutes, "h" - hours, "d" -
	days, "M" - months, "Y" or "y" for years

{QUERY}

	Following query parameters are supported:
	"format=json|text|help" and "logout" to log out from a
	browser.


Examples
--------

First ping to non-existing spotter will create a spotter. Here we
create a new spotter named "my-cron-job" that will expect the next
ping to happen within 1 week and 2 days.

       https://user:my_password@api.pingjoe.com/my-cron-job/1w2d

Ping the already existing "my-cron-job" spotter and set the next ping
time window to 1 hour and 10 minutes.

       https://user:my_password@api.pingjoe.com/my-cron-job/1h10m

Show "my-cron-job" spotter ping history.

       https://user:my_password@api.pingjoe.com/my-cron-job/history

List all the spotters of the API user "user".

       https://user:my_password@api.pingjoe.com

Disable & remove the "my-cron-job" spotter.

       https://user:my_password@api.pingjoe.com/my-cron-job/delete
