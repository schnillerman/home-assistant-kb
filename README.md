# Matter & Threads
## Setup Docker-Based Matter Server
This needs to be on the host network. So you might want to use a RPi or similar, the performance is sufficient.
### Setup Docker
#### Linux / Raspberry Pi
##### Outsource Docker Data
If you're using an RPi and want to use, e.g., an external drive or another directory for all docker data (images, containers, volumes, etc.), you need to create a file with ```sudo nano /etc/docker/daemon.json``` and insert the following:
```
{
  "data-root": "[directory]"
}
```
##### Install Docker & Docker Compose
1. https://docs.docker.com/engine/install/debian/
2. https://docs.docker.com/engine/install/linux-postinstall/
### Start Matter Server
Docker-compose:
Note the _network_mode: host_!
```
services:
  # python-matter-server
  home-matter-server:
    image: ghcr.io/home-assistant-libs/python-matter-server:stable
    container_name: home-matter-server
    restart: unless-stopped
    # Required for mDNS to work correctly
    network_mode: host
    security_opt:
      # Needed for Bluetooth via dbus
      - apparmor:unconfined
    volumes:
      # Create an .env file that sets the USERDIR environment variable.
      - ./data:/data #${USERDIR:-$HOME}/docker/matter-server/data:/data/
      # Required for Bluetooth via D-Bus
      - /run/dbus:/run/dbus:ro
    # If you adjust command line, make sure to pass the default CMD arguments too:
    #command: --storage-path /data --paa-root-cert-dir /data/credentials --bluetooth-adapter 0
```
Add the matter integration in Home Assistant, configure with IP of the RPi (do not change port) then pair first device!
# Dashboards
## Synology NAS
<img src="https://github.com/user-attachments/assets/07d9a74d-c496-483e-83ad-3ef64410ca86" width="200" align="right">

Source: [Reddit](https://www.reddit.com/r/synology/comments/1gwpq15/home_assistant_synology_integration_dashboard/) (where else) - [YAML Code on Pastebin](https://pastebin.com/nphtJm3j)
Code:
```
#HA Dashboard for Synology NAS using Synology Integration
#Image: https://imgur.com/f033ShZ
type: grid
cards:
  - type: heading
    heading_style: title
    heading: NAS
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_temperature_7
        color: orange
        tap_action:
          action: more-info
        icon: mdi:memory
      - type: entity
        show_state: true
        show_icon: false
        entity: sensor.nas_cpu_utilization_total
        color: red
        tap_action:
          action: more-info
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_memory_usage_real
        color: blue
        tap_action:
          action: more-info
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_security_status
        tap_action:
          action: more-info
        color: state
  - type: tile
    entity: sensor.nas_download_throughput
    name: Inbound
    color: red
  - type: tile
    entity: sensor.nas_upload_throughput
    name: Outbound
    color: green
  - graph: line
    type: sensor
    entity: sensor.nas_used_space
    name: Volume 1
    detail: 1
  - graph: line
    type: sensor
    entity: sensor.nas_used_space_2
    name: Volume 2
    detail: 1
  - type: heading
    heading: Volume 1
    heading_style: title
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_volume_used
        color: cyan
        tap_action:
          action: more-info
        icon: mdi:harddisk
      - type: entity
        show_state: true
        show_icon: true
        color: cyan
        entity: sensor.nas_used_space
        tap_action:
          action: more-info
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status
        tap_action:
          action: more-info
        color: state
    icon: ""
  - type: heading
    icon: mdi:harddisk
    heading: Drive 1
    heading_style: subtitle
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_temperature_3
        color: orange
        tap_action:
          action: more-info
        icon: mdi:thermometer
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_below_min_remaining_life_3
        color: red
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_exceeded_max_bad_sectors_3
        color: cyan
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status_4
        tap_action:
          action: more-info
        color: state
  - type: heading
    icon: mdi:harddisk
    heading: Drive 2
    heading_style: subtitle
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_temperature_4
        color: orange
        tap_action:
          action: more-info
        icon: mdi:thermometer
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_below_min_remaining_life_4
        color: red
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_exceeded_max_bad_sectors_4
        color: cyan
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status_5
        tap_action:
          action: more-info
  - type: heading
    icon: mdi:harddisk
    heading: Drive 3
    heading_style: subtitle
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_temperature
        color: orange
        tap_action:
          action: more-info
        icon: mdi:thermometer
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_below_min_remaining_life
        color: red
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_exceeded_max_bad_sectors
        color: cyan
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status_2
        tap_action:
          action: more-info
  - type: heading
    heading: Volume 2
    heading_style: title
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_volume_used_2
        color: cyan
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        color: cyan
        entity: sensor.nas_used_space_2
        tap_action:
          action: more-info
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status_8
        tap_action:
          action: more-info
    icon: ""
  - type: heading
    icon: mdi:sd
    heading: Drive 1
    heading_style: subtitle
    badges:
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_temperature_2
        color: orange
        tap_action:
          action: more-info
        icon: mdi:thermometer
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_below_min_remaining_life_2
        color: red
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: binary_sensor.nas_exceeded_max_bad_sectors_2
        color: cyan
        tap_action:
          action: more-info
        icon: ""
      - type: entity
        show_state: true
        show_icon: true
        entity: sensor.nas_status_3
        tap_action:
          action: more-info
column_span: 1
```
