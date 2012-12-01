Template project for running ants on Heroku
===========================================

This template runs the python Ants TCP client, and can run a bot in another language that is supported on Heroku. The requirement is that there must exist a Heroku buildpack for that language.
In the .buildpacks file the buildpacks must be listed.

The Heroku app must be configured for multi-buildpack:

    heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

The boot script takes the ant server password from ENV, so it must be set thusly:

    heroku config:set ANT_SERVER_PASSWORD=mypassword


