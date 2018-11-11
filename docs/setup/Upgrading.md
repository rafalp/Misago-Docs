Upgrading your Misago to latest version
=======================================

To upgrade your Misago site, connect to the server, do `cd ~/misago_docker` to move to Misago directory, and run following commands:

1. `git pull`
2. `./appctl upgrade`

`git pull` will download latest version of Misago from the GitHub, and `./appctl upgrade` will run upgrades.

You may be asked by `git pull` to "identify yourself". If this is the case, simply follow the instructions on the screen to set your GitHub e-mail.

> **Note:** This guide assumes that you have originally git cloned Misago to your home directory. If you have cloned it elsewhere, you will have to run `cd` with custom path.