# Semi-Autonomous Arena PvP Battlebot

An autonomous and remote-controlled combat robot engineered for a 3v3 arena PvP battle game. Built on an ESP32-S3 microcontroller, the platform combines closed-loop RPM feedback, low-overhead Wi-Fi teleoperation, time-of-flight obstacle sensing, and a high-torque differential drive chassis designed to dominate physical engagements.

---

## 🎮 Game Concept & Arena Rules

The competition takes place in a MOBA-style arena game (similar to *League of Legends*):
* **Base Towers:** Each team's base is surrounded by physical buttons. Depressing an opponent's  button deducts base HP.
* **Tower Hazards:** Base buttons are guarded by a slowly spinning, high-damage physical arm that enforces an unblock-able attack radius.
* **Neutral Objective:** A central tower can be captured to deal automated, risk-free chip damage to the opponent's base.
* **Team Compositions:** 3 robots per team (built by 3 separate student groups) operating under autonomous, remote control (RC), or hybrid modes.
* **Hit Detection & Respawns:** Each robot carries an exposed whisker switch as its target zone. Strikes to this switch deduct HP; reaching 0 HP forces a timed respawn penalty.
* **Offensive Weapons:** Active mechanisms (e.g., servo-driven sweeping arms) are permitted within strict length constraints.
* **Communication Penalty:** RC-mode is meant for emergencies/manual corrections. To discourage continuous manual driving, every transmitted RC packet deducts **-1 HP** from the bot's health pool.

---

## Core Winning Strategy: "Creative Exploitations"

> *"It’s not what the rule book says, it is what the rule book doesn't say that's important."*  
> — **Gordon Murray**

Our strategy focused on identifying rule loopholes and specifically engineering around them legally: **RC health penalty** and the **tower hazard radius**.

### 1. Interrupt-Driven Teleoperation (Bypassing the Health Drain)
* **The Constraint:** Standard RC controllers continuously stream motion packets at high frequencies (e.g., 30 packets/sec), which would drain a bot's total HP ~3 seconds. Conversely, autonomous navigation suffered from unreliable arena localization (ruleset limitation + arena design flaw).
* **The Solution:** We replaced polling-based streaming with an **interrupt-based input architecture**. Packets were transmitted *only* on keyboard event triggers (`key_press` / `key_release`). It uses WASD controls for easy general purpose piloting. Pre-programmed complex commands such as wall following, or dead-reckoning straight to opponent tower could be activated with 1 packet, further increasing efficiency and ease of use.
* **The Impact:** A complete directional maneuver required only 2 packets (start/stop) instead of dozens. This gave us a budget of over 50 discrete maneuvers per respawn. Combining pre-programmed commands with manual piloting, we would consistently reach opponent base tower with less than 10 packets (5 commands). This however does mean we lose analog speed control (traditional joysticks), but we deemed it acceptable in our strategy. 


### 2. The Deployable "Brick"
* **The Constraint:** To avoid tower defense radius, the rover must move out of the way, or need to tank damage and respawn, both of which causes attack downtime. 
* **The Solution:** Deploying a "Brick" to hold down the button, and leaving the hazard zone entirely, risking at most 1 hit from the tower (This becomes especially easy since we have near full RC capabilities)
* **The Impact:** Our rover is under 0 risk from the towers once brick deployed, which has no attack downtime. Furthermore, our rover is then free to capture the center tower for extra continuous chip damage. Together, this gives us the theoretical fastest time to kill on opponent towers, hence unbeatable strategy.
  

### 2.1 High-Torque Closed-Loop Drive
* **The Constraint:** To climb the arena ramps and overpower opponents in collisions with our added brick weight, standard cheap brushed DC motors typically stall. 
* **The Solution:** We selected high-gear-ratio large motors and implemented a custom **encoder-based closed-loop PID speed controller**.
* **The Impact:** Maintained maximum available torque at low rotational speeds, preventing tire slippage and enabling precise, high-traction positional control. Our heavy bot + closed-loop control meant that when we collide with an opponent, we always had more grip + our motors adjusted PWM to maintain target RPM, completely overpowering open-loop, high-speed drives that immediately stalled or lost traction.

---

## Strategic Evolution: "Brick 2.0"

### Unexpected repairs
The few days before final match, our rover spontaneously burn out our power regulators, necessitating a rebuild. This took precious time from implementing brick mode. We decided against potentially risking burning out our rebuild with the extra electrical loads of the deployment mechanism. Unfortunately we were down a core winning exploit, which was also our "funny" trick.

### Adaptation
During competition matches, we adapted our high-torque drive and precise teleoperation into an offensive grappling strategy, one where we turned any opponent in our path into our "brick"
1. **Grapple & Overpower:** Using our closed-loop torque advantage, we physically rammed into opposing bots and pushed them backward into their own base.
2. **Pinned Traps:** We held enemy bots directly against their own base buttons, forcing them to trigger button damage on their own nexus.
3. **Friendly Fire Exploitation:** By holding the enemy bot in place, the spinning tower arm struck the *opponent* repeatedly instead of us, turning their defense system into self-inflicted damage.

