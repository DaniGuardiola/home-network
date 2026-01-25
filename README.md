# home-network

a collection of scripts, utilities, configuration files, and more that i use to set up and maintain my home network

## what this repo helps me set up

- a dietpi machine with:
  - decent security settings
  - proper, secure ssh access
  - pre-installed software like docker, vim, git, DietPi dashboard, etc
  - nice ergonomics like fish shell + starship, aliases, etc
  - a reverse proxy (caddy) running in a docker container that exposes services to the internet in a secure way, including:
    - home assistant instance running on a separate machine in the local network

## usage

this repo assumes the following:

- a dietpi machine will be installed, and it will serve as the entry point to any publicly exposed services through a reverse proxy
- there is a separate home assistant machine in the local network with the `ha.servers` domain assigned to it, and with home assistant running in the default port (8123) which is open in the local network

the order of operations when setting things up from scratch:

### 1. dietpi.txt

use the `dietpi-txt-wizard` utility to create a `dietpi.txt` config. the config can vary a lot, but the hard requirements are:

- ssh is configured (i usually pick dropbear)
- a public ssh key is set up to log into the machine
- the dietpi-postinstall script is added by url (`https://h.dio.la/scripts/dietpi-postinstall`) to be run on first boot

### 2. the DietPi image

use `dietpi-baker` to create a DietPi image for the specific machine that will run it, and with the `dietpi.txt` config baked in.

### 3. flash the image

flash the created image to the sdcard. note that steps 1 and 2 can be skipped if you already have a baked image with a `dietpi.txt` config that you're happy with - you can just re-flash that image to the sdcard.

### 4. first boot

connect the machine to the network (typically via ethernet cable) and power it on. wait for the first boot to finish (including the postinstall script). once it's done, you should be able to ssh into the machine as the main dietpi user (as configured in `options.sh`) using the ssh key you provided.

### 5. manual steps

- change the password of the main user to something secure using `passwd`
- assign a fixed local network ip (optionally also a domain) for the dietpi machine, for ssh access
- set up port forwarding (80/443) in the gateway to the dietpi machine, so that the reverse proxy can be reached from the internet
- set up a vpn server in the gateway to be able to access the local network securely when outside, and ssh into the dietpi machine

## want to use this for your own home network?

while not the main purpose, here i will try to document how to use these scripts and utilities for your own home network setup. you will need to fork this repo and host it somewhere (github, gitlab, etc). then you will need to create a vercel deployment from it (default options) so that the `vercel.json` file will take effect.

### options

if you just want to copy my whole setup but using your own options, you need to do two things:

- modify the `options.sh` file at the root
- tweak the `vercel.json` file with the domains you want to use, then search+replace the entire repo to replace my domains with yours
  - h.dio.la points to the repo homepage itself (the web page, not the actual git repo ending in `.git`)
  - h.dio.la/some/path/to/a/file.txt points to the raw file at `some/path/to/a/file.txt` in the repo (useful as a quick shortcut to download and possibly execute files such as scripts)
  - h-utils.dio.la points to the `scripts/install-utils` script in the repo (handy shortcut so i don't need to remember the full path to the script)

### customizing

some things you might want to customize:

- reverse proxy configs (add, remove, or modify services), check `TODO:/add/path/here`
