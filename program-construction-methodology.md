---
# Program Construction Methodology
---
## Overview

Program Construction Methodology is a systematic approach to building software programs. It consists of two main components:

1. **Overall System Design**
2. **Software Development**

> **Important Note:** Program construction is the entire journey, while programming is just a part of it.

## Prerequisites: Essential Steps Before Development

Before beginning any software project, the following steps must be completed:

1. **System Requirements Analysis**
2. **System Requirements Specification**
3. **System Architecture Design**
4. **System Validation**

## Types of Requirements

Requirements are broadly classified into two main categories:

### Functional Requirements

Functional requirements describe **what a system must do**. They specify the actual functions and behaviors the system needs to have to meet user needs.

#### Two Approaches to Define Functional Requirements

**1. Based on Functionality (Specific Functions)**

Examples for an Online Banking System:
- The system must allow users to transfer money between accounts
- The system must generate monthly account statements
- The system must calculate interest on savings accounts
- The system must allow password changes

**2. Based on Behavior (How the System Responds)**

Input/Output Behavior Examples:
- **Input:** User enters correct PIN → **Output:** ATM displays main menu
- **Input:** Invalid login attempt 3 times → **Output:** System locks account for 30 minutes
- **Input:** Account balance below $100 → **Output:** System sends low balance alert
- **Input:** User clicks "Forgot Password" → **Output:** System sends reset email

### Non-Functional Requirements

Non-functional requirements specify **how a system should perform**. They are commonly referred to as "-ilities".

#### Examples of Non-Functional Requirements:

- **Reliability** - How dependable is the system?
- **Scalability** - Can it handle growth?
- **Usability** - How easy is it to use?
- **Maintainability** - How easy is it to modify?
- **Security** - How well protected is it?
- **Portability** - Can it work on different platforms?
- **Availability** - How often is it operational?

#### Characteristics of Non-Functional Requirements

Non-functional requirements are often:

1. **Subjective** - "The system should be user-friendly"
2. **Ambiguous** - "The system should be fast"
3. **Conflicting** - May contradict each other

## Documentation Requirements

Requirements must be documented to be easily understood by:
- **Non-technical readers** (customers)
- **Technical readers** (software developers)

## Comparison: Functional vs Non-Functional Requirements

| Aspect | Functional Requirements | Non-Functional Requirements |
|--------|------------------------|----------------------------|
| **Objective** | Describe what product does | Describe how product works |
| **End Result** | Define product features | Define product properties |
| **Focus** | On user requirements | On user expectations |
| **Documentation** | Captured in Use Cases | Captured as quality attributes |
| **Origin Type** | Defined by user (mostly) | Defined by developers/experts |
| **Testing** | • Component Testing<br>• Module Testing<br>• API Testing<br>• UI Testing | • Performance Testing<br>• Usability Testing<br>• Security Testing<br>• Load Testing |

---

*This guide provides a structured approach to understanding and implementing program construction methodology for successful software development projects.*
