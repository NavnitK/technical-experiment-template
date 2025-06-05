# 🧩 Modifiability Quality Attribute Requirement

## Problem Statement  
The system should allow developers to switch the user interface application to use a different map provider (e.g., from the current Google Maps to another) with minimal code changes, low risk of introducing bugs, and without impacting unrelated parts of the system.

## Specific Modifiability Issue to Solve  
- **Swapping Map Providers**  
  Enable easy replacement of the map component while keeping the rest of the UI stable and unaffected.

## Design Objective  
Design the Remote User Interface (RUI) to have loosely coupled components with well-defined interfaces to isolate changes and make the map provider interchangeable with minimal effort.

## Usability Tactics  
- **Isolate Map Service**  
  Encapsulate all map-related functionality behind an abstract interface.  
- **Dependency Injection**  
  Use runtime injection to provide the specific map provider to the UI, allowing easy replacement.

---

## Quality Attribute Scenario: Swapping Map Provider

- **Source**  
  Developer  
- **Stimulus**  
  Switch map provider from Google Maps to another provider  
- **Artifact**  
  Map rendering module in the Remote User Interface (RUI)  
- **Environment**  
  During platform upgrade or component refactoring  
- **Response**  
  New map provider integrates with minimal changes and no regressions in UI behavior  
- **Response Measure**  
  Integration completed in under 4 hours; no unintended side effects observed  

### 🛠 Approach 1: Abstract Common Services with Map Interface  
- Define a common abstract interface for map services that all map providers.  
- The RUI depends only on this interface, allowing the map provider to be swapped.  
- This reduces coupling between UI and map components and increases cohesion within map modules.  

### 🛠 Approach 2: Modularize Map Component with Dependency Injection  
- Encapsulate the map logic into a separate module injected at runtime into the RUI.  
- Changing the map provider involves injecting a new module conforming to the map interface without modifying the UI codebase.  
- This defers binding decisions to runtime, increasing flexibility and minimizing code ripple effects.  

### ⚖️ Tradeoff Analysis

| Quality Attribute | Abstract Map Interface                | Dependency Injection Module          |
|-------------------|------------------------------------|------------------------------------|
| Performance       | Very high; static binding          | Slight overhead due to runtime binding |
| Resiliency        | High; isolated map component       | High; modular and replaceable       |
| Extensibility     | Moderate; requires interface updates| High; new modules can be added easily |
| Modifiability     | Easy; confined to map interface     | Very easy; no UI changes required   |

### 🧠 Rationale for Approach Selection  
- ✅ **Abstract Common Services with Map Interface**  
  - Provides type safety and static guarantees.  
  - Well suited when map provider changes are infrequent.  
  - Encourages high cohesion within map functionality and reduces coupling to the UI.  
- ⚠️ **Modularize Map Component with Dependency Injection**  
  - Preferred if map providers are expected to change frequently or dynamically.  
  - Adds runtime flexibility at a small performance cost.  
  - Defers binding, easing future extensibility and maintenance.  

### 🏁 Final Recommendation  
> ✅ **Abstract Common Services with Map Interface** to enable low-risk, maintainable map provider swapping with minimal code changes.
