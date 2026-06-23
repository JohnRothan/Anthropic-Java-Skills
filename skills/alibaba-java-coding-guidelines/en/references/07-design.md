# 7. Design Specification

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

 1. **[Mandatory]** The design of the storage solution and the underlying data structures must be reviewed and unanimously approved, and must be captured in documentation.
Note: A flawed underlying data structure tends to increase system risk and reduce extensibility, and refactoring costs also rise sharply due to historical data migration and the smooth transition of the system. Therefore, the storage solution and data structures need to be carefully designed and reviewed, and a double check is required after they are committed and executed in the production environment.
Positive example: The review covers storage media selection; whether the table structure design can satisfy the technical solution; whether the access performance and storage space can satisfy business growth; the dialectical relationships among tables or fields; field names; field types; indexes, etc. Data structure changes (such as adding new fields to an existing table) must also go live only after passing review.

 2. **[Mandatory]** In the requirements analysis phase, if more than one category of User interacts with the system and there are more than 5 related UseCases, use a use case diagram to express the structured requirements more clearly.

 3. **[Mandatory]** If a business object has more than 3 states, use a state diagram to express them and clearly define the trigger conditions for each state change.
Note: The core of a state diagram is object state. First, clarify how many states the object has; then determine whether a direct transition relationship exists between each pair of states; then clarify what the conditions are that trigger state transitions.
Positive example: A Taobao order has states such as order placed, awaiting payment, paid, awaiting shipment, shipped, and received. For example, there cannot be a direct transition relationship between the two states "order placed" and "received".

 4. **[Mandatory]** If the call chain of a certain function in the system involves more than 3 objects, use a sequence diagram to express it and clearly define the input and output of each call link.
Note: A sequence diagram reflects the interaction and collaboration relationships among a series of objects, clearly and three-dimensionally reflecting the depth of the system's call chain.

 5. **[Mandatory]** If there are more than 5 model classes in the system with complex dependency relationships, use a class diagram to express them and clearly define the relationships among the classes.
Note: A class diagram is like a construction drawing in the building field. If you are building a bungalow, you may not need it, but if you are constructing the Ant Z-Space Building, you definitely need detailed construction drawings.

 6. **[Mandatory]** If there are collaboration relationships among more than 2 objects in the system and a complex processing flow needs to be represented, use an activity diagram to represent it.
Note: An activity diagram is an extension of the flowchart, adding object swimlanes that can reflect collaboration relationships, and it supports representing concurrency, etc.

 7. **[Mandatory]** During system design, accurately identify weak dependencies and design targeted degradation and contingency plans to ensure that the core system remains available.
Note: If, after a third-party service that the system depends on is degraded or blocked, the main process can still continue without being affected—only non-critical functions such as information display or message notification are affected—then these services are called weak dependencies.
Positive example: When a system weakly depends on multiple external services, if a downstream service takes too long, it will severely affect the current caller. Corresponding degradation measures must be taken. For example, when the average response time or error rate of a downstream service call in the call chain exceeds a threshold, the system automatically performs degradation or circuit-breaking operations, blocking the negative impact of the weak dependency and protecting the availability of the current system's main functions.
Counter-example: A certain pandemic-related QR code failed with the message "The server is a bit busy, please try again later", and the unavailability lasted a long time, drawing high social attention. The cause may have been that the RT of the external dependency service being called was too high, causing the system to appear frozen, while the display end had no degradation plan and could only throw an error directly to the user.

 8. **[Recommended]** When designing the system architecture, clearly define the following objectives:
- Determine the system boundary. Determine what the system does and does not do at the technical level.
- Determine the relationships among modules within the system. Determine the dependency relationships among modules and the macro-level inputs and outputs of modules.
- Determine the principles guiding subsequent design and evolution. Enable the subsequent design of subsystems or modules to continue evolving within a predetermined framework and technical direction.
- Determine the non-functional requirements. Non-functional requirements refer to security, availability, extensibility, etc.

 9. **[Recommended]** When performing requirements analysis and system design, while considering the main functions, fully evaluate exception flows and business boundaries.

 10. **[Recommended]** Classes should conform to the Single Responsibility Principle in their design and implementation.
    Note: The Single Responsibility Principle is the easiest to understand yet the hardest to implement. As the system evolves, the original intent of the class design is often forgotten.

 11. **[Recommended]** Use inheritance for extension with caution; prefer aggregation/composition to implement it.
    Note: If inheritance must be used, it must conform to the Liskov Substitution Principle. This principle states that wherever a parent class can appear, a subclass must also be able to appear. For example, with "hand over the money", subclasses of money such as US dollars, euros, and RMB can all appear.

 12. **[Recommended]** In the system design phase, according to the Dependency Inversion Principle, depend on abstract classes and interfaces as much as possible, which is beneficial for extension and maintenance.
    Note: Lower-level modules depend on the abstractions of higher-level modules, facilitating decoupling among systems.

13. **[Recommended]** In the system design phase, pay attention to being open for extension and closed for modification.
Note: In the extreme case, the delivered code is unmodifiable, and requirement changes within the same business domain are implemented through extension of modules or classes.

14. **[Recommended]** In the system design phase, extract common business or common behaviors into common modules, common configurations, common classes, common methods, etc., so that no duplicate code appears in the system—that is, the DRY principle (Don't Repeat Yourself).
Note: As the number of code duplications keeps increasing, maintenance costs rise exponentially. Casually copying and pasting code will inevitably lead to code duplication, and when maintaining the code, all copies need to be modified, which is prone to omissions. When necessary, extract common methods, or abstract common classes, or even componentize.
Positive example: A class has multiple public methods, all of which need to perform several lines of the same parameter validation operation. In this case, please extract it:
      private boolean checkParam(DTO dto) {...}

15. **[Recommended]** Avoid the following misconception: agile development = telling stories + coding + releasing.
Note: Agile development is about rapidly delivering iterative, usable systems, omitting redundant design schemes and abandoning traditional approval processes, but the necessary design and documentation capture at the core key points are still needed.
Counter-example: A certain team, in order to develop business quickly, turned agile into an excuse for product managers to rush schedules. The system was full of code that barely ran but was as tangled as noodles, with extremely poor maintainability and extensibility. A year later, they had no choice but to perform large-scale refactoring, which was not worth the cost.

16. **[Reference]** The role of design documents is to clarify requirements, straighten out logic, and support later maintenance; their secondary purpose is to guide coding.
Note: Avoid designing for the sake of designing. System design documents help with later system maintenance and refactoring, so the design results need to be classified, archived, and preserved.

17. **[Reference]** The essence of extensibility is to find the points of change in the system and to isolate the points of change.
Note: The numerous design patterns in the world are in fact one design pattern—the pattern of isolating points of change.
Positive example: The hallmark of ultimate extensibility is that adding new requirements does not require any form of modification to the existing code deliverables.

18. **[Reference]** The essence of design is to identify and express the difficulties of the system.
Note: Identifying and expressing are completely different things. Many people mistakenly believe that once the system difficulties are identified, expressing them is a natural and effortless matter. But in design reviews, people often produce vague or even inarticulate explanations. Accurately expressing system difficulties requires the following abilities: proficiency in the rules and tools of expression; the limits of abstract thinking and summarization ability; the completeness of one's foundational knowledge system; and vivid expressiveness that makes the profound simple.

19. **[Reference]** The view that "code is documentation" is wrong. Clear code is only a certain fragment of documentation, not all of it.
Note: Issues such as the deep call chains of code, the network of dependency relationships at the module level, business scenario logic, and non-functional requirements need to be fully presented with corresponding documentation.

20. **[Reference]** When designing accessible products, the following need to be considered:
- All interactive control elements must be focusable by the tab key, and the focus order must conform to natural operational logic.
- Verification codes used for login validation and request interception must all provide methods other than graphical verification.
- Custom control types must clearly specify the interaction method.
Positive example: In a login scenario, the input field buttons all need to consider tab key focus. The operation order that conforms to natural logic is as follows: "enter username, enter password, enter verification code, click login", where the verification code implements a voice verification method. For controls implemented with custom tags, the role attribute can be used to set the control type.
