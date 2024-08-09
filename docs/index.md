# Payment Solution

## Introduction
An outline and overview of the options and design patterns that could be used in a payment app built using JavaScript and React

## Architecture

### Data Flow

#### Handling Everything in the Browser

```plantuml
!include <C4/C4_Container>
@startuml
Bob -> Alice : hello2
@enduml
```

#### Handling Stripe via Backend Service

```plantuml
!include <C4/C4_Container>
@startuml
Bob -> Alice : goodbye2
@enduml
```
