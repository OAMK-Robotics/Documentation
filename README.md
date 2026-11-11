# Documentation

In this space we will be keeping all the documentation across the entire robotics project. 

## The teams

NAO: Antti, Kalle, Simon

Drone: Amila, Josh

GoPiGo: Annina, Ben, Boris

UR: Samuel, Rasanjula, Ed

## First ideas and diagrams
In our first meeting we were tasked with coming up with mind maps for the individual robots and their functionalities:

[Mind maps for robotics project.pdf](https://github.com/user-attachments/files/34419168/Mind.maps.for.robotics.project.pdf)

Afterward we came up with the following ideas:

[Ideas for robotics project.pdf](https://github.com/user-attachments/files/34419045/Ideas.for.robotics.project.pdf)

From this document we decided on the robot bar as the idea for our project.

This is the initial flow chart we came up with for the project:

[Flow chart robotics project.pdf](https://github.com/user-attachments/files/34419207/Flow.chart.robotics.project.pdf)

With this flow chart we had a rough idea of what we wanted the project to be able to do, but we did not have the technical specifics yet.

## Technical specifications

### MQTT broker

All the robots will communicate through an MQTT broker which is connected to the ASUS router within the room. The topic structure, event topics and message schemes can be found in the document below.

[mqtt_schema.pdf](https://github.com/user-attachments/files/34471134/mqtt_schema.pdf)

### 3D Printing

We used the Prusa CORE one+ to print our cups and our cup holder. Below you can find the schematics for both.

<img width="542" height="545" alt="glass" src="https://github.com/user-attachments/assets/685459d1-b40d-4fc2-a51d-743164658f6f" />

### Software and code

The main bulk of the software for the project is written in Python 3.13, this includes: the drone table detection, Nao's communication, the GoPiGo's navigation and all the communication with the MQTT broker. The movements of the UR are designed in its proprietary software but the connection with the rest of robots was established in python. Nao is an edge case because Nao's code is written in Python 2.7 but through a flask server the communication with Nao through Python 3.13 is made possible.

## Daily meetings and Sprint meetings

### Daily meetings
Daily meeting on 05.11.2026:

[Daily documentation 05.11.pdf](https://github.com/user-attachments/files/34420031/Daily.documentation.05.11.pdf)

Daily meeting on 06.11.2026:

[Daily documentation 06.11.pdf](https://github.com/user-attachments/files/34469637/Daily.documentation.06.11.pdf)

Daily meeting 10.11.2026:

[Daily documentation 10.11.2026.pdf](https://github.com/user-attachments/files/34618524/Daily.documentation.10.11.2026.pdf)

Daily meeting 11.11.2026:

[Daily documentation 11.11.2026.pdf](https://github.com/user-attachments/files/34677829/Daily.documentation.11.11.2026.pdf)


### Sprint reviews

Sprint review 09.11.2026:

[Sprint review 09.11.2026.pdf](https://github.com/user-attachments/files/34568503/Sprint.review.09.11.2026.pdf)

### Sprint plannings

Sprint planning 09.11.2026:

[Sprint planning 09.11.2026.pdf](https://github.com/user-attachments/files/34571655/Sprint.planning.09.11.2026.pdf)

## Individual robot functionalities

### UR

### GoPiGo

### NAO

### Tello drone

The individual functionality of the Tello drone can be broken down into 2 main software components. The connection code, for the connection to the router and the detection code, for identifying the table that customers are sitting at.

#### Connection Code

In short this script configures the drone to leave its own WIFI and connect to the 2.4 GHZ ASUS router in the room. It does that by sending plain-text UDP commands to the drone’s SDK port, using the Tello SDK’s command and ap <ssid> <password> commands. The code can be found in the drone repository under tello_connect_router.py

#### Detection Code

The detection code is a Python application that controls the Tello drone on order to search for customers sitting at tables in the bar. It uses the drones mission pads to represent the tables, mission pad 1 is table 1 and mission pad 3 is table 2. It connects to the local MQTT broker and publishes a JSON event showing the table that is in use. After finding the specific table it ends its side to side search motion, returns to the center, retreats and lands. The specific code can be found in the drone repository under Drone table detection.py.
