# OpenDL
OpenDL is an open source data logger, with the ability to measure temperature, humidity and concentration of alcohol in the air in spacially separated points. 

This device has been designed and built in order that anyone with little or zero technical knowledge could replicate it and make it work in a straightforward process of very simple steps.

## Basics
The device has a central unit (MCU), which could be considered as the "brain" of the system. From this central unit, emerge what we call the "Nodes". These nodes are the units that measure the different variables; temperature, relative humidity and concentration of alcohol in the air. Each node consists of 4 sensors; three DHT22 (temperature and relative humidity) and one MQ3 (concentration of alcohol in the air). A DHT22 and an MQ3 are physically inside the node. The two remaining DHT22, detach from the node to be placed anywhere you want to measure temperature and relative humidity, without the need of an extra node.

The system interacts in a way in which the MCU commands all the nodes (Master-Slave). The MCU can command up to 16 nodes. Each node carries a unique identifier (ID), which is manually defined through the DIP switch installed inside the node. Once the program starts, the MCU performs a scanning loop to identify the active nodes. Once the scan is complete, the MCU begins cyclically to collect the data from each node. This data is stored in a database, which can then be accessed by the user. 

![Topology](/images/interconexion.png)

## Getting Started

These instructions will explain everything you need to know and acquire in order to build this data logging device and get it up and running.

### Prerequisites

Here we will give you the list of materials you will need to buy.

MCU Materials:

* Raspberry Pi 3B+ or newer
* Micro SD memory card with 8gb or more
* Power supply 5V/2.5A
* MAX485 Module
* Logic Level Converter 5V/3.3V
* x3 LEDs (preferably green, red and blue)
* x3 330 Ohms resistors
* x2 100 Ohms resistors
* Button
* Push button
* x2 unipolar wire (preferably of different colors)
* 5x5cm PCB (for homemade manufacturing) 
* Male and female pin headers

Materials per Node:

* Arduino Nano
* Power supply 5V/1A
* MAX485 module
* x3 DHT22 sensor
* MQ3 sensor
* 4 keys DIP Switch
* LED
* 330 Ohms resistor
* 10x10cm PCB (for homemade manufacturing)
* x3 2 input terminal blocks
* x2 unipolar wire (preferably of different colors)
* Male and female pin headers

### Assemblying

Once you have everything you need, the assemblying process is pretty straightforward. 

First of all, you will need to get your PCBs done, so you can later solder the components. You can use directly the following images for th printing process:

![server](/images/circuitoServer.png)
![node](/images/circuitoNodo.png)

The following schematics display the connections between components:

![serverSchem](/images/server-schem.png)
![nodeSchem](/images/node-schem.png)

Once the PCBs are ready, you just need to wire the nodes with the MCU in a daisy chain topology as shown above. Make sure to connect the 100 Ohms resistors at the end-point of the lines to avoid impedance reflectance. More spececifically, you will have to connect one 100 Ohms resistor betweens lines A and B of the MCU, and one 100 Ohms resistor between lines A and B of the farthest node.

### Software Installation

**Note:** For the Raspberry Pi installation, you will need a monitor with an HDMI connection, a keayboard and a mouse.

#### MCU installation

1. Install Raspberry Pi OS following the steps from the [official Raspberry Pi page](https://www.raspberrypi.org/documentation/installation/installing-images/).
1. Once the OS is installed, connect the device to a local wifi network.
1. Open a command line interface.
1. Install Node JS through the following commands:
	> $ -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
	> $ sudo apt-get install -y nodejs
1. Verify correct installation:
	> $ node -v
1. Install the database:
	> $ sudo apt-get install mysql-server -y
	> $ npm install mysql
1. Create the database:
	> $ sudo mysql --user=root
	> $ DROP USER 'root'@'localhost';
	> $ CREATE USER 'root'@'localhost' IDENTIFIED BY 'opendl';
	> $ GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost';
1. Download server software
	> $ cd Documents
	> $ git clone https://github.com/arielrahmane/DLServer.git
	> $ cd DLServer
	> $ sudo npm install --unsafe-perm
1. Serial port configuration
	> $ sudo raspi-config
	> Go to interface options -> Serial and select **NO**
1. Install wifi-connect module
	> $ bash <(curl -L https://github.com/balena-io/wifi-connect/raw/master/scripts/raspbian-install.sh)
1. Configure the Raspberry Pi so the program runs automatically after booting up:
	> $ sudo nano /etc/rc.local
	> Go to the end of the file and put the following line right before the *exit 0* line
	> $ sudo node /home/pi/Documents/DLServer/src/app.js &
	> Do Ctrl+O > Enter > Ctrl+X

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc