# Documentation

In this space we will be keeping all the documentation across the entire robotics project. 

## The teams

NAO: Antti, Kalle, Simon

Drone: Amila, Joshua

GoPiGo: Anniina, Ben, Boris

UR: Samuel, Rasanjula, Eetu

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

### NAO

The NAO robot is responsible for customer interaction and order-taking. Its primary purpose is to receive drink orders through voice communication, process the spoken information with the help of an external AI system and provide an appropriate verbal response to the customer. NAO acts as the main interface between the customer and the automated drink preparation process. Once an order has been identified, the corresponding information is transmitted through the MQTT infrastructure so that the UR robot can prepare the requested drink.

Initially, we planned to implement a movement function that would allow NAO to walk towards the table identified by the Tello drone. However, during development, we decided against this approach because NAO's walking movements were relatively noisy, slow and difficult to integrate reliably into the overall process. Instead, NAO will be positioned between the two customer tables, allowing it to interact with customers from a fixed location. This approach reduces the complexity of the system and avoids unnecessary movement-related problems.

NAO is programmed using Choregraphe and Python 2.7. The main functionality is implemented through the robot's built-in NAOqi modules, including ALTextToSpeech, ALAudioRecorder and ALAudioPlayer. These modules allow NAO to speak to customers, record their voices and play back audio files. The current Get Order script begins by asking the customer for their order before recording approximately three seconds of audio. The recording is stored locally as a WAV file with a sampling rate of 16 kHz.

#### Speech recognition and AI integration

For the initial implementation, we selected Google's Gemini AI to recognise and interpret customer orders. The Flask backend sends the recorded audio to Gemini together with instructions describing NAO's role as a robot waiter. The AI is instructed to recognise the customer's speech, identify the requested drink and generate a short, professional response. Since the robot bar currently offers three different drinks, the system uses predefined drink IDs: beer (1), soda (2) and wine (3). These IDs allow the recognised order to be communicated to the other robotic components without transferring the entire conversation.

Although Gemini provides a functional solution for processing customer orders, we identified its complexity and processing time as potential limitations. Since our application only needs to recognise a small number of predefined drinks, a large language model may offer more functionality than necessary. Therefore, we began investigating OpenAI's Whisper as an alternative speech recognition system. In the proposed implementation, Whisper transcribes the recorded audio, while a separate Python function searches for the corresponding drink keywords and assigns the correct drink ID. This could simplify order processing and potentially improve response times, although the actual performance still needs to be evaluated.

The decision to replace Gemini with Whisper depends on the results of further testing and the remaining development time. Gemini will remain the primary implementation if Whisper does not provide sufficient improvements or if integrating the alternative would introduce unnecessary risks before the project deadline.

#### MQTT communication and backend

MQTT is used as the communication infrastructure connecting the different components of the robot bar. The system follows a publish-subscribe architecture, in which the individual robotic systems exchange messages through a central MQTT broker. For our project, we established a Mosquitto MQTT broker on a virtual machine. Tailscale is used to support the network connectivity required for communication between the NAO and the backend.

Our NAO team is responsible for connecting NAO's customer interaction functionality to the MQTT communication system. Instead of establishing a direct MQTT connection from NAO, we implemented a Flask backend that acts as an intermediary between the robot and the broker. This approach allows us to use modern Python libraries for MQTT communication and AI processing while maintaining compatibility with NAO's older Python 2.7 environment.

#### Flask Backend and MQTT Bridge

The backend is implemented in Python using Flask, the Paho MQTT library and additional libraries for processing audio recordings and AI responses. It communicates with the MQTT broker through predefined topics and subscribes to the messages published by the Tello drone. When the drone detects an occupied table, it publishes the corresponding table information through MQTT. The backend receives this information and temporarily stores incoming messages in a queue, allowing NAO to retrieve them through a separate HTTP connection.

The Flask backend provides two main endpoints for communicating with NAO. The GET /messages endpoint allows the robot to check whether new MQTT messages have arrived, while the POST /audio endpoint receives the recorded customer speech. The latter validates and stores the WAV recording before forwarding it to the selected speech processing system. Once the customer's order has been identified, the backend publishes the corresponding drink ID to the configured MQTT topic. The UR controller can then use this information to select the appropriate drink preparation program. Both HTTP endpoints use an authentication token to restrict unauthorised access.

#### MQTT Listener

The MQTT Listener is implemented as a separate Python script within NAO's Choregraphe environment. Its purpose is to retrieve messages from the Flask backend and make the received information available to NAO's behaviour system. The script periodically checks the backend's /messages endpoint, using an interval of approximately two seconds, and places incoming messages into a local queue. Separate threads are used for checking the backend and processing the received messages, reducing the risk of blocking other operations while the robot is waiting for new information.

The intended workflow is that a table-detection message from the drone triggers NAO's order-taking behaviour. After receiving the corresponding information through the Flask backend, NAO can start the customer interaction and record the requested drink. The recorded audio is subsequently processed by the backend, which publishes the resulting drink ID to MQTT. This provides the connection between customer interaction and the UR robot's drink preparation process. The individual communication components have been developed, while their complete integration into the automated workflow remains part of the ongoing project work.

#### Challenges

One of our first challenges was becoming familiar with NAO and its programming environment. Since we had only a limited amount of time to learn how to use the robot before starting the project, implementing the required functionality involved considerable experimentation. We needed to understand how Choregraphe, NAO's Python 2.7 environment and its built-in audio modules work together. Even comparatively simple tasks, such as recording audio and transferring it to another computer, required additional research and testing because we had limited previous experience with these technologies.

The biggest technical challenge was establishing reliable network communication between NAO, the Flask backend and the MQTT infrastructure. During development, we encountered connectivity problems related to the university network, including difficulties with IP addresses and communication between devices. We also needed to establish a working backend environment that could communicate with NAO, access the necessary AI services and exchange information through MQTT. To address these issues, we worked towards a more reliable network architecture using a virtual machine, Mosquitto and Tailscale. This required repeated configuration changes, debugging and adaptation of our original technical approach.

Another significant challenge involved the reliability of the speech recognition process. We had to ensure that NAO could successfully record the customer's voice, transfer the audio file to the backend and receive a suitable response. During the earlier stages of development, several problems occurred while recording, transmitting and processing audio. Even after establishing basic communication, we needed to adjust the AI instructions and response handling to ensure that the system could correctly identify the supported drink orders.

Time management also became an important challenge throughout the project. Several tasks required more development time than initially expected, which resulted in unfinished work being transferred between iterations. Since we were working with multiple unfamiliar technologies, we frequently relied on documentation, experimentation, AI-assisted programming and extensive debugging to make progress. Although this approach helped us develop working solutions within a limited timeframe, it also required additional testing to verify the generated code and ensure reliable integration between components.

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

