# Documentation

In this space we will be keeping all the documentation across the entire robotics project. 

## The teams

NAO: Antti, Kalle, Simon

Drone: Amila, Josh

GoPiGo: Anniina, Ben, Boris

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

The broker was later uploaded into a docker file and made usable without the specific end user device. Below you can find the brief on the MQTT configuration with the docker.

[mqtt_brief (1).pdf](https://github.com/user-attachments/files/35267728/mqtt_brief.1.pdf)


### 3D Printing

We used the Prusa CORE one+ to print our cups and our cup holder. Below you can find the schematics for both.

<img width="442" height="445" alt="glass" src="https://github.com/user-attachments/assets/685459d1-b40d-4fc2-a51d-743164658f6f" />
<img width="512" height="272" alt="image" src="https://github.com/user-attachments/assets/57742467-0f7d-40d1-b4f6-0b015aeab432" />


### Software and code

The main bulk of the software for the project is written in Python 3.13, this includes: the drone table detection, Nao's communication, the GoPiGo's navigation and all the communication with the MQTT broker. The movements of the UR are designed in its proprietary software but the connection with the rest of robots was established in python. Nao is an edge case because Nao's code is written in Python 2.7 but through a flask server the communication with Nao through Python 3.13 is made possible.

### Final order of execution

Below you can find a more detailed flow diagram showing the final order of execution for the project demo. This contains the individual tasks of each robot and in which order they're carried out.

[Flow Diagram (detailed).pdf](https://github.com/user-attachments/files/35016964/Flow.Diagram.detailed.pdf)


## Daily meetings and Sprint meetings

### Daily meetings
Daily meeting 05.11.2026:

[Daily documentation 05.11.2026pdf](https://github.com/user-attachments/files/34420031/Daily.documentation.05.11.pdf)

Daily meeting 06.11.2026:

[Daily documentation 06.11.2026pdf](https://github.com/user-attachments/files/34469637/Daily.documentation.06.11.pdf)

Daily meeting 10.11.2026:

[Daily documentation 10.11.2026.pdf](https://github.com/user-attachments/files/34618524/Daily.documentation.10.11.2026.pdf)

Daily meeting 11.11.2026:

[Daily documentation 11.11.2026.pdf](https://github.com/user-attachments/files/34677829/Daily.documentation.11.11.2026.pdf)

Daily meeting 12.11.2026:

[Daily documentation 12.11.2026.pdf](https://github.com/user-attachments/files/34737823/Daily.documentation.12.11.2026.pdf)

Daily meeting 17.11.2026:

[Daily Documentation 17.11.2026.pdf](https://github.com/user-attachments/files/34959850/Daily.Documentation.17.11.2026.pdf)

Daily meeting 18.11.2026:

[Daily Documentation 18.11.2026.pdf](https://github.com/user-attachments/files/35017041/Daily.Documentation.18.11.2026.pdf)

Daily meeting 19.11.2026:

[Daily Documentation 19.11.2026.pdf](https://github.com/user-attachments/files/35078450/Daily.Documentation.19.11.2026.pdf)

Daily meeting 24.11.2026:

[Daily documentation 24.11.2026.pdf](https://github.com/user-attachments/files/35322313/Daily.documentation.24.11.2026.pdf)


### Sprint reviews

Sprint review 09.11.2026:

[Sprint review 09.11.2026.pdf](https://github.com/user-attachments/files/34568503/Sprint.review.09.11.2026.pdf)

Sprint review 13.11.2026:

[Sprint review 13.11.2026.pdf](https://github.com/user-attachments/files/34800712/Sprint.review.documentation.13.11.2026.pdf)

Sprint review 20.11.2026:

[Sprint review 20.11.2026.pdf](https://github.com/user-attachments/files/35146253/Sprint.review.20.11.2026.pdf)


### Sprint plannings

Sprint planning 09.11.2026:

[Sprint planning 09.11.2026.pdf](https://github.com/user-attachments/files/34571655/Sprint.planning.09.11.2026.pdf)

Sprint planning 16.11.2026:

[Sprint planning 16.11.2026.pdf](https://github.com/user-attachments/files/34904276/Sprint.planning.16.11.2026.pdf)

Sprint planning 23.11.2026:

[Sprint planning 23.11.2026.pdf](https://github.com/user-attachments/files/35265935/Sprint.planning.23.11.2026.pdf)


## Individual robot functionalities

### UR

The UR is responsible for preparing and loading the drinks within the robot bar. It fills the cup and places it in the cup holder atop the GoPiGo. It also retrieves the empty cup from the GoPiGo once it is returned.

#### Code for the motions

The code for the motions is divided into 5 different programs: Pouring left.urp, Pouring middle.urp, Pouring right.urp, loading.urp and pickup.urp. Pouring left.urp prepares the drink in the left dispenser. Pouring middle.urp prepares the drink in the middle dispenser. Pouring right.urp prepares the drink in the right dispenser. loading.urp places the filled cup onto the GoPiGo. pickup.urp removes the empty cup from the GoPiGo.

#### Remote control code

The robot is connected to a laptop via ethernet. A python script on this laptop allows the robot to be controlled remotely, using socket communication and the UR dashboard's server on port 29999.

#### MQTT communication code

Through the Paho MQTT library in python the UR sends and receives MQTT messages. The controller chooses a pouring program based on the incoming command.

#### Challenges

The first challenge arose in finding a suitable position for the cup and the three dispensers. The position had to be within the robot's reach and it had to be reachable within the robots range of motion. The biggest challenge was loading and unloading the cups on the GoPiGo due to the cup holder having very little free space and thus requiring a very precise motion. 

### GoPiGo

The GoPiGo is responsible for transporting the prepared drinks from the robot bar to the correct table and returning to the starting position. It uses a line-following system to navigate along a predefined track and sensors to detect obstacles and monitor the cup during transportation. 

#### Navigation 

The navigation functionality is implemented in Python using the EasyGoPiGo3 library. The GoPiGo starts its journey next to the UR arm, follows black lines on the tables using a linefollowing sensor and proportional control, and returns to the same starting position after completing a delivery.

At the junction, the robot determines which of the two tables to navigate to based on the table ID and predefined table route. The modified T-junction layout allows it to detect the endpoint of the main line through an all-white sensor reading. It then performs a predefined 45 or -45 degree turn using the turn_degrees() function and searches for the separate black line leading to the selected table.

The original Y-junction design caused difficulties because proportional control could unintentionally steer the robot towards the left branch. Separating the lines with a small white gap and using predefined turns made junction detection and route selection more reliable. 

#### Obstacle detection 

The GoPiGo uses an I2C distance sensor to detect obstacles in front of the robot. When an obstacle is detected within the defined safety distance, the robot stops and activates a buzzer to indicate the situation. The robot then performs a recovery manoeuvre before attempting to continue its navigation. 

#### Cup monitoring 

An ultrasonic sensor is used to monitor whether the cup remains in its holder during transportation. The system measures the distance to the cup and compares the readings against a calibrated baseline. When the cup is detected as removed, the monitoring system triggers a callback that publishes a drink-delivered event through MQTT. 

#### MQTT communication

The GoPiGo communicates with the other robots through the MQTT broker using the Paho MQTT Python library. It subscribes to messages from the UR robot to receive information about prepared drinks and publishes its own status and event messages.

This allows the GoPiGo to communicate its availability, delivery progress and completed deliveries to the rest of the system.

#### Main program and threading

The main program initializes the robot, sensors and MQTT client. It starts two separate threads: one for navigation and another for MQTT communication with the broker.

The navigation thread handles line following, obstacle detection and cup monitoring by calling their respective functions. The MQTT thread maintains the broker connection and handles incoming and outgoing messages. This allows navigation and MQTT communication to operate concurrently. 

#### Challenges

One of the main challenges has been achieving reliable navigation around junctions. The original Y-junction design caused difficulties because proportional control could unintentionally steer the robot towards the left branch. Modifying the track and introducing predefined turns made navigation more predictable.

Another challenge has been I2C communication. Both the line-following sensor and the distance sensor use the same I2C bus, and reading both sensors concurrently appeared to cause intermittent I2C I/O communication errors.

Combining the different functionalities has also required careful coordination. Navigation, obstacle detection and cup monitoring need to operate without interfering with each other. 

### NAO

#### Code / function

#### Challenges

### Tello drone

The individual functionality of the Tello drone can be broken down into 2 main software components. The connection code, for the connection to the router and the detection code, for identifying the table that customers are sitting at.

#### Connection Code

In short this script configures the drone to leave its own WIFI and connect to the 2.4 GHZ ASUS router in the room. It does that by sending plain-text UDP commands to the drone’s SDK port, using the Tello SDK’s command and ap <ssid> <password> commands. The code can be found in the drone repository under tello_connect_router.py

#### Detection Code

The detection code is a Python application that controls the Tello drone on order to search for customers sitting at tables in the bar. It uses the drones mission pads to represent the tables, mission pad 1 is table 1 and mission pad 3 is table 2. It connects to the local MQTT broker and publishes a JSON event showing the table that is in use. After finding the specific table it ends its side to side search motion, returns to the center, retreats and lands. The specific code can be found in the drone repository under Drone table detection.py.

#### Raspberry Pi

We decided to use a Raspberry Pi with a headless Linux system in order to start the drone's software with the push of a button. We uploaded all the relevant code onto the Raspberry Pi along with a python script that allows the button push to send an MQTT publish for the drone's table detection software to execute. The Raspberry Pi also includes the MQTT configuration file for the drone. 

#### Challenges

The Tello drone comes with an array of challenges we had to learn to deal with. The drones battery life is a very limiting factor when it comes to its use. To be able to use it properly we have to use it only for a short duration so that the battery does not run empty. It has an in flight battery life of about 5-8 minutes which means it can't stay in the air for long. Another challenge that came with the drone was its tendency to overheat from being om and ready. The drone can be on and not moving but it will heat up and at a certain point it will be too hot to take off. Another challenge that we had to face was the fact that the drones built in stabilizers wouldn't keep it stable in the air.
