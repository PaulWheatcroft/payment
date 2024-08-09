# Payment Solution

## Introduction

## UX

## Architecture

### Data Flow

#### Handling Everything in the Browser

```plantuml
@startuml
!include <C4/C4_Context>
skinparam linetype ortho
title IRAP System Context
Boundary(irap, "IRAP", "Migration Scope") {
    System(irap_app, "IRAP", "")
}

Person(irap_user, "IRAP User", "")
Rel(irap_user, irap_app, "\n", "HTTP")
Lay_Distance(irap_user, irap_app, 1)

System_Ext(clamav, "ClamAV", "Virus scanner")
Lay_L(irap_app, clamav)
Rel(irap_app, clamav, "\n", "HTTP")
Lay_Distance(clamav, irap_app, 2)

System_Ext(sso, "Staff SSO", "Authentication")
Rel(irap_app, sso, "\n", "HTTP")
Rel(irap_user, sso, "\n", "HTTP")
Lay_Distance(sso, irap_app, 2)

System_Ext(govuk_notify, "GOV.UK Notify", "Sends email notifications")
Rel(irap_app, govuk_notify, "\n", "HTTP")

System_Ext(sentry, "Sentry", "Tracing and Performance")
Rel(irap_app, sentry, "\n", "HTTP")
Lay_Distance(sentry, irap_app, 2)
Lay_R(irap_app, sentry)
@enduml

```

#### Handling Stripe via Backend Service

```plantuml
!include <C4/C4_Container>
@startuml
Bob -> Alice : goodbye2
@enduml
```

## State Management

## Payment Provider Integration

## Theming

## Security

## Testing and Quality Assurance

## Conclusion and Questions