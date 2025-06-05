# 🧩 Modifiability Quality Attribute Requirement

## Problem Statement  
The system should support easy integration of new features and changes (e.g., detection modules, UI components like maps) without requiring major rewrites or introducing regressions. It must be adaptable to evolving requirements and developer environments.

## Specific Modifiability Issues to Solve  
- **Adding New Functionalities**  
  Allow plug-and-play integration of features like aircraft anomaly detection.  
- **Swapping Components**  
  Make components like the map service easily replaceable without breaking the UI.

## Design Objective  
Create a system architecture that separates concerns, encourages modularity, and minimizes interdependencies, allowing safe and efficient modifications or extensions.

## Usability Tactics  
- **Extensible Feature Integration**  
  Design the system to allow new detection modules or features to be added with minimal impact on existing code.  
- **Swappable Components**  
  Architect components such as the map provider to be easily replaceable with minimal code changes and risk.

---

## Quality Attribute Scenario: Extensible Feature Integration

- **Source**  
  Developer  
- **Stimulus**  
  Add anomaly detection feature  
- **Artifact**  
  Aircraft data processing and UI display  
- **Environment**  
  During iterative development  
- **Response**  
  New detection module is added with minimal changes to existing code  
- **Response Measure**  
  No regression bugs, integration completed in under one day  

### 🛠 Approach 1: Plug-in Architecture (Microkernel)
- Separate core processing from optional modules.
- Each module implements a shared interface.
- New detectors (e.g., flight zone violations) are added without modifying core logic.

### 🛠 Approach 2: Event-Driven Observer Model  
- Core system emits aircraft tracking events.
- Listeners (detection modules) subscribe and react to specific events.
- Modules can be added or removed without altering the core emitter.

### ⚖️ Tradeoff Analysis

| Quality Attribute | Plug-in Architecture             | Event-Driven Observer Model          |
|-------------------|---------------------------------|-------------------------------------|
| Performance       | Slight overhead for module calls | High performance                    |
| Resiliency        | High isolation of faulty modules | Depends on observer error handling  |
| Extensibility     | Very high, supports 3rd-party modules | High, event listeners easily added |
| Modifiability     | Excellent, changes isolated       | Good, requires event contract management |

### 🧠 Rationale for Approach Selection  
- ✅ **Plug-in Architecture**  
  - Frequent addition of new modules expected.  
  - Clear separation between core and extensions.  
  - **Patterns used:** Microkernel, Strategy  
- ⚠️ **Event-Driven Observer Model**  
  - Lightweight behavior injection.  
  - System behavior driven by events.  
  - **Patterns used:** Event-Driven, Observer  

### 🏁 Final Recommendation  
> ✅ **Plug-in Architecture (Microkernel)** for modular growth and safe extensibility.

---

## Quality Attribute Scenario: Swappable Components

- **Source**  
  Developer  
- **Stimulus**  
  Switch from Mapbox to OpenLayers map provider  
- **Artifact**  
  Map rendering module in UI  
- **Environment**  
  During platform upgrade or rebranding  
- **Response**  
  New map service integrates without affecting other UI parts  
- **Response Measure**  
  Integration completed in under 4 hours; unrelated UI unaffected  

### 🛠 Approach 1: Adapter Pattern with Abstract Map Interface  
- Define a `MapService` interface for UI components.  
- Mapbox, OpenLayers, Leaflet implement this interface.  
- Swap map providers by replacing implementation without affecting other code.

### 🛠 Approach 2: Plugin-based UI Components  
- Treat maps as injectable UI plugins.  
- Map modules register at runtime with a UI factory or slot.  
- Decouples the map component from other system parts.

### ⚖️ Tradeoff Analysis

| Quality Attribute | Adapter + Interface             | Plugin-based UI Component           |
|-------------------|--------------------------------|-----------------------------------|
| Performance       | Very high, statically bound     | Slight runtime overhead            |
| Resiliency        | High; map swap isolated         | High; maps are isolated modules    |
| Extensibility     | Moderate; requires interface implementations | High; new maps self-register       |
| Modifiability     | Easy; confined changes          | Very easy; no impact on other code |

### 🧠 Rationale for Approach Selection  
- ✅ **Adapter Pattern with Interface Abstraction**  
  - Prefer type-safe, static code.  
  - Map provider unlikely to change often.  
  - **Patterns used:** Adapter, Interface Segregation  
- ⚠️ **Plugin-based Component**  
  - Frequent UI plugin changes expected.  
  - Need runtime flexibility.  
  - **Patterns used:** Factory, Plugin  

### 🏁 Final Recommendation  
> ✅ **Adapter Pattern with Interface Abstraction** for predictable and low-risk provider swapping.
