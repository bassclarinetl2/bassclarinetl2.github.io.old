---

layout: post
title: Halp I think I borked something
date: 2017-02-17

---

So you've just spent hours (and several [drinks of choice](https://youtu.be/jvCTaccEkMI?t=16)) getting your config entered and the frontend wont come up or you get a notification about an Invalid Config.  Well [don't panic](https://youtu.be/7rOMGIbY-9s?t=70) grab your towel, and lets figure out what went wrong. (Also, we do have normality, so if you can't cope with the problem, that's on [you](https://youtu.be/YCRxnjE7JVs?t=8).)

NOTE:
Again the assumption is that you installed via the [Raspberry Pi](https://www.raspberrypi.org/) [All-in-One](https://home-assistant.io/docs/installation/raspberry-pi-all-in-one/) script so for other platforms some of these steps might not apply.

First of let's take a look at the `home-assistant.log` file in the same directory as `configuration.yaml`.  This can be accomplished by using the command `cat home-assistant.log`.  

On a running system it should be full of information:


![home-assistant.log in nano]({{ site.url }}/assets/img/homeass-log.png)

But wait, there are some errors in there.  Specifically it looks like the [panel_custom](https://home-assistant.io/components/panel_custom/) component has an issue with its config. Also it looks like the [steam](https://home-assistant.io/components/sensor.steam_online/) component is having issues.  However even with these issues, the front end still loaded.

![Steam user status]({{ site.url }}/assets/img/steam.png)

Now, if the frontend fails to load and you've made sure you are using the right ip address, port (default is 8123) and protocol (HTTP vs HTTPS), then `home-assistant.log` will likley look similar to this:

![home-assistant.log with error]({{ site.url }}/assets/img/homeass-log_error.png)

This particular issue can be decoded as follows, on line 74 (or 73 or 75) in column 3 (or 2 or 4) something isn't right.  In this specific case, the `api_password:` section was indented to far.  Which brings up the topic of indents in YAML. The [hass website](https://home-assistant.io/getting-started/yaml/) has a decent explanation that boils down to indents and tab vs space makes a difference. Line Endings can also cause weird effects but that only comes into play when you are using an editor on windows and moving files to the pi.

If `home-assistant.log` doesn't provide any information, or hass doesn't get to the point of writing to the log, we need to look at the 'journal'.

Before we look at the entire journal, or even the subsection related to the hass service, let's determine if hass is even running. (This also can apply to mosquitto).  While SSH'd into the box, run the following `sudo systemctl status home-assistant.log -l`. (For mosquitto subsititute `mosquitto.service`)

![Hass Status Running]({{ site.url }}/assets/img/servstatushass.png)

This is what should show up for a running hass instance.  If it says something else (other than Stopped) under `Active:` something has gone wrong.

Its usually at this point that you should get in touch with the Home Assistant Community on [Gitter](https://gitter.im/home-assistant/home-assistant).

When you do, you can help us out by letting us know:

- How you installed HASS.
- What platform your running on (rpi, Mac, Ubuntu, etc.).

The members in the chat may ask for snippets of logs or your configuration.  You can either paste the relevant chunk of code in the chat directly: blocked out with three backticks on there own line:

   e.g.  
   \`\`\`  
   code  
   \`\`\`  

or upload it to [Hastebin](http://hastebin.com) ([Pastebin](http://pastebin.com) is fine too) and paste the link into the [chat](https://gitter.im/home-assistant/home-assistant).  Please make sure to sanitize (read: remove anything sensitive: passwords, api keys etc.) before uploading. You're using `secrets.yaml` right?

When you do ask a question in the chat, please keep in mind that we are volunteers, and might not be able to get to your question right away.  If you don't get a reply don't take it as a slight, your question may have gotten lost in the shuffle, or no one has an answer.

The random notes section will be found [here](/docs/random.md) when i get to it..

AUTHORS:

{% for contributor in site.github.contributors %}

{{ contributor.avatar_url }} {{ contributor.login }}
{% endfor %}
