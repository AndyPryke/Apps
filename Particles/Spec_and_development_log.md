# **Snowflake Engine: Master Specification & Development Log**

## **1. Project Vision**

A "programming game" inspired by Lemmings, where structural and mechanical problems are solved using modular hexagonal particles. Programming is expressed through the physical properties of the particle's six surfaces rather than text-based code.

## **2. Master Specification (Cumulative Requirements)**

### **Core Mechanics**

* **Hexagonal Topology:** Every particle is a hexagon with 6 distinct interaction surfaces.  
* **Surface Programming:** Each face is individually programmable with specific logic (Fixed, Spring, Anchor, Void, Passive).  
* **Emitter System:** A draggable "Source" bar allows the user to define the drop coordinate for crystals.  
* **Payload Delivery:** The primary goal is to guide "Payload Balls" to a destination cup using constructed machines or bridges.

### **Physics Requirements (User Hints & Constraints)**

* **True Physics:** The game should feel like it occurs in a real physical world
* **Vertex-Down Spawn:** Particles must drop point-first (angle 0\) to maintain a crystal aesthetic.  
* **Natural Settling:** Particles should not balance unrealistically on their points. They must tumble and settle flat on surfaces based on standard gravity and friction.  
* **Face-to-Face Snapping:** Active bonds (Fixed) must enforce a rigid hexagonal lattice, snapping particles into perfect alignment. There is a slight "magnetism" to interactions which pulls particles into place but only when this is physically possible.   
* **Collision Integrity:** Particles must not overlap or penetrate "shelves" (static platforms).

### **Surface Property Dictionary**

* **Passive (White):** Neutral; accepts bonds but does not initiate them.  
* **Fixed (Blue):** Rigid bond; creates a stiff structural lattice.  
* **Spring (Green):** Flexible bond; creates dampened mechanical connections.  
* **Anchor (Yellow):** Environmental lock; freezes the particle in space upon contact with ground or static structures.  
* **Void (Slate):** Non-stick; rejects all bonds and prevents adhesion.

## **3. Bug Report & Resolution History**

| Issue | User Observation | Resolution |
| :---- | :---- | :---- |
| **Overlap** | Particles mushing together under weight. | Increased sub-stepping (8x) and position iterations (30). |
| **Point Balance** | Particles balancing on vertices like needles. | Added small biased randomness to spawn and removed forced torques. |
| **Air Sticking** | Particles freezing in the air where released. | Lowered spawn point (Y=110) to clear top UI boundaries. |
| **Shelf Penetration** | Particles clipping into the ground before freezing. | Implemented manual separation resolution before setStatic(true). |
| **Sticking on Drop** | Particles snagging on invisible emitter zones. | Isolated emitter logic to visual-only; removed temporal grace periods. |

## **4. Pending Features (Requested Future Specs)**

* **Magnetic Poles:** Surface properties for attraction (+) and repulsion (-).  
* **Restitution Control:** Per-surface "Bounce" properties.  
* **Actuators/Motors:** Surfaces that apply active force or rotation.  
* **Sensor Logic:** Surfaces that change state based on external triggers.
* **Delayed Activation:** Surfaces which change state based on a timer. 

## **5. UI & UX Architecture**

* **Mobile-First Design:** Using 100dvh for full-screen reliability on mobile browsers.  
* **Chip Designer:** A central visual tool to cycle edge properties.  
* **Built-in Help:** A dictionary accessible via the HUD explaining all surface types.  
* **Real-time Legend:** Persistent color key for quick reference.
