# **Particle Surface Property Guide**

Each of the 6 faces of a Snowflake crystal can be programmed with specific interaction logic. Use the **Chip Designer** to cycle through these properties.

### **1. Passive (White)**

* **Behavior:** Neutral receptor.  
* **Logic:** Does not initiate connections on its own. It acts as a standard physical surface that other "Active" surfaces (Fixed or Spring) can latch onto.

### **2. Fixed Bond (Blue)**

* **Behavior:** Rigid structural link.  
* **Logic:** When this face touches a **Passive**, **Fixed**, or **Spring** face, it creates a high-stiffness constraint. This is the primary tool for building sturdy bridges, towers, and chassis.

### **3. Spring Bond (Green)**

* **Behavior:** Flexible mechanical link.  
* **Logic:** Creates a low-stiffness constraint with damping. Useful for suspension systems, soft joints, or "living" structures that need to sag or bounce under weight.

### **4. Anchor Point (Yellow)**

* **Behavior:** Environmental lock.  
* **Logic:** If this specific face touches the **Ground** or any **Static Particle**, the entire particle freezes in place (becomes static). Use this to "nail" your structure to the cliffside.

### **5. Void (Slate)**

* **Behavior:** Anti-stick / Non-reactive.  
* **Logic:** This face ignores all bond requests and refuses to participate in structural snapping. It behaves like a low-friction plastic edge. Use this to prevent parts of your machine from sticking to each other accidentally.
