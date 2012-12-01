Template project for running ants on Heroku
===========================================

Heroku project overview
-----------------------
Heroku wants a Procfile, which tells Heroku how to start your application. It specifies the connectAndPlay.sh script as boot script, which you don't need to change.

connectAndPlay.sh will then start a dummy HTTP server in a background process, which is necessary on Heroku (otherwise the app will be shut down). Then it starts the tcpclient.py, which connects to the Ants TCP server and runs your bot. For each new game it engages, it will start your bot in a new subprocess.


Language support
----------------
This template runs the python Ants TCP client, and can run a bot in another language that is supported on Heroku. The requirement is that there must exist a Heroku buildpack for that language.

In the .buildpacks file the buildpacks must be listed. Since python is used for the tcp client, it must be included in the .buildpacks.



Setup
-----
The boot script takes the ant server password from ENV, so it must be set thusly:

    heroku config:set ANT_SERVER_PASSWORD=mypassword

In the connectAndPlay.sh, change:

 * BOT_COMMAND: this command will be executed in a subprocess, running your bot
 * ANT_SERVER_USER: the username you will be listed as on the Ants TCP server


The Heroku app must be configured for multi-buildpack:

    heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

If your bot is written in anything else than python, a buildpack for your language must be added to the .buildpacks file.


Heroku issues
-------------
An app on the free plan will be shut down after one hour of not receiving HTTP requests. This can be circumvented in several ways.

### Add dyno
Add a second dyno to your Heroku app. Then it will never be shut down, but you will have to pay a monthly fee.

### Send requests to your app
Send HTTP requests to your app regularly from a cronjob, or use some third party tool like pingdom.com.

You can set up a free cronjob in Heroku, provided as an [addon](https://addons.heroku.com/catalog/schedule://addons.heroku.com/catalog/scheduler).
Set up a command like this:

    curl http://your-app-name.herokuapp.com/?$(date +%s)

that runs every ten minutes, where "your-app-name" is the name of your app.

