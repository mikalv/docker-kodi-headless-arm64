[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/index.php/irc/
[podcasturl]: https://www.linuxserver.io/index.php/category/podcast/
[appurl]: https://kodi.tv/
[hub]: https://hub.docker.com/r/lsioarmhf/kodi-headless-aarch64/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/kodi-headless-aarch64
[![](https://images.microbadger.com/badges/version/lsioarmhf/kodi-headless-aarch64.svg)](https://microbadger.com/images/lsioarmhf/kodi-headless-aarch64 "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/kodi-headless-aarch64.svg)](http://microbadger.com/images/lsioarmhf/kodi-headless-aarch64 "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/kodi-headless-aarch64.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/kodi-headless-aarch64.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/arm64/arm64-kodi-headless)](https://ci.linuxserver.io/job/Docker-Builders/job/arm64/job/arm64-kodi-headless/)

A headless install of kodi in a docker container, most useful for a mysql setup of kodi to allow library updates to be sent without the need for a player system to be permanently on.

[![kodi](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/kodi-banner.png)][appurl]

## Usage

```
docker create --name=kodi-headless \
-v <path to data>:/config/.kodi \
-e PGID=<gid> -e PUID=<uid> \
-e TZ=<timezone> \
-p 8080:8080 -p 9777:9777/udp \
lsioarmhf/kodi-headless-aarch64
```

**Parameters**

* `-p 8080` - webui port
* `-p 9777/udp` - esall interface port
* `-v /config/.kodi` - path for kodi configuration files
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` - for timezone information *eg Europe/London, etc*

It is based on ubuntu xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it kodi-headless /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application
`IMPORTANT... THIS IS THE ARM64 VERSION`

Mysql/mariadb settings are entered by editing the file advancedsettings.xml which is found in the userdata folder of your /config/.kodi mapping. Many other settings are within this file also.

The default user/password for the web interface and for apps like couchpotato etc to send updates is kodi/kodi.

If you intend to use this kodi instance to perform library tasks other than merely updating, eg. library cleaning etc, it is important to copy over the sources.xml from the host machine that you performed the initial library scan on to the userdata folder of this instance, otherwise database loss can and most likely will occur.

## Info

* Shell access whilst the container is running: `docker exec -it kodi-headless /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f kodi-headless`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' kodi-headless`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/kodi-headless-aarch64`

## Credits
Various members of the xbmc/kodi community for patches and advice.

## Versions

+ **22.08.17:** Bump Krypton to 17.4.
+ **25.05.17:** Bump Krypton to 17.3.
+ **23.05.17:** Bump Krypton to 17.2.
+ **23.04.17:** Initial Release.
