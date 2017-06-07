---

layout: post
title: Notes For Noobs (aka Now What Do I Do?)
date: 2017-02-17

---

So you've spun up a new [Home Assistant](https://home-assistant.io) [Raspberry Pi](https://www.raspberrypi.org) with the guide [here](https://home-assistant.io/getting-started/installation-raspberry-pi-all-in-one/) and have no idea what to do now.  (Or you've never used this thing called 'Linux' before)  Hopefully these notes will point you in the right direction.

First off, make sure that you are ssh'd into your Pi. The process for this will differ depending on the client your using (Putty, MobaXTerm, Mac Terminal, Linux Terminal, etc.) Usually it involves some version of of the command `ssh pi@ipaddress`. 

For example, from Linux one runs:

![SSH Login]({{ site.url }}/assets/img/ssh.png)

Now that we are "on the box" as it were, we can make changes to the configuration etc.  Assuming that you have used the [Raspberry Pi All-In-One Installer script](https://home-assistant.io/getting-started/installation-raspberry-pi-all-in-one/), first let's change to the configuration directory.  Run the command `cd /home/homeassistant/.homeassistant/`.  This command changes the current directory to the one containing `configuration.yaml`.

For simplicity, let's start with the simple single file approach.  From your prompt, enter `sudo nano configuration.yaml`. This will run the editor `nano` as the `root` user and open the file `configuration.yaml` for editing.  

```text
cd /home/homeassistant/.homeassistant/
sudo nano configuration.yaml
```

![nano config yaml]({{ site.url }}/assets/img/nano.png)

Nano functions in a similar fashion to your standard word processor. Now, let's add a password to the frontend.  Rather than enter it directly in `configuration.yaml` we are going to store it in `secrets.yaml`.  While this does involve a second file, it makes your installation of Home Assistant more secure and reduces the need to scrub you config files when asking for help. It is also the recommended practice.  If however, you still don't want to use `secrets.yaml` an example `configuration.yaml` reflecting this is available [here](/docs/examplenosecrets.yaml). (Please be aware that the rest of this guide assumes that you have a `secrets.yaml`. Please alter/ignore the irrelevant bits as needed.) Assuming you're still in `nano` press Ctrl + X, Y (to confirm that you want to save changes, if you haven't made any, you might not be prompted), and then Enter.

You should now be back at the shell prompt: `pi@homeassistant:/home/homeassistant/.homeassistant $`

Now enter `sudo nano secrets.yaml` (note "secrets" is plural).  In this file, we will define all those things that we dont want to share with the world. (like api keys)  

![Example Secrets.yaml]({{ site.url }}/assets/img/secret.png)

As you'll notice in the image above, there are a couple lines prefixed with the pound/hash character \#; these are comments.  They are there soley for the purpose of being able to make sense of the code.  In our example, the first line indentifies the file itself (to the author) and the second serves as a 'heading' for the code block to follow. Next we define the placeholder to be used in the main configuration (this is what will show up in the config to represent the actual password).  In this case it has been named `ha_pwd` but it can be named anything you want; although having it make sense saves on hair pulling.  The text following the `:` is the actual 'secret' itself. (The frontend password in our case.)  It cant be stressed enough that you come up with your own password and NOT use the example here.

Again, close `nano` saving the changes.

Now, open up `configuration.yaml` again.  

![Nano with API Password Secret]({{ site.url }}/assets/img/nanopwd.png)

Notice in this image that the text after 'api_password' has been replaced with `!secret ha_pwd`. This is the magic that keeps the secrets safe. `!secret` tells HA to go look in the `secrets.yaml` file while `ha_pwd` specifies that the value associated with that name should be substituted in.  Thus, rather than entering `api_password: Penguin` we can instead use `api_password: !secret ha_pwd` thus keeping the password secure.  Also make sure to remove the leading \# or the line will remain a comment.

Now that Home Assistant is secure, let's add two new components to the `introduction` and to the items `discovery` has found.

![Nano showing added components]({{ site.url }}/assets/img/nanocomponew.png)

In the picture above you'll notice that two new blocks of code have been added. Specifically the entries for the [Weblink](https://home-assistant.io/components/weblink/) component, the [Weather Underground (WUnderground)](https://home-assistant.io/components/sensor.wunderground/) component, and a [Worldclock](https://home-assistant.io/components/sensor.worldclock/) component. You'll also notice the `-` before both the `platform:yr` and `platform: wunderground` lines.  This symbol is used to denote multiple entries under the parent (in this case sensor) section.  The specfic components themselves are available [here](https://home-assistant.io/components/) and have the relevent snippets of code in the entries there.

The weblink component should show links to the National Science Foundation page for McMurdo Station, the Home Assistant home page, and the API registration page for WUnderground.  The clock should show the time in Virgina (Where NSF is headquartered) and the Wunderground component requires something more.

Some components in Home Assistant require an api key or other authentication to that service. (Wink and IFTTT are a couple others.)  To make the Wunderground component work, you will need an api key from the link included as a weblink in our sample config here, or from the Component page (link above).  Once you've got that, let's add it to our configuration.

Since you've still got `nano` open, add a placeholder for the api key in our configuration under `api_key:`. (Or add your actual key if you dont want to use `secrets.yaml` to `configuration.yaml` in the form `api_key: yourkeyhere` and ignore the references to `secrets.yaml`)  Again it can be anything, but in this example its called `wu_key`.  Make sure you include the `!secret`. Close `nano`, saving the configuration.  Now open up `secrets.yaml` again.  

![Nano with wu secret]({{ site.url }}/assets/img/secretwu.png)

In this picture you can see that we have added another header and the placeholder `wu_key`.  As with the frontend password, enter the api key as provded after the `:`. (Here it is simply reference text)

Now close and save the secrets file.


Now comes the big reveal.  Restart Home Assistant by sending the command `sudo systemctrl restart home-assistant.log`.  Once the prompt returns, open a browser and enter the `ipaddressofthepi:8123` replacing the ipaddress as appropriate.

![Login Page]({{ site.url }}/assets/img/waitforit.png)

Once you enter your password, you should see the fruits of your labor.

![Success]({{ site.url }}/assets/img/success.png)


If not, check out the troubleshooting steps [here](). 

Credits: @JonMurphy, @bassclarinetl2
