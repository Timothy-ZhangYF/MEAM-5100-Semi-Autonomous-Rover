# Semi-Autonomous Arena PvP Battlebot

A semi-autonomous robot engineered for a 3v3 arena PvP battle game. Built on an ESP32-S3 microcontroller, the platform combines closed-loop RPM feedback, low-overhead Wi-Fi teleoperation, time-of-flight obstacle sensing, and a high-torque differential drive chassis designed to dominate physical engagements.

---

## Game Concept & Arena Rules

The competition takes place in a MOBA-style arena game (similar to *League of Legends*):
* **Base Towers:** Each team has a home base with physical buttons, which would damage the base HP when depressed.
* **Tower Damage:** the base buttons are guarded by a continuously slow spinning physical arm that feature an unavoidable attack radius.
* **Neutral Objective:** A central middle tower can be captured to execute automated, risk-free chip damage against the opponent's base.
* **Team Compositions:** 3 robots per team operating under autonomous routines, remote control (RC), or hybrid modes.
* **Target & Hit Detection:** Each robot is equipped with a designated target zone—an exposed physical whisker switch. Any physical strike to this switch deducts HP.
* **Offensive Weapons:** Robots can equip active physical attack mechanisms (such as a servo-driven sweeping arm), subject to strict length limit constraints.
* **Communication Penalty:** To discourage over-reliance on manual operation, each transmitted remote control data packet deducts **-1 HP**.

## Winning Strategy
As legendary Formula 1 designer Gordon Murray once said, "It’s not what the rule book says, it is what the rule book doesn't say that's important." Our strategy follows exactly this philosophy.




## 🛠 Hardware Architecture

* **Microcontroller:** ESP32-S3 (Station Mode Wi-Fi + I2C master)
* **Actuators:** 2× 12V Pololu 37Dx68L Metal Gearmotors (30:1 Gear Ratio, 330 RPM) + MG996R Servo (Arm)
* **Motor Driver:** DROK L298 Dual H-Bridge (7A continuous per channel)
* **Sensors:** 3× Adafruit VL53L1X Time-of-Flight (ToF) Distance Sensors (Front, Left, Right)
* **Power System:** 3S 11.1V 2200mAh 35C LiPo Battery + SparkFun 5V/2A Buck-Boost Converter
* **Chassis & Frame:** Laser-cut 1/8" MDF T-slot modular box frame with low center of gravity
* **Health Interface:** Custom Top Hat system interfaced via I2C for real-time HP reporting

---

## 💻 Software & Firmware Highlights

* **Closed-Loop Speed Control:** Implemented PID loop controls on motor encoders to maintain precise wheel RPM across heavy load and pushing states.
* **Autonomous Wall-Following:** Closed-loop PID navigation using left and front ToF distance sensors. Includes reading clamping algorithms (`INFINITE` range handling) to reject noise when pointing into open arena spaces.
* **Synchronized I2C Bus Management:** Configured dual-device I2C timing pipelines to handle high-speed ToF sensor polling alongside 40kHz Top Hat referee communications without bus lockups.
* **Low-Latency Teleoperation UI:** Integrated HTML/JavaScript keypress event listeners communicating directly over Wi-Fi sockets.

---

## 📸 System Schematics & Diagrams
