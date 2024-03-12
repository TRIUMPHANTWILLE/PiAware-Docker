![dark](https://github.com/obstruse/PiAware-Docker/raw/master/Images/dark3-2.png "dark")
# PiAware
Docker build of PiAware 9.2 + dump1090-fa + OpenLayers3 + lighttpd, for Raspberry Pi.

https://github.com/obstruse/PiAware-Docker<br>
https://hub.docker.com/r/obstruse/piaware

Intended for use on LibreELEC(Kodi) with Docker and Portainer addons.

## Hardware

- Raspberry Pi 3-5 (e.g.: [https://www.canakit.com/raspberry-pi-5](https://www.canakit.com/raspberry-pi-5))
- RTL-SDR Dongle (e.g.: [https://flightaware.store/products/pro-stick](https://flightaware.store/products/pro-stick) )

## Install Piaware

Access Portainer at:  http://192.168.1.12:9000 (replace the IP with the address of your Raspberry Pi)

![Dashboard](https://github.com/obstruse/PiAware-Docker/raw/master/Images/dashboard.png "Dashboard")

### Add tmpfs Volume

* Volumes -> Add volume
  * Name: run
  * Driver: local
  * Driver options -> add driver option
    * name: device
    * value: tmpfs
  * Driver options -> add driver option
    * name: type
    * value: tmpfs
  * Driver options -> add driver option
    * name: o
    * value: size=20m,uid=1000
  * Use NFS volume: off
  * Use CIFS volume: off
  * Enable access control: off
* Click **Create the volume**

![tmpfs](https://github.com/obstruse/PiAware-Docker/raw/master/Images/tmpfs.png "tmpfs")
  
### Deploy Container

* Containers -> Add container
  * Container name: Piaware
  * Image configuration Name: obstruse/piaware
  * Port mapping
    * 8180 -> 8080 (to avoid conflict with Kodi on 8080)
    * 30005 -> 30005 (data in Beast format)
    * 30105 -> 30105 (MLAT data in Beast format)
  * Disable Access control
  * Console:  Interactive & TTY
  * Volumes -> map additional volume
    * Container: /run  Volume
    * volume: run - local  Writable  
  * Restart policy: Unless stopped
  * Runtime & Resources: Privileged Mode
* Click **Deploy the container**

![Container-1](https://github.com/obstruse/PiAware-Docker/raw/master/Images/container1.png "Container1")
![Volumes](https://github.com/obstruse/PiAware-Docker/raw/master/Images/volumes.png "Volumes")
![Restart Policy](https://github.com/obstruse/PiAware-Docker/raw/master/Images/restartpolicy.png "Restart Policy")
![Resources](https://github.com/obstruse/PiAware-Docker/raw/master/Images/resource.png "Resources")

The PiAware Skyview web page will be available on port 8180 on your Raspberry Pi;
the OpenLayers3 web page will be available on port 8181.

### Claim your PiAware client on FlightAware.com

https://flightaware.com/adsb/piaware/claim

### Set Location and Height of Site

Go to your stats page:

https://flightaware.com/adsb/stats/user/yourusername

Click on the gear icon to the right of your site name, and configure location and height.

### Changing Feeder ID

The feeder ID is assigned when the feeder first connects, and is stored in the PiAware instance.  If you redeploy PiAware, a new feeder ID is assigned and will look like a new site on FlightAware. If you want to continue using the same site after redeploying, you need to manually reconfigure the feeder ID.

* Find the feeder ID that you want to use. Go to:  https://flightaware.com/adsb/stats/user/yourusername. The feeder ID is labeled "Unique Identifier",
* Using Portainer, open a console on the PiAware instance and enter:
```
piaware-config feeder-id
```
* Paste the unique identifier.
* Restart piaware

![enroute](https://github.com/obstruse/PiAware-Docker/raw/master/Images/enroute2-2.png "enroute")
