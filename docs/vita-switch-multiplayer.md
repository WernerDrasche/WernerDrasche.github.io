# PSP multiplayer with Switch and Vita
#### Requirements
- jailbroken switch (I have atmosphere)
- jailbroken vita (I have henkaku enso 1.65)
- spare computer to run the adhoc server on (I have raspberry pi 4)
#### What you get
You can play e.g. Monster Hunter Portable 3rd (only game I tested with this setup) online
with a PS-Vita, a Nintendo Switch and a Computer. The non-pc devices must be in the same
local network as the server.
#### Preparing the server
##### Linux computer
Download the source code from [this thread](https://forums.ppsspp.org/showthread.php?tid=25024) or from 
[here](vita-switch-multiplayer/adhoc-server.7z) and then run make (on ubuntu server for the raspberry I didn't have to install any
additional libraries but I think psql might be a requirement). Don't forget to make the server executable (`chmod +x server_name`).
To start the server at boot install the following crontab (`crontab -e`):
```
@reboot cd /absolute/path/to/server/directory && ./server_name
```
After testing if it runs fine reboot the raspberry pi.
##### Windows computer
Unless you know how to compile stuff in windows (I certainly don't) you can just resort to opening up a game in PPSSPP
on that computer everytime you want to play multiplayer. Make sure that the `network >> enable pro adhoc server` setting is checked.

#### Preparing the Vita
Follow [this guide](https://vita.hacks.guide/adrenaline.html) to install adrenaline. Adding an ISO is easy and involves just dragging it into the
`ux0:pspemu/ISO` folder (in windows you have to check the "show operating system folders" setting).
To exit adrenaline just long-press the home button, select settings and then close the emulation.
Now we might want to figure out the Vita's IP address. The easiest way that I could find was to just enable the FTP server on Vita-Shell
(press start and then change from USB to FTP mode). Now you can locate the device on your router and make the IP fixed which makes connecting
with FTP easier in the future (just create a new site in WinScp for example). The next step is the installation of the plugin that will allow
adrenaline to connect to the adhoc server. It is called "Pro online" and there is apparently a version created specifically for adrenaline.
You can get it on [reddit](https://www.reddit.com/r/vitahacks/comments/ixn9bk/psa_we_have_a_working_pro_online_client_for/) or
[here](vita-switch-multiplayer/pro-online.7z) in case the link is dead. Extract the contents into `ux0:pspemu` and prepare the following files:

- `game.txt`: `ms0:/seplugins/atpro.prx 1` 
- `hotspot.txt`: {name of wifi network (without the curly braces of course)}
- `server.txt`: {local ip address of spare computer (you should also make sure that it's fixed as well)}

Now go back to adrenaline and open the network settings. There you select infrastructure mode then scan and connect to your network.
To finish the plugin installation press select and enable `recovery menu >> advanced >> force high memory layout`. Go back and on your
way you should check that `recovery menu >> plugins >> atpro.prx` is indeed enabled.
#### Get PPSSPP for the Switch
Use [this thread](https://gbatemp.net/threads/ppsspp-switch-beta-release.535468/page-57#post-10189409) to get a recent release.
Change the following settings:

- `network >> pro adhoc address`: {server local IP-address}
- `network >> wifi channel`: 1
- `network >> port offset`: 0

If the fonts look bad ingame you can get original fonts [here](vita-switch-multiplayer/font.7z). Extract them into
`switch >> ppsspp >> assets >> flash0 >> font`. If there is no improvement try to change the game resolution (x times PSP)
and restart the game (maybe some stuff is cached).

#### Done
Everything should work now. You could probably add the server to a zerotier network
so that you could play with computers outside of the local network (I did not test this). 
It took me a lot of hours to get this to work so maybe this can be helpful to someone.
Try it out in Monster Hunter lobby :)
