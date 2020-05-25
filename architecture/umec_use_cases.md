# MicroMEC Use cases

There are different use cases for uMEC, and in fact we have used it as a basis 
for competitions to invent and implement them. 

## [IoThon 2019](https://iothon.io)

The challenge was to create example demos of new digital services and business
opportunities in a real smart city using LuxTurrim5G and uMEC. The [challenge 
description](https://github.com/nokia/IoT-Hackathon-sample-code) also had a 
sample code to help getting started. 

Please [click here to read more and find photos](https://www.aalto.fi/en/news/5g-enabled-iot-innovations-created-at-iothon)
about the event.

### [Moose ETWS](https://github.com/mooseetws/mooseetws-tensorflow-detection)

The winners implemented Moose ETWS (Early Traffic Warning System). 

This system consists of a phone application that a user can use for registering 
to the service and for receiving warnings of roaming moose in the area. 

The uMEC nodes have cameras and run a Tensorflow algorithm to detect the moose. 
When the user is driving in that area a warning will be sent.  With this 
application, detection can be done on the uMEC node and there is no need to
send the whole live image to a data center for analysis.

### Fighting Mongooses: Fluffy Hounder

The runner up team implemented Fluffy Hounder. It is a service for pet owners: 
the uMEC nodes recognize and identify dogs, cats and other animals. If a pet 
gets lost, it can be detected automatically. The pet owner will pay a monthly 
fee for the service.

## [Junction 2019](https://2019.hackjunction.com)

The challengers of the [Open Open City](https://2019.hackjunction.com/challenges/open-open-city)
track were asked to demonstrate use cases and create a prototype of an open data
platform which can collect, analyze and share weather, traffic, emergency 
information with citizens.

Detailed challenge [description and APIs were provided](https://github.com/SUSE/Junction2019)
for all participants. 

### [Flowify](https://app.hackjunction.com/projects/junction-2019/view/5dcf0a0762c4f1002bdd095f)

The winner team was Flowify, who used openSUSE Tumbleweed base containers for 
their backend which ran entirely on a uMEC Edge gateway node. They used 
state-of-the-art image detection to analyze static images taken by the security 
cameras. A people count is extracted from the pictures and collected. 
Pictures were taken via a uMEC API. The data can be utilized in city planning 
and design of shopping malls and other places.

### [suseXcity](https://app.hackjunction.com/projects/junction-2019/view/5dcf1c0a62c4f1002bdd5eee)

The one man team became 1st runner of. He created a dashboard with an SMS 
backhaul for managing uMEC clusters.

Source code [available](https://github.com/CiubotaruBogdan/suseXcity). 

### [Ravenholm](https://app.hackjunction.com/projects/junction-2019/view/5dcf0f23914ad1002b8db833)

The 2nd runner up was Ravenholm. They brought a business case, where uMEC 
deployments would help city officials, construction site planners and project 
managers to better manage the effects of large, open air building projects.

Source code [available](https://github.com/muke101/junction-hack-project).

## Metropolia Innovation Project

During fall 2019, students from the Metropolia University used the platform and 
created a new service according to the ETSI MEC framework, using OpenAPI to 
generate APIs and skeleton code.

Separate teams were responsible for different areas of the project. 

They have built a backend infrastructure that consists of 4 servers and a couple
of Raspberry Pi 3B+ uMEC nodes. 

The teams created a CI / CD environment with Jenkins, a comprehensive 
[performance monitoring and visualisation](https://github.com/Metropolia-Innovation-Project-2019-H2/umec-performance)
with Grafana and [improved the Moose ETWS application](https://github.com/Metropolia-Innovation-Project-2019-H2/uMEC-ETWS--API)
for container based installations. 
