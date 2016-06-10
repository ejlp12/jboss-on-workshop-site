Download patch file `jon-server-3.3-update-05.zip`

Stop JON server using command `rhqctl stop`

Extract the patch file into a directory

Go into the extracted directory

Run command to apply the patch `./apply-updates.sh <JON_SERVER_INSTALL_DIR>`

For example:
```
./apply-updates.sh /Servers/jon-server-3.3.0.GA/
```

Wait until you see following message in the console:

```
You have successfully extracted patch contents into (JON server) and/or (JON agent) installation folder(s).
```


