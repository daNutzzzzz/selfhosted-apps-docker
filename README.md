# Selfhosted-Apps-Docker

###### guide-by-example

---

![logo](https://i.imgur.com/u5LH0jI.png)

---

* [Buku](buku/) - Bookmarks
* [bookstack](bookstack/) - notes and documentation
* [borg_backup](borg_backup/) - backup utility
* [CouchPotato](couchpotato/) - Automatic Video Library Manager for Movies
* [ofelia](ofelia/) - job scheduler
* [DDNS Updater](ddns-updater/) - automatic DNS update
* [Headphones](headphones/) - Automatic Library Manager for Music
* [Heimdall](heimdall/) - Homepage
* [HomeAssistant](homeAssistant/) - Home Automation
* [LazyLibrarian](lazylibrarian/) - Automatic Library Manager for Books
* [LibreSpeed](librspeed/) - Speedtest
* [nextcloud](nextcloud/) - file share & sync
* [Medusa](medusa/) - Automatic Video Library Manager for TV Shows
* [NZBHydra](nzbhydra/) - Usenet meta search
* [PiHole](pihole/) - DNS Network Protection
* [PlexMediaServer](plexmeddiaserver/) - Media Streaming Server
* [Portainer](portainer/) - docker management
* [prometheus_grafana](prometheus_grafana/) - monitoring
* [SABnzbd](sabnzbd/) - Newsreader
* [SmokePing](smokeping/) - Network Latency Monitor
* [Tautulli](tautulli) - Plex Media Server Monitor
* [Uptime-Kuma](https://github.com/louislam/uptime-kuma) - Uptime Monitor
* [unifi](unifi/) - mangment utility for ubiquiti devices
* [watchtower](watchtower/) - automatic docker images update
* [wireguard](wireguard/) - the one and only VPN to ever consider
* [arch_linux_host_install](arch_linux_host_install)

# How to self host various services

You do need to have **basic linux and basic docker-compose knowledge**,
the shit here is pretty hand holding and detailed, but it still should not be
your first time running a docker container.

a certain format is followed in the services pages

* **Purpose & Overview** - basic overview and intented use
* **Files and directory structure** - lists all the files/folder involved
 and their placement
* **docker-compose** - the recipe file how to build a container, with .env file too
* **Reverse proxy** - reverse proxy specific settings, if a container has
 a webserver providing web interface
* **Update** - how to update the container, usually just running Watchtower
* **Backup and restore** - of the entire container using borg backup
* **Backup of just user data** - steps to backup databases and other user data
* **Restore the user data** - steps to restore user data in a brand new setup


The core of the setup is Caddy reverse proxy.</br>
It's described in most details.

# Some extra info

### Compose

When making changes use `docker-compose down` and `docker-compose up -d`,
not just restart or stop/start.

* you **do not** need to fuck with `docker-compose.yml` to get something up,
simple copy paste should suffice
* you **do** need to fuck with `.env` file, that's where all the variables are
  
Often the `.env` file is used as `env_file`,
which can be a bit difficult concept at a first glance.

`env_file: .env`

* `.env` - actual name of a file that is used only by compose.</br>
  It is used automatically just by being in the directory
  with the `docker-compose.yml`</br>
  Variables in it are available during the building of the container,
  but unless named in the `environment:` option, they are not available
  in the running containers.
* `env_file` - an option in compose that defines an existing external file.</br>
  Variables in this file will be available in the running container,
  but not during building of the container.

So a compose file having `env_file: .env` mixes these two together.

Benefit is that you do not need to make changes at multiple places.
Adding variables or changing a name in `.env` does not require you
to also go in to compose to add/change it there...  also the compose file
looks much cleaner, less cramped.

Only issue is that **all** variables from the `.env` file are available in
all containers that use this `env_file: .env` method.</br>
That can lead to potential issues if a container picks up enviroment
variable that is intented for a different container of the stack.

In the setups here it works and is tested, but if you start to use this
everywhere without understanding it, you can encounter issues.
So first troubleshooting step should be abandoning `.env` and write out 
the variables directly in the compose file only under containers that want them.

---

### Docker images latest tag

All images are without any tag, which defaults to `latest` tag being used.</br>
This is [frowned upon](https://vsupalov.com/docker-latest-tag/),
but feel free to choose a version and sticking with it once it goes to real use.

---

### Bind mount

No docker volumes are used. Directories and files from the host
are bind mounted in to containers.</br>
Don't feel like I know all of the aspects of this,
but I know it's easier to edit a random file on a host,
or backup a directory when it's just there, sitting on the host.

---

### SendGrid

For sending emails free sendgrid account is used, which provides 100 free emails
a day.

The configuration in `.env` files is almost universal, `apikey` is
really the username, not some placeholder.
Only the password(actual value of apikey) changes,
which you generate in apikey section on SendGrid website.

Though I heard complains lately that is not as easy as it was to register on SendGrid.

---

### Cloudflare

For managing DNS records. The free tier provides lot of managment options and 
benefits. Like proxy between your domain and your server, so no one
can get your public IP just from your domain name. Or 5 firewall rules that allow
you to geoblock whole world except your country.

[How to move to cloudflare.](https://support.cloudflare.com/hc/en-us/articles/205195708-Changing-your-domain-nameservers-to-Cloudflare)

---

### ctop

[official site](https://github.com/bcicen/ctop)

![ctop-look](https://i.imgur.com/nGAd1MQ.png)

htop like utility for quick containers managment.

It is absofuckinglutely amazing in how simple yet effective it is.

* hardware use overview, so you know which container uses how much cpu, ram, bandwith, IO,...
* detailed info on a container, it's IP, published and exposed ports, when it was created,..
* quick managment, quick exec in to a container, check logs, stop it,...

Written in Go, so its super fast and installation is trivial when it is a single binary,
as likely your distro does not have it in repos. If you use arch, like I do, its on AUR.


---

### other guides

[StarWhiz/docker_deployment_notes](https://github.com/StarWhiz/docker_deployment_notes/blob/master/README.md)
    - got inspired and wrote in similar way setup for various services

* Heimdall - Another Homepage Dashboard
* FreePBX+Asterisk - VOIP & telephony server
* Mumble - Voice Chat Before Discord Days
* PureFTPd - FTP server
* qBit+windscribe - Torrent Client w/ Windscribe VPN
* Rocket.chat - Discord / Slack Clone
* ShinobiCCTV (Not Recommended) - CCTV NVR
* Seafile - Cloud Drive
* Ubiquiti UniFi Controller - Management Utility for Ubiquiti Devices
* Wordpress - CMS / Website Hosting
* Zoneminder - CCTV NVR

---

### other selfhosted lists

* [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted)
* [SelfhostedPro](https://github.com/SelfhostedPro/selfhosted_templates)
* [DoTheEvo](https://github.com/DoTheEvo/selfhosted-apps-docker)
* [kha7iq](https://github.com/kha7iq/selfhosted)

### Cyber

* [Web Crawlers](https://github.com/BruceDone/awesome-crawler)
* [Honeypots](https://github.com/paralax/awesome-honeypots)
