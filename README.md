# Semi-Autonomous Arena PvP Battlebot

An autonomous and remote-controlled combat robot engineered for a 3v3 arena PvP battle game. Built on an ESP32-S3 microcontroller, the platform combines closed-loop RPM feedback, low-overhead Wi-Fi teleoperation, time-of-flight obstacle sensing, and a high-torque differential drive chassis designed to dominate physical engagements.

---

## 🎮 Game Concept & Arena Rules

The competition takes place in a MOBA-style arena game (similar to *League of Legends*):
* **Base Towers & Nexus Buttons:** Each team has a home base defended by buttons. Depressing an opponent's nexus button reduces their base HP.
* **Tower Damage:** Towers possess an unavoidable attack radius that damages any bot operating within range.
* **Neutral Objective:** A central middle tower can be captured to execute automated, risk-free chip damage against the opponent's main tower.
* **Team Compositions:** 3 robots per team operating under autonomous routines, remote control (RC), or hybrid modes.
* **Communication Penalty:** To discourage continuous network flooding, each transmitted remote control data packet deducts **-1 HP** from the robot's health pool via the central referee system.

---

## ⚡ Winning Strategy: "The Bouncer"

Rather than relying on fragile mechanical attack mechanisms, our team optimized for physical field control, battery efficiency, and packet-frugal teleoperation:

1. **Interrupt-Driven RC Control:** Manual drive packets are transmitted **only** on keypress and key-release events (`keydown`/`keyup`). This drastically reduced network overhead and virtually eliminated packet penalty damage.
2. **High-Torque Closed-Loop Drive:** Utilizing 30:1 Pololu metal gearmotors with encoder-based RPM feedback control. This allowed the robot to operate at low linear speeds with maximum stall torque (up to 14 kg-cm), maintaining traction without tire slippage.
3. **Bully & Self-Sabotage Tactics:** With physical pushing dominance, our primary offensive tactic involved grappling opponent bots, overpowering their drive systems, and forcibly shoving them into their own base buttons. This forced enemy bots to tank their own tower's unavoidable radius damage while self-inflicting nexus button hits.

---

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
