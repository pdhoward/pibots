# pi-bots

This is a simple system I came up with for running multiple Twitter bots from my Raspberry Pi. Essentially, it really just makes cron task scheduling a tiny bit easier, and allows you to edit your schedule remotely via GitHub, using the gitpull shell script. Note I run this on Raspbian GNU/Linux 8 (jessie).

I've also includded the code to a few of my own bots in the /bots directory, to help get you started with making bots in node.js. Basically just use [Twit](https://github.com/ttezel/twit).

The setup is pretty easy. Just install node.js and:

1. Clone this repo to your Pi
2. `npm install`
3. Edit cron jobs on your Pi
`$ crontab -e`
4. Paste in the contents of pi/cron.txt
5. Copy pi/gitpull.sh to /home/pi/shell/
6. Make it executable with `$ chmod +x gitpull.sh`
7. When you create a new bot, put it in a new folder in the /bots directory.

If you copy pi-bots to your own repo, you can use gitpull.sh to pull from it periodically, so that you can edit your bots remotely. All you have to do is add it to an interval in tasks.json, like:

```
"10mins": [
  "/home/pi/shell/gitpull.sh"
],
```

You can run your bots the same way. For example, if you want to run a bot every two hours, you'd add the run command to that interval in tasks.json, like:

```
"2hours": [
  "/usr/sbin/node /home/pi/pi-bots/bots/ascii_bot/app.js",
],
 ```
You can also run multiple bots at the same interval:

```
"2hours": [
  "/usr/sbin/node /home/pi/pi-bots/bots/ascii_bot/app.js",
  "/usr/sbin/node /home/pi/pi-bots/bots/hum/app.js"
],
 ```
Note you can view the most recent cron job errors/responses for each interval with, for example:

`$ cat /tmp/2hourError`

And you can view the cron job event log with:

`$ grep CRON /var/log/syslog`
