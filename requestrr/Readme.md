# Home assistant add-on: requestrr

![Supports aarch64 Architecture][aarch64-shield] ![Supports amd64 Architecture][amd64-shield] ![Supports armhf Architecture][armhf-shield] ![Supports armv7 Architecture][armv7-shield]
![Supports smb mounts][smb-shield]

# About

---

[Requestrr](https://github.com/darkalfx/requestrr) is a chatbot used to simplify using services like Sonarr/Radarr/Ombi via the use of chat. Current platform is Discord only, but the bot was built around the ideology of quick adaptation for new features as well as new platforms.
This addon is based on the docker image https://github.com/linuxserver/docker-requestrr

# Installation

---

The installation of this add-on is pretty straightforward and not different in comparison to installing any other add-on.

1. Add my add-ons repository to your home assistant instance (in supervisor addons store at top right, or click button below if you have configured my HA)
   [![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Falexbelgium%2Fhassio-addons)
1. Install this add-on.
1. Click the `Save` button to store your configuration.
1. Set the add-on options to your preferences
1. Start the add-on.
1. Check the logs of the add-on to see if everything went well.
1. Open the webUI and adapt the software options

# Configuration

---

Webui can be found at <http://your-ip:PORT>.
The default username/password : described in the startup log.
Configurations can be done through the app webUI, except for the following options

```yaml
GUID: user
GPID: user
TZ: timezone
```

## Support

Create an issue on github

# Illustration

![illustration](https://nashosted.com/wp-content/uploads/2020/04/requestrr-discord-nashosted.com_-1024x680.jpg)

---

[repository]: https://github.com/alexbelgium/hassio-addons
[smb-shield]: https://img.shields.io/badge/smb-yes-green.svg
[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[armhf-shield]: https://img.shields.io/badge/armhf-yes-green.svg
[armv7-shield]: https://img.shields.io/badge/armv7-yes-green.svg