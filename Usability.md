# 🧩 Usability Quality Attribute Requirement
# Problem statement
The software system can be easily understood, corrected, improved, and adapted over time.
For a user interface, the goal is not to exhibit defects or errors.
An essential element to achieve this goal includes a codebase that is clean, well-documented,
has good test coverage, and is structured in a way that simplifies debugging and enhancements.

# Specific Usability Issues to Solve
The Remote User Interface (RUI) is intended for educational use by air traffic control students, who must interact with and analyze dynamic aircraft data in both real-time and playback modes. To ensure the system is intuitive, efficient, and minimizes cognitive load during operation, the following usability challenges have been identified:
- **Data Overwhelm During Live Monitoring**  
  Allow users to pause updates to analyze specific aircraft without distraction.
- **Repetitive Reconfiguration**  
  Preserve user preferences like IP address and map style across sessions.
- **Seamless Interaction Experience**  
  Enable smooth operation despite network or hardware disruptions.

# Design Objective
Design a responsive and intuitive system that enables efficient analysis of aircraft data, while ensuring minimal disruption and persistent user preferences.

# Usability Tactics
- **Pause/Resume**  
  Users shall be able to pause live aircraft data updates to analyze specific tracks without distraction, then resume updates smoothly.
- **Maintain User Model**  
  The system shall remember user preferences such as Raspberry Pi IP address, map style, and display settings across sessions to minimize repetitive setup and enhance efficiency.

## Quality Attribute Scenarios: Pause/Resume
- **Source**  
  User
- **Stimulus**  
  User requests to pause live data stream
- **Artifact**  
  Remote User Interface (RUI) data display
- **Environment**  
  Normal operation with active live data feed
- **Response**  
  RUI pauses data updates while maintaining current display; on resume, data updates continue without loss
- **Response Measure**  
  Data stream pauses/resumes within 1 second; no data loss or UI glitches observed

### Approach 1: Deferred Data Rendering
- The system continuously receives live ADS-B data, but when the user pauses the view, incoming data is held back from immediate display and stored temporarily.
- When the user resumes, the stored data is rendered on the UI in a controlled manner, ensuring smooth updates without overwhelming the user.
- The buffer size is managed to prevent memory overload by discarding the oldest data as needed.

### Approach 2: Adaptive Data Throttling at Source
- The Raspberry Pi Flight Tracker adjusts its data transmission rate based on the UI state.
- When paused, the Pi reduces or temporarily halts data streaming to the user interface.
- On resume, the full data stream is restored for real-time updates.

### Tradeoff Analysis
| Quality Attribute | Deferred Data Rendering                   | Adaptive Data Throttling at Source              |
|-------------------|------------------------------------------|------------------------------------------------|
| **Performance**   | Fast UI response; data still arrives smoothly | May introduce lag due to changing data rates     |
| **Resiliency**    | High; data always collected locally       | Moderate; depends on reliable communication      |
| **Extensibility** | Moderate; buffering logic localized to UI | Limited; requires changes to source data system |
| **Modifiability** | Easier to change within UI component       | More complex; source and UI must coordinate      |


Quality Attribute	Deferred Data Rendering	Adaptive Data Throttling at Source
Performance	Fast UI response; data still arrives smoothly	May introduce lag due to changing data rates
Resiliency	High; data always collected locally	Moderate; depends on reliable communication
Extensibility	Moderate; buffering logic localized to UI	Limited; requires changes to source data system
Modifiability	Easier to change within UI component	More complex; source and UI must coordinate

### 🧠 Rationale for Approach Selection
- ✅ **Deferred Data Rendering**  
  - **Pattern Used:** Observer + Command Pattern
  - Continuous data capture is important, regardless of UI state.
  - Quick UI responsiveness with minimal complexity is required.
  - Modifications should be limited to the UI side.
- ⚠️ **Adaptive Data Throttling at Source**  
  - Reducing unnecessary data transmission is important (e.g., bandwidth or power saving).
  - There is flexibility and capability to modify the Raspberry Pi software.
  - Close coordination between source and UI is manageable.

### 🏁 Conclusion & Recommendation
After evaluating both architectural approaches against key quality attributes — **performance**, **resiliency**, **extensibility**, and **modifiability** — the recommended solution for implementing Pause/Play functionality is:
> ✅ **Deferred Data Rendering**

## Quality Attribute Scenario: Maintain User Model
- **Source**  
  User  
- **Stimulus**  
  User restarts or revisits the Remote User Interface (RUI)  
- **Artifact**  
  User preferences and interface settings (e.g., map type, IP address, zoom level)  
- **Environment**  
  Normal operation after UI reload or system reboot  
- **Response**  
  RUI restores previously saved settings automatically  
- **Response Measure**  
  Preferences are restored instantly upon startup; user does not need to reconfigure settings manually

### 🛠 Approach 1: Local Settings Persistence (UI-based)
- User preferences (e.g., IP address, map view, display options) are saved to a local configuration file (e.g., JSON).
- When the UI loads, it reads the config file and restores the previous session settings automatically.
- Updates to settings are written back on user change or on safe-close.

### 🛠 Approach 2: Cloud-Based User Model (via External Storage)
- User settings are stored in a cloud storage solution (e.g., Firebase, BigQuery, or a lightweight backend API).
- On startup, the UI queries the external service for the user’s saved settings.
- Ideal for multi-device usage or centralized preference management.

### ⚖️ Tradeoff Analysis
| Quality Attribute | Local Settings Persistence                 | Cloud-Based User Model                           |
|-------------------|--------------------------------------------|--------------------------------------------------|
| **Performance**   | Very fast; low latency on read/write       | Slight delay due to network access               |
| **Resiliency**    | Strong offline support                     | Dependent on network availability                |
| **Extensibility** | Moderate; localized to UI                  | High; supports syncing across devices/users      |
| **Modifiability** | Easy to change and debug locally           | Requires external system and authentication logic|


### 🧠 Rationale for Approach Selection
- ✅ **Choose _Local Settings Persistence_ if**:
  - **Pattern Used:** Singleton + Repository Pattern
  - You want a **lightweight and fast** solution.
  - The system is **used on a single device** (e.g., student laptop).
  - Offline use and simplicity are more important than multi-user sync.
  - You want to **avoid dependency on external services**.
- ⚠️ **Choose _Cloud-Based User Model_ if**:
  - You need **settings to sync across devices or users**.
  - The application will eventually be **multi-user or web-based**.
  - The network is reliable, and cloud integration is acceptable.
  - You want to **centralize configuration management**.

### 🏁 Conclusion & Recommendation
After evaluating both architectural approaches against key quality attributes — **performance**, **resiliency**, **extensibility**, and **modifiability** — the recommended solution for implementing the Maintain User Model functionality is:
> ✅ **Local Settings Persistence (UI-based)**
