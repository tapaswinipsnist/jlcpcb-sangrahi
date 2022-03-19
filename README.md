# jlcpcb-sangrahi
![jlcpcb](https://user-images.githubusercontent.com/86110814/159132177-fde1f606-18cb-4122-aec7-0ed7cf22d0fa.png)
https://jlcpcb.com/rel
                                            REDHAAN REPORT
                                              SANGRAHI
                                            INTRODUCTION 
PROBLEM STATEMENT: Trash is collected at irregular intervals, causing uncleanliness and diseases. 
INTRODUCTION TO THE PROJECT :We have an idea that can help us solve the above-mentioned problem statement. Our Smart Bin,Sangrahi is an intelligent waste management system that helps keep its surroundings clean of trash. As our bot is autonomous, it decreases the workload on humans thereby making it better than our traditional garbage bins. It not only collects the trash, but also has other specifications like:
 ● Automatic open/close of lid
 ● Disinfects the area, comes with an inbuilt sanitizer
 ● Cleans the path using vacuum 
● Facilitated with a light which turns on when the lid is open
 ● Buzzes when the bag is full 
We will start by designing the chassis of the robot. Our robot will move autonomously using a sensor system and a joystick via Blynk application. We will have a mopping system underneath the bin which helps collect the dust on the floor, and disinfects the area using UV light. 

Hardware List:
Arduino Uno:
 Arduino Uno is a microcontroller board based on the ATmega328P (datasheet). It has 14 digital input/output pins (of which 6 can be used as PWM outputs), 6 analog inputs, a 16 MHz ceramic resonator (CSTCE16M0V53-R0), a USB connection, a power jack, an ICSP header and a reset button. 
IR Sensor:
 IR sensor is a simple electronic device which emits and detects IR radiation in order to find out certain objects/obstacles in its range. Some of its features are heat and motion sensing. IR sensors use infrared radiation of wavelength between 0.75 to 1000µm which falls between visible and microwave regions of the electromagnetic spectrum. 
DC Motor:
A DC Motor is the simplest kind of motor. It has two terminals or leads. When 8 connected with a battery the motor will rotate, and if the connections are reversed, the motor will rotate in the opposite direction. If the voltage across the terminals is reduced, the motor speed will reduce accordingly.
Stepper Motor:
A stepper motor, also known as step motor or stepping motor, is a brushless DC electric motor that divides a full rotation into a number of equal steps. The stepper motor is used for precise positioning with a motor, such as hard disk drives, robotics, antennas, telescopes, and some toys.
Servo Motor MG90: 
A Servo Motor is a small device that has an output shaft. This shaft can be positioned to specific angular positions by sending the servo a coded signal.
L298N Motor Driver: 
In a 15-lead Multi-watt and PowerSO20 system, the L298N is an incorporated solid circuit. It is a double full-connect driver for higher voltage , high ebb and flow designed to recognize typical TTL rational levels and drive inductive loads, such as transfers, solenoids, DC and venturing engines.
Jumpers:
Jumper wires are simple wires that have connector pins at each end, allowing them to be used to connect two points to each other without soldering.
RECHARGEABLE BATTERIES:
 Lithium-ion batteries are rechargeable batteries. A 2200mAh battery can supply 2.2A for one hour. Or about (12v * 2.2A =)26.4watts can be drained from the battery before it will need to be charged.
ULTRASONIC SENSOR:
An ultrasonic sensor is an electronic device that measures the distance of a target object by emitting ultrasonic sound waves, and converts the reflected sound into an electrical signal.
ESP32-CAM:
The ESP32-CAM is a small size, low power consumption camera module based on ESP32. It comes with an OV2640 camera and provides an onboard TF card slot.The ESP32-CAM can be widely used in intelligent IoT applications such as wireless video monitoring, WiFi image upload, QR identification, and so on.
SUBMERGED MOTORS
Micro DC 3-6V Micro Submersible Pump or submerged motor, pushes water into the pipe by converting rotary energy into kinetic energy into pressure energy.
UV-C light:
UV-C light is used for disinfecting the surface. It aims at preventing and reducing the spread of infectious diseases, viruses, bacteria, and other types of harmful organic micro-organisms in the environment by breaking down their DNA-structure.
Rack and Pinion:
A rack and pinion is a type of linear actuator that comprises a circular gear (the pinion) engaging a linear gear (the rack), which operates to translate rotational motion into linear motion.

 Software Architecture of the bot:
![cad1](https://user-images.githubusercontent.com/86110814/159131469-653f9c5c-db3d-4f50-af78-b8fe294216ba.jpeg)
![cad2](https://user-images.githubusercontent.com/86110814/159131605-ad940368-1302-4144-801e-d09d2356be50.jpeg)

Code:
#include "src/OV2640.h" 
#include #include
 #include // Select camera model //
#define CAMERA_MODEL_WROVER_KIT
 #define CAMERA_MODEL_ESP_EYE
 //#define CAMERA_MODEL_M5STACK_PSRAM 
//#define CAMERA_MODEL_M5STACK_WIDE
 //#define CAMERA_MODEL_AI_THINKER
 #include "camera_pins.h" 
#define SSID1 "wifiname" 
#define PWD1 "password" 
OV2640 cam;
 WebServer server(80); 
const char HEADER[] = "HTTP/1.1 200 OK\r\n" 
\ "Access-Control-Allow-Origin: *\r\n" \ "Content-Type: multipart/x-mixed-replace; 
boundary=12345678900000000000098765432 1\r\n"; 
const char BOUNDARY[] = "\r\n-- 123456789000000000000987654321\r\n"; const char CTNTTYPE[] = "Content-Type: image/jpeg\r\nContent-Length: "; 
const int hdrLen = strlen(HEADER); 
const int bdrLen = strlen(BOUNDARY); 
const int cntLen = strlen(CTNTTYPE); 
void handle_jpg_stream(void) { char buf[32]; 
int s; WiFiClient client = server.client();
 client.write(HEADER, hdrLen);
 client.write(BOUNDARY, bdrLen);

 while (true) { if (!client.connected()) break; cam.run();
 s = cam.getSize(); 
client.write(CTNTTYPE, cntLen);
 sprintf( buf, "%d\r\n\r\n", s ); 
client.write(buf, strlen(buf)); 
15 client.write((char *)cam.getfb(), s); 
client.write(BOUNDARY, bdrLen);
 } } 
const char JHEADER[] = "HTTP/1.1 200 OK\r\n" \ "Content-disposition: inline; 
filename=capture.jpg\r\n" \ "Content-type: image/jpeg\r\n\r\n";
 const int jhdLen = strlen(JHEADER);
 void handle_jpg(void) { WiFiClient client = server.client(); 
cam.run();
 if (!client.connected()) 
return;
 client.write(JHEADER, jhdLen);
 client.write((char *)cam.getfb(), cam.getSize());
 } 
void handleNotFound() { 
String message = "Server is running!\n\n";
 message += "URI: ";
 message += server.uri();
 message += "\nMethod: "; 
message += (server.method() == HTTP_GET) ? "GET" : "POST"; 
message += "\nArguments: "; 
message += server.args(); 
message += "\n"; server.send(200, "text / plain", message);
 } 
void setup() { 
Serial.begin(115200); 
//while (!Serial); 
//wait for serial connection. camera_config_t config;
 config.ledc_channel = LEDC_CHANNEL_0; 
config.ledc_timer = LEDC_TIMER_0; 
config.pin_d0 = Y2_GPIO_NUM; 
config.pin_d1 = Y3_GPIO_NUM; 
config.pin_d2 = Y4_GPIO_NUM;
 config.pin_d3 = Y5_GPIO_NUM;
 config.pin_d4 = Y6_GPIO_NUM; 
config.pin_d5 = Y7_GPIO_NUM;
 config.pin_d6 = Y8_GPIO_NUM; 
16 config.pin_d7 = Y9_GPIO_NUM; 
config.pin_xclk = XCLK_GPIO_NUM; 
config.pin_pclk = PCLK_GPIO_NUM;
 config.pin_vsync = VSYNC_GPIO_NUM;
 config.pin_href = HREF_GPIO_NUM;
 config.pin_sscb_sda = SIOD_GPIO_NUM;
 config.pin_sscb_scl = SIOC_GPIO_NUM; 
config.pin_pwdn = PWDN_GPIO_NUM;
 config.pin_reset = RESET_GPIO_NUM; 
config.xclk_freq_hz = 20000000; 
config.pixel_format = PIXFORMAT_JPEG;
 // Frame parameters
 // config.frame_size = FRAMESIZE_UXGA; 
config.frame_size = FRAMESIZE_QVGA;
 config.jpeg_quality = 12; config.fb_count = 2;
 #if defined(CAMERA_MODEL_ESP_EYE) 
pinMode(13, INPUT_PULLUP);
 pinMode(14, INPUT_PULLUP);
 #endif cam.init(config);
 IPAddress ip; 
WiFi.mode(WIFI_STA); 
WiFi.begin(SSID1, PWD1);
 while (WiFi.status() != WL_CONNECTED) {
 delay(500); 
Serial.print(F("."));
 }
 ip = WiFi.localIP();
 Serial.println(F("WiFi connected"));
 Serial.println(""); 
Serial.println(ip);
 Serial.print("Stream Link: http://");
 Serial.print(ip); 
Serial.println("/mjpeg/1"); 
server.on("/mjpeg/1", HTTP_GET, handle_jpg_stream);
 server.on("/jpg", HTTP_GET, handle_jpg); server.onNotFound(handleNotFound);
 server.begin();
 } 
void loop() {
 server.handleClient(); 
 #include 17
 #include AF_DCMotor rightBack(1);
 AF_DCMotor rightFront(2); 
AF_DCMotor leftFront(3);
 AF_DCMotor leftBack(4); 
Servo servoLook; 
byte trig = 2;
 byte echo = 13;
 byte maxDist = 150;
 byte stopDist = 15; 
float timeOut = 2*(maxDist+10)/100/340*1000000;
 byte motorSpeed = 55;
 int motorOffset = 10;
 int turnSpeed = 50;
 void setup() {
 rightBack.setSpeed(motorSpeed);
 rightFront.setSpeed(motorSpeed); leftFront.setSpeed(motorSpeed+motorOffset); leftBack.setSpeed(motorSpeed+motorOffset);
 rightBack.run(RELEASE);
 rightFront.run(RELEASE);
 leftFront.run(RELEASE); 
leftBack.run(RELEASE); 
servoLook.attach(10); 
pinMode(trig,OUTPUT);
 pinMode(echo,INPUT);
 } 
void loop() { 
servoLook.write(90);
 delay(750);
 int distance = getDistance();
 if(distance >= stopDist) {
 moveForward();
 } 
while(distance >= stopDist) {
 distance = getDistance(); delay(250);
 }
 stopMove();
 int turnDir = checkDirection();
 Serial.print(turnDir); 
18 switch (turnDir) {
 case 0: turnLeft (400);
     break;
 case 1: turnLeft (700);
     break;
 case 2: turnRight (400); 
     break;
 } }
 void moveForward() {
 rightBack.run(FORWARD); 
rightFront.run(FORWARD);
 leftFront.run(FORWARD);
 leftBack.run(FORWARD);
 } 
void stopMove() {
 rightBack.run(RELEASE); 
rightFront.run(RELEASE);
 leftFront.run(RELEASE);
 leftBack.run(RELEASE);
 } 
void turnLeft(int duration) { 
rightBack.setSpeed(motorSpeed+turnSpeed); rightFront.setSpeed(motorSpeed+turnSpeed); leftFront.setSpeed(motorSpeed+motorOffset+t urnSpeed); leftBack.setSpeed(motorSpeed+motorOffset+t urnSpeed); rightBack.run(FORWARD);
 rightFront.run(FORWARD);
 leftFront.run(BACKWARD);
 leftBack.run(BACKWARD); 
delay(duration); 
rightBack.setSpeed(motorSpeed); 
rightFront.setSpeed(motorSpeed);
 leftFront.setSpeed(motorSpeed+motorOffset);
 leftBack.setSpeed(motorSpeed+motorOffset); 
rightBack.run(RELEASE);
 rightFront.run(RELEASE);
 leftFront.run(RELEASE); 
19 leftBack.run(RELEASE); 
}
 void turnRight(int duration) { 
rightBack.setSpeed(motorSpeed+turnSpeed);
 rightFront.setSpeed(motorSpeed+turnSpeed);
 leftFront.setSpeed(motorSpeed+motorOffset+t urnSpeed); 
leftBack.setSpeed(motorSpeed+motorOffset+t urnSpeed); 
rightBack.run(BACKWARD); 
rightFront.run(BACKWARD);
 leftFront.run(FORWARD);
 leftBack.run(FORWARD);
 delay(duration); 
rightBack.setSpeed(motorSpeed); 
rightFront.setSpeed(motorSpeed);
 leftFront.setSpeed(motorSpeed+motorOffset);
 leftBack.setSpeed(motorSpeed+motorOffset);
 rightBack.run(RELEASE); 
rightFront.run(RELEASE);
 leftFront.run(RELEASE); 
leftBack.run(RELEASE); 
} 
int getDistance() { 
unsigned long pulseTime;
 int distance; digitalWrite(trig, HIGH);
 delayMicroseconds(10); 
digitalWrite(trig, LOW);
 pulseTime = pulseIn(echo, HIGH, timeOut);
 distance = (float)pulseTime * 340 / 2 / 10000;
 return distance;
 }
 int checkDirection() {
 int distances [2] = {0,0};
 int turnDir = 1;
 servoLook.write(180);
 delay(500);
 distances [0] = getDistance();
 servoLook.write(0); 
delay(1000); 
distances [1] = getDistance();
 if (distances[0]>=200 && distances[1]>=200)
 turnDir = 0;
 else if (distances[0]<=stopDist && distances[1]<=stopDist)
 turnDir = 1; 
else if (distances[0]>=distances[1]) 
turnDir = 0;
 
}
WORKING
Sangrahi is an  autonomous bot, which also moves with the help of  iot streaming viewed from a webpage and controlling it  through buttons   in the blynk app. Ultrasonic sensors help detect obstacles on the path as the bot moves and if any object detected it avoids it and further move on according to the environment. The lid of the bot opens when the IR sensor detects the hand and closes itself after a few seconds. The submerged motor draws the disinfectant liquid and wets the mop. This mop rotates as the bot moves  further  hence cleans the floor. The UV-C light dry disinfects the surface as the bot moves. When the garbage bag is full, the buzzer alarms, and the level is displayed in the blynk app. The disinfectant at the bottom layer needs to be replaced regularly. 

Practical Implementation & Output
After implementation of the bot , it moves accordingly avoiding the obstacles and cleans the floor as it moves. This can be monitored through a webpage whose link is provided in the serial monitor once the program is executed. Below picture shows the streaming example. 
![imp1](https://user-images.githubusercontent.com/86110814/159131785-06c0d630-de46-4ea6-8b64-a29594ac8833.png)
And the belo one shows the esp32 cam module taking continuous picture(.jpeg)s which are viewed in form of video(.mjpeg).
![imp2](https://user-images.githubusercontent.com/86110814/159131789-f4e6db93-a73c-44c3-88a4-6809bbe9a00e.png)
And the bot can be controlled by buttons on the blynk app. The filling display level is also viewed properly on the blynk app. The below picture shows the details of icons on the blynk app.
 ![imp4](https://user-images.githubusercontent.com/86110814/159131854-23c8fbde-c5db-46e4-9831-71e4bfc7b21c.jpg)
                                                          Existing Competitors
Our bot is different from the existing smart bins as it has the combined facility of lid opening , automatic trash bag cutting, mopping ,moves automatically  and can be controlled from anywhere near.
Some examples of the existing competitors related to this are
1)	Smart dual dustbin model for waste management in smart cities. G Sai Rohit, M Bharat Chandra, Shaurabh Saha, Debanjan Das.2018 3rd International Conference for Convergence in Technology (I2CT), 1-5, 2018
2)	Intelligent trash bin (ITB) with trash collection efficiency optimization using IoT sensing.Hamza Sohail, Saif Ullah, Asim Khan, Omar Bin Samin, Maryam Omar2019 8th International Conference on Information and Communication Technologies (ICICT), 48-53, 2019
3)	i-BIN: An Intelligent Trash Bin for Automatic Waste Segregation and Monitoring System. Miko Pamintuan, Shiela Mae Mantiquilla, Hillary Reyes, Mary Jane Samonte.2019 IEEE 11th International Conference on Humanoid, Nanotechnology, Information Technology, Communication and Control, Environment, and Management (HNICEM), 1-5, 2019.
4)	Smart Garbage Bin–A Waste Management System. Nayan Patel, Omkar Mugdum, Tirthesh S Pawar, Deepshikha Chaturvedi, Shashikant S Radke. Asian Journal For Convergence In Technology (AJCT) ISSN-2350-1146 6 (3), 100- 104, 2020.
5)	 Smart dustbin using GPS tracking. Sonali Joshi, Uttkarsh Kumar Singh, Sahil Yadav. Int. Res. J. Eng. Technol 6, 165-170, 2019 .
6)	A Smart Dustbin using Mobile Application. CS Anilkumar, G Suhas, S Sushma. International Journal of Innovative Technology and Exploring Engineering (IJITEE), ISSN, 3964-3967, 2019.
7)	 Arduino Based Smart Dustbin For Waste Management system. Badri Narayan Mohapatra, Pranav Shirapuri. Perspectives in Communication, Embedded-systems and Signal-processing-PiCES 4 (3), 8-11, 2020.
8)	 IOT Based Smart Dustbin Monitoring With Tracking System Using ATMega 2560 Microcontroller. Mohammad Abbas Hussain, Kvs Nikhil, Koppuravuri Yaswanth Pavan Kalyan. 2019 Fifteenth International Conference on Information Processing (ICINPRO), 1-6, 2019 .
9)	IOT based garbage clearance alert system with GPS location using Arduino. Shareefa Ahmad Abu Shahada, Suzan Mohammed Hreiji, Shermin Shamsudheen. International Journal of MC Square Scientific Research 11 (1), 1-8, 2019

                                                                  CONCLUSION
As per the problem stated above, we together as a team made an attempt on solving the issue by constructing the SANGRAHI- a smartbin. During this project, we were involved in three basic types of tasks – electronic, mechanical and computer engineering. Using the mechanical aspect of engineering, we designed a four-wheel robot which can perform the task of movement, mopping, and lid opening. We also used the electronic aspect by implementing the buzzer system, camera module, where ESP 32 is used. IR sensors have been used for the detection of the movement and lid opening. Furthermore, we used many aspects of computer engineering interchangeably to program the robot so that it can perform the task swiftly and successfully. The programming part was slightly tricky and needed many trials for the best performance of the bot. We would like to conclude by saying that our bot helps keep the environment clean.
                                                          Applications & Future Scope
Our bot has many applications where it can be used, like offices, smart homes and any places .It is a great tool which can reduce the required man power. Here are some of the scope of future enhancements which can be made to our bot.
1.Sanitizer: A sanitizer can be installed in the robot’s body which runs on IR sensors. It will be utilized after a person disposes of the trash.
 2.Computer Vision: Trash can be identified and detected using computer vision.CV will also help the bot’s movement.
 3.Auto Disposal: The trash bag is going to be automatically sealed and disposed of  by the bot itself. 
4.Vacuum: The robot will vacuum the floor.



