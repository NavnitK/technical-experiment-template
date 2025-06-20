# Why describe one view with multiple UML diagrams?
**1. Different Diagrams Show Different Aspects**  
  - For example, in a Client-Server View:  
    - A Component Diagram shows the static components and interfaces.
    - A Sequence Diagram shows how messages flow during runtime interactions.
    - A Deployment Diagram shows where client and server physically reside.
    - Each diagram highlights a different facet of the same view.

**2. Clarity & Completeness**  
  - One diagram alone can’t capture all the complexities.
  - Combining diagrams provides a fuller, clearer picture of both structure and behavior.

**3. Audience Needs Vary**  
  - Developers might focus on class or component diagrams.
  - Network engineers might need deployment diagrams.
  - QA or analysts might want sequence diagrams for behavior validation.

**4. Reduce Complexity**  
  - Trying to cram all info into one diagram makes it cluttered and hard to understand.
  - Splitting concerns across diagrams keeps each focused and easier to read.

**5. Better Communication**  
  - Multiple views cater to different stakeholder perspectives, improving overall communication.

# Architectural Views and UML Diagrams

- **Module View** (Static)
  - Layered View
    - Package Diagram
    - Class Diagram (within layers)
    - Component Diagram
  - Decomposition View
    - Package Diagram
    - Class Diagram
    - Component Diagram
  - Uses View
  - Ownership View
  - Class/Package View
    - Class Diagram
    - Package Diagram

- **Component & Connector View**
  - Client-Server View
    - Static: Component Diagram, Deployment Diagram
    - Dynamic: Component Diagram, Sequence Diagram
  - Pipe-and-Filter View
    - Static: Component Diagram, Activity Diagram
    - Dynamic: Sequence Diagram, Communication Diagram, Activity Diagram
  - Publish-Subscribe View
    - Static: Component Diagram
    - Dynamic: Sequence Diagram, Communication Diagram
  - Service-Oriented View
    - Static: Component Diagram
    - Dynamic: Sequence Diagram, Activity Diagram
  - Peer-to-Peer View
    - Static: Component Diagram, Deployment Diagram
    - Dynamic: Sequence Diagram, Communication Diagram, Activity Diagram
  - Shared Data View
    - Static: Class Diagram, Component Diagram, Deployment Diagram
    - Dynamic: Sequence Diagram, Activity Diagram, Communication Diagram

- **Deployment View**
  - Node Allocation View
    - Static: Deployment Diagram
    - Dynamic: Sequence Diagram (optional)
  - Network Topology View
    - Static: Deployment Diagram
    - Dynamic: Sequence Diagram, Communication Diagram
  - Physical View
    - Static: Deployment Diagram
    - Dynamic: Sequence Diagram, Communication Diagram
  - Virtualized/Cloud View
    - Static: Deployment Diagram, Component Diagram
    - Dynamic: Sequence Diagram, Activity Diagram, Communication Diagram
  - Redundancy/Fault Tolerance View
    - Static: Deployment Diagram, Component Diagram
    - Dynamic: Sequence Diagram, Communication Diagram, Activity Diagram
