Upgrading your Misago to latest version
=======================================

To upgrade your Misago site, connect to the server, do `cd ~/misago_docker` to move to Misago directory, and run following command:

```
./appctl upgrade
```

You will be asked to confirm that you wish to upgrade your site to latest available version.

Upgrade process will back up your data, then pull latest code from [GitHub repository](https://github.com/rafalp/misago_docker). After pull completes successfully, your site will be stopped and updated to latest version.

You may be asked by `git pull` to "identify yourself". If this is the case, simply follow the instructions on the screen to set your GitHub e-mail.

You will also be presented with command you can use to return to previous version in case that things go awry. You will need to restore the backup manually to recover your data.

> **Note:** This guide assumes that you have originally git cloned Misago to your home directory. If you have cloned it elsewhere, you will have to run `cd` with custom path.