---

layout: post
title: Creating a user to edit my HA config via SFTP
date: 2017-06-21

---

So you've heard about editing your config via SFTP or your sick and tired of editinig via nano or vi, here's how to create a sftp user to allow editing via [Sublime Text](https://www.sublimetext.com/) for example.  (Although Sublime requires a paid addon to deal with sftp)

NOTE:
This guide assumes that you are able to ssh into the enviroment and have sudo access.  It also doesn't take into account any 'tweaks' that a docker enviroment might require.

DISCLAIMER: 

As usual, this guide is a best effort and I'm not responsible if by allowing an SFTP user access to your device it starts making poor life choices.

That out of the way let's "[Get on with it!](https://youtu.be/l1YmS_VDvMY)"

Initially we need to add the user.  For the sake of sanity I've named it `sftpedit` but any username will do provided it doesn't already exist:

```
sudo useradd --system sftpedit
```

This command will add the user `sftpedit` as a system user (no home directory and shouldn't ask for a password)

Now that the user exists we need to make it a member of the `homeassistant` group.  This is related to permissions and to remove the need to grant access to the config directory to everyone which is a decided security risk:

```
sudo usermod -aG homeassistant sftpedit
```

Now that we have the user and it's in the correct group, we need to give it a password so that we can login with it. Yes, having the password be the same as the username is awful from a security standpoint and I should be whipped with a cat 6 cable ;-), but in my enviroment I can only remotely access the network via a vpn, and if 'they' are already on my network it's game over already, so I'm less concerned.  If this gives you nightmares, change the `crypt` command to look like this `crypt("password","Q4")` replacing "password" as desired:

```
sudo usermod -p `perl -e "print crypt("sftpedit","Q4")"` sftpedit
```

Now let's make sure that all the files in the config directory are owned by user `homeassistant` and group `homeassistant` (The `-R` here tells `chown` to recurse through all sub-directories:

```
sudo chown -R homeassistant:homeassistant /home/homeassistant/.homeassistant
```

Lastly, we need to grant permissions to Read, Write, and Execute to the `homeassistant` group:

```
sudo chmod -R g+rwx /home/homeassistant/.homeassistant/
```

You should now be able to access your configuration via SFTP using the credentials defined in these steps.

As always, reach out if you're having issues.