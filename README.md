# Overview

This document provides an overview of the Infix Calculator repository, a single-file C implementation that evaluates arithmetic expressions using stack-based algorithms. The system converts infix notation expressions to postfix notation before performing evaluation, demonstrating fundamental data structure concepts including stack operations and expression parsing.

The calculator supports basic arithmetic operations (+, -, *, /, ^) with single-digit operands and handles parentheses for grouping.

## System Purpose and Capabilities

The Infix Calculator implements a three-phase processing pipeline that transforms user-entered mathematical expressions into computed results. The system accepts infix expressions containing single-digit numbers, arithmetic operators, and parentheses, then produces floating-point results with four decimal places of precision.

**Key Capabilities:**
- Infix to postfix expression conversion using the Shunting Yard algorithm
- Postfix expression evaluation using stack-based arithmetic
- Input validation for bracket matching and operator sequences
- Support for operator precedence rules (^, *, /, +, -)
- Interactive command-line interface with error recovery

**Technical Constraints:**
- Single-digit operands only (0-9)
- Fixed-size arrays for stack storage
- No support for decimal input numbers
- Recursive error handling approach

## High-Level System Architecture

The system architecture centers around three primary data structures and a sequential processing pipeline implemented in a single C file.

## Code Entity Mapping

The following diagram maps the system's conceptual components to their actual implementation in the codebase:

#### Function and Data Structure Relationships

```mermaid
graph LR
    subgraph "Entry Point"
        A["main()"]
    end
    
    subgraph "Validation Functions"
        B["chck_brakects()"]
        C["chck_operators()"]
    end
    
    subgraph "Stack Management"
        D["push(char c)"]
        E["pop()"]
        F["pushev(float c)"]
        G["popev()"]
        H["top()"]
        I["isempty()"]
    end
    
    subgraph "Core Algorithms"
        J["prefered(char a)"]
        K["evaluate()"]
        L["popall()"]
    end
    
    subgraph "Global State"
        M["stack[256]<br/>count"]
        N["post[100]<br/>postcount"]
        O["eval[100]<br/>counteval"]
    end
    
    A --> B
    A --> C
    A --> J
    A --> K
    
    B --> D
    B --> E
    B --> H
    B --> I
    
    K --> F
    K --> G
    
    D --> M
    E --> M
    F --> O
    G --> O
    L --> N
    
    J --> M
    K --> N
```

## Processing Pipeline Overview

The calculator follows a strict sequential processing model where each phase must complete successfully before proceeding to the next:

| Phase | Primary Functions | Input | Output | Error Handling |
|-------|------------------|-------|--------|----------------|
| **Validation** | `chck_brakects()`, `chck_operators()` | `inf[100]` string | Boolean validation result | Recursive `main()` call |
| **Conversion** | `main()` parser loop, `prefered()`, stack ops | Validated infix string | `post[100]` postfix array | N/A (validation ensures success) |
| **Evaluation** | `evaluate()`, `pushev()`, `popev()` | `post[100]` array | `float` result | N/A (well-formed postfix guaranteed) |

The system uses global index counters (`count`, `postcount`, `counteval`) to track the current state of each data structure throughout processing.

**Error Recovery Mechanism:**
When input validation fails, the system prints an error message and recursively calls `main()` to restart the entire process, effectively implementing a simple retry loop without structured exception handling.

## Key Design Characteristics

**Single-File Architecture:** The entire calculator implementation resides in the `Calculator Stack` file, containing all data structures, algorithms, and user interface logic in approximately 210 lines of C code.

**Stack-Centric Design:** The system employs three distinct stack/array structures:
- Character stack (`stack[256]`) for operator precedence management during conversion
- Postfix storage (`post[100]`) as an intermediate representation
- Float stack (`eval[100]`) for arithmetic computation during evaluation

**Global State Management:** All data structures and counters are declared as global variables, simplifying function interfaces but requiring careful state management across the processing pipeline.

**Precedence-Driven Conversion:** The `prefered()` function encodes operator precedence rules (parentheses: -1, +/-: 1, */: 2, ^: 3) that drive the infix-to-postfix conversion algorithm.

