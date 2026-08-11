# Semi-Autonomous Arena PvP Battlebot

An autonomous and remote-controlled combat robot engineered for a 3v3 arena PvP battle game. Built on an ESP32-S3 microcontroller, the platform combines closed-loop RPM feedback, low-overhead Wi-Fi teleoperation, time-of-flight obstacle sensing, and a high-torque differential drive chassis designed to dominate physical engagements.

---

## 🎮 Game Concept & Arena Rules

The competition takes place in a MOBA-style arena game (similar to *League of Legends*):
* **Base Towers:** Each team's base is surrounded by physical buttons. Depressing an opponent's  button deducts base HP.
* **Tower Hazards:** Base buttons are guarded by a slowly spinning, high-damage physical arm that enforces an unblock-able attack radius.
* **Neutral Objective:** A central tower can be captured to deal automated, risk-free chip damage to the opponent's base.
* **Team Compositions:** 3 robots per team (built by 3 separate student groups) operating under autonomous, remote control (RC), or hybrid modes.
* **Hit Detection & Respawns:** Each robot carries an exposed whisker switch as its target zone. Strikes to this switch deduct HP; reaching 0 HP forces a timed respawn penalty.
* **Offensive Weapons:** Active mechanisms (e.g., servo-driven sweeping arms) are permitted within strict length constraints.
* **Communication Penalty:** To discourage continuous manual driving, every transmitted RC packet deducts **-1 HP** from the bot's health pool.

---

## Winning Strategy: Loopholes

> *"It’s not what the rule book says, it is what the rule book doesn't say that's important."*  
> — **Gordon Murray**

Our strategy focused on identifying rule loopholes to legally bypass system limitations—specifically engineering around the **RC health penalty** and the **tower hazard radius**.

### 1. Interrupt-Driven Teleoperation (Bypassing the Health Drain)
* **The Constraint:** Standard RC controllers continuously stream motion packets at high frequencies (e.g., 10 packets/sec), which would drain a bot's total HP within 10 seconds. Conversely, autonomous navigation suffered from unreliable arena localization.
* **The Solution:** We replaced polling-based streaming with an **interrupt-based input architecture**. Packets were transmitted *only* on keyboard event triggers (`keydown` / `keyup`).
* **The Impact:** A complete directional maneuver required only 2 packets (start/stop) instead of dozens. This gave us a budget of over 50 discrete maneuvers per respawn, eliminating the need for fragile autonomous pathfinding while maintaining near-zero HP penalty.

### 2. High-Torque Closed-Loop Drive (Ramp & Pushing Dominance)
* **The Constraint:** To climb the arena ramps and overpower opponents in collisions, standard cheap brushed DC motors typically stall or slip.
* **The Solution:** We selected high-gear-ratio motors (30:1 reduction) and implemented a custom **encoder-based closed-loop PID controller**.
* **The Impact:** Maintained maximum available torque at low rotational speeds, preventing tire slippage and enabling precise, high-traction positional control.

---

## 🛠 Strategic Evolution: "Brick" to "Human Shield"


### The Initial Plan: The Deployable "Brick"
Our original concept was to carry a weighted payload up the ramp and drop it onto the opponent's base button. This would trigger continuous nexus damage without requiring our bot to remain in the hazard zone or expend HP (abandoned due to late-stage hardware repairs and assembly time constraints).

### The Adaptation: "Brick 2.0" (The Human Shield Tactic)
During competition matches, we adapted our high-torque drive and precise teleoperation into an offensive grappling strategy:
1. **Grapple & Overpower:** Using our closed-loop torque advantage, we physically rammed into opposing bots and pushed them backward into their own base.
2. **Pinned Traps:** We held enemy bots directly against their own base buttons, forcing them to trigger button damage on their own nexus.
3. **Friendly Fire Exploitation:** By holding the enemy bot in place, the spinning tower arm struck the *opponent* repeatedly instead of us, turning their defense system into self-inflicted damage.
