# Chapter 8: Data Structures (`interfaces/`, `types/`)

Welcome to the final chapter of our tutorial! In the [previous chapter](07_sar_filing_information___sarfilinginformation____updatefilinginformation___.md), we looked closely at the `SarFilingInformation` structure, which holds all the specific details needed for an official SAR filing. This structure is a perfect example of what we'll discuss now: the importance of defining standard shapes for our data.

## What's the Problem? Keeping Information Consistent

Imagine you're building something with LEGO bricks. It works because every brick has standard bumps and holes, ensuring they fit together perfectly. Now, what if every team in your organization used slightly different formats for recording an address?
*   Team A uses: `street, city, zip, country`
*   Team B uses: `addressLine1, addressLine2, town, postalCode, nation`
*   Team C uses: `line1, line2, city_or_town, postcode`

When Team A needs to share address data with Team B, confusion and errors are guaranteed! How do you reliably pass information around if everyone has their own way of structuring it?

**Data Structures solve the problem of ensuring that data objects used throughout the service have a consistent, predictable shape.** They act like standardized forms or templates (or LEGO bricks!) for our data.

## What ARE Interfaces and Types? Data Blueprints

In the `sar-service-commands` project, we use TypeScript, a language that allows us to define the "shape" or "blueprint" for our data objects. We primarily do this using:

1.  **Interfaces:** These define the names and types of properties an object *must* have. Think of it as a contract: "If you want to be considered an `Actor` object, you must have an `id` property that's a string, a `name` property that's a string, etc."
2.  **Types:** These can define custom names for specific data types, often built from basic types or other interfaces. For example, we might define `type NarrativeType = "DETECTION" | "DISPOSITION" | "FILING";` to say that a `NarrativeType` can only be one of those three specific strings.

These definitions are mostly located in two main directories:
*   `src/interfaces/`: Contains the main interface definitions for core concepts like commands (`ICommands.ts`), events (`IEvent.ts`), actors (`IActor.ts`), process instances (`IProcessInstance.ts`), and complex structures like `SarFilingInformation.ts`.
*   `src/types/`: Contains simpler type definitions or utility types, like `FocalActor.ts`.

**Analogy:** Think of these definitions like official, standardized forms used across a company.
*   `IActor` is like the standard "Employee Information Form".
*   `IAddress` is like the standard "Address Block" on that form.
*   `SarFilingInformation` is like the complex, multi-page "Regulatory Report Form".

Using these standard forms ensures everyone records and understands information the same way.

## How Are These Structures Used?

These defined shapes are used *everywhere* in the code to ensure consistency:

1.  **Command Arguments:** When you call a command like `updateFilingInformation`, the arguments you pass must match the defined interface (`IUpdateFilingInformationArguments`). This interface, in turn, specifies that the `filingInformation` property must match the shape defined by `SarFilingInformation`.

    ```typescript
    // Simplified from: src/interfaces/ICommands.ts
    export interface IUpdateFilingInformationArguments extends ICommonArguments {
      // This tells TypeScript: the 'filingInformation' property MUST
      // look exactly like the SarFilingInformation interface defines.
      filingInformation: SarFilingInformation;
    }
    ```
    If you tried to pass an object with missing fields or fields of the wrong type, TypeScript would give you an error *during development*, preventing bugs later.

2.  **Event Payloads:** When an event like `FilingInformationUpdatedEvent` is created, its `data` payload must match the structure defined in its interface.

    ```typescript
    // Simplified from: src/events/FilingInformationUpdatedEvent.ts
    export interface FilingInformationUpdatedEvent extends IEvent {
      name: EventName.FILING_INFORMATION_UPDATED;
      // The data payload MUST conform to the SarFilingInformation blueprint
      data: SarFilingInformation;
    }
    ```
    This ensures that anyone processing this event knows exactly what fields to expect in the `data` object.

3.  **Case State:** The [Case State Aggregate (`CaseState`)](01_case_state_aggregate___casestate___.md) uses these interfaces to define its own properties.

    ```typescript
    // Simplified from: src/commands/CaseState.ts
    export class CaseState {
      // ... other properties ...
      // Guarantees this property will hold an object matching
      // the SarFilingInformation shape, or be undefined.
      public filingInformation?: SarFilingInformation;

      // Uses the IProcessInstance interface for items in this array
      public processInstances: ProcessInstances = new ProcessInstances();

      // Uses the NarrativeType custom type
      public narratives: Map<NarrativeType, string> = new Map();
    }
    ```
    This ensures the `CaseState` object itself is structured predictably.

## Examples of Data Structures

Let's look at a few simplified examples found in `src/interfaces/` and `src/types/`:

*   **`IActor` (from `src/interfaces/IActor.ts`)**: Defines the basic shape for an actor involved in a case.
    ```typescript
    export interface IActor {
      id: string; // Unique system ID
      internalId: string; // Original ID from source system
      ingestDate: string; // When it was first seen
      displayName: string; // How the actor is shown in the UI
      primary: boolean | undefined; // Is this a main actor?
    }
    ```
    Anywhere the code expects an "Actor" object, it relies on this structure.

*   **`IAddress` (from `src/interfaces/IAddress.ts`)**: Defines the standard way to represent an address.
    ```typescript
    export interface IAddress {
      line1: string;
      line2: string;
      city: string;
      state: string;
      postcode: string;
      country: string;
      type: IAddressType; // Uses an enum (IAddressType) for address type
    }
    ```
    This ensures all addresses have the same fields, regardless of where they are used (e.g., inside an `Actor` or `SarFilingInformation`).

*   **`NarrativeType` (from `src/interfaces/NarrativeType.ts`)**: A custom type defining allowed narrative categories.
    ```typescript
    // This isn't an interface, but a type alias
    export type NarrativeType = "DETECTION" | "DISPOSITION" | "FILING";
    ```
    This ensures that whenever we refer to a narrative's type, we only use one of these specific, allowed values.

## Under the Hood: TypeScript's Role

There isn't complex runtime logic for these data structures themselves – they are primarily **definitions** used by TypeScript during development and build time.

1.  **Defining the Blueprints:** Developers write `interface` and `type` definitions in `.ts` files within `src/interfaces/` and `src/types/`.
2.  **Using the Blueprints:** Other code files (like command handlers, event factories, `CaseState`) import these definitions and use them to declare the types of variables, function parameters, and object properties.
    ```typescript
    // Example usage in another file
    import { IActor } from "../interfaces/IActor";

    function processActor(actor: IActor): void {
      // TypeScript knows 'actor' MUST have 'id', 'displayName', etc.
      console.log(`Processing actor: ${actor.displayName}`);
    }
    ```
3.  **Type Checking:** When the code is compiled (or checked in the editor), TypeScript verifies that the usage matches the definitions. If you try to pass an object that *doesn't* match the `IActor` interface to `processActor`, TypeScript will report an error immediately.

**Visualizing the Reliance:**

```mermaid
graph TD
    subgraph Definitions [src/interfaces/, src/types/]
        direction LR
        IActor(IActor)
        IEvent(IEvent)
        ICommands(ICommands)
        SarInfo(SarFilingInformation)
        NarrativeType(NarrativeType)
    end

    subgraph System Components
        direction LR
        Commands([Commands Service<br>(Chapter 2)])
        Events([Events<br>(Chapter 4)])
        CaseState([CaseState<br>(Chapter 1)])
        Validations([Validations<br>(Chapter 3)])
    end

    Commands -- Uses --> ICommands
    Commands -- Uses --> SarInfo
    Commands -- Creates --> Events
    Events -- Uses --> IEvent
    Events -- Uses --> SarInfo
    CaseState -- Holds --> SarInfo
    CaseState -- Holds --> NarrativeType
    Validations -- Checks --> CaseState

    %% Dashed lines show conceptual reliance on structures defined within interfaces
    Commands -.-> IActor
    Events -.-> IActor
    CaseState -.-> IActor
```
This diagram shows that many core components of the system rely on the same central definitions from `interfaces/` and `types/` to understand the shape of the data they work with.

## Conclusion: Consistency is Key!

The data structures defined in `src/interfaces/` and `src/types/` are the bedrock of consistency and predictability in the `sar-service-commands` project. By defining standard "blueprints" for objects like Actors, Accounts, Observations, Filing Information, and more, we ensure that:

*   Data passed between different parts of the system (commands, events, state) has a known and reliable shape.
*   Errors due to mismatched data formats can be caught early during development thanks to TypeScript's type checking.
*   Code becomes easier to read and understand because the structure of data objects is clearly defined.

---

**Tutorial Summary**

Congratulations on completing the `sar-service-commands` tutorial! Over the past eight chapters, we've explored the core concepts that make this event-sourced system work:

1.  [**Case State Aggregate (`CaseState`)**](01_case_state_aggregate___casestate___.md): The live, in-memory dossier for a case, built by replaying history.
2.  [**Commands Interface (`ISarCommands` & `Commands`)**](02_commands_interface__isarcommands___commands__.md): The control panel for initiating actions and changes.
3.  [**Command Validations (`validations.ts`)**](03_command_validations___validations_ts___.md): The guards ensuring actions are allowed and safe *before* they happen.
4.  [**Events & Event Sourcing**](04_events___event_sourcing_.md): The immutable logbook where every significant action is recorded as a historical fact.
5.  [**Workflow Actions (`performAction`, `ActionPerformedEvent`)**](05_workflow_actions___performaction____actionperformedevent___.md): How cases are moved through predefined investigation steps.
6.  [**Process Instances & Workflows (`IProcessInstance`, `ProcessInstances`, `IProcessDAO`)**](06_process_instances___workflows___iprocessinstance____processinstances____iprocessdao___.md): The blueprints (Processes) and trackers (Process Instances) defining and managing the case lifecycle.
7.  [**SAR Filing Information (`SarFilingInformation`, `updateFilingInformation`)**](07_sar_filing_information___sarfilinginformation____updatefilinginformation___.md): The dedicated structure for holding complex regulatory filing details.
8.  [**Data Structures (`interfaces/`, `types/`)**](08_data_structures___interfaces_____types____.md): The foundational blueprints ensuring data consistency throughout the service.

By understanding how these pieces fit together – how commands trigger events, how events build state, how state is used for validation and workflow, and how data structures ensure consistency – you now have a solid foundation for working with and extending the `sar-service-commands` project. Good luck!

---

Generated by [AI Codebase Knowledge Builder](https://github.com/The-Pocket/Tutorial-Codebase-Knowledge)