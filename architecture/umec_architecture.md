# MicroMEC Architecture

## MicroMEC Unit

The diagram below presents the a basic uMEC Unit that includes hardware and 
software elements. The uMEC Unit is typically installed close to the data 
sources (sensors, cameras etc.). 

Integration to data sources are done in cooperation with the hardware vendors. 

![uMEC Unit](./umec_unit.png)

Below we will outline each major block of the architecture.

### Sensor Plugins

Data source vendors may provide access to high level APIs (e.g. REST, ONVIF) or 
lower level protocols (Modbus, I2C, SPI etc.) over physical layers such as 
RS-485, RS-232, Ethernet etc. 

For integration purposes uMEC has a plugin system: Sensor Plugins. 

{continue}

### NATS

{todo}

### APIs and Applications

{todo}

### Network Connections

{todo}

## MicroMEC Network

uMEC Units can form their own private network, and they may also connect to a 
cloud network. 

![uMEC Network](./umec_network.png)

{explanation}
