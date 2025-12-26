# LSE Dispatcher – Input Preparation and Queue Distribution

## Overview

The Dispatcher is responsible for preparing input data and distributing workload to UiPath Orchestrator Queues for further processing by the Performer.

Its main role is to validate, normalize, and safely enqueue input records so that each financial instrument can be processed independently and in a scalable manner.

## Execution Model

The Dispatcher is designed for **unattended execution** in UiPath Orchestrator but can also be launched manually using **UiPath Assistant** for testing or local execution scenarios.

It is typically executed once per batch to prepare Queue Items for the Performer.

## Responsibilities

- Load input data from a configurable source  
- Validate input structure and required columns  
- Normalize and validate business data  
- Generate a unique Batch ID for traceability  
- Create Queue Items in UiPath Orchestrator  
- Skip invalid records without interrupting the batch  

## Input Sources

The Dispatcher supports multiple input delivery mechanisms via configuration:

- **Orchestrator Storage Bucket** (default – cloud execution)  
- **Local file system** (optional – local execution)  
- **HTTP endpoint** (optional – integration-ready)  

The active input source is selected through configuration without code changes.

## Validation Strategy

Input validation is performed before queueing:

- Structural validation of CSV files  
- Required column presence validation  
- Record-level validation (e.g. missing stock code)  

Invalid records are skipped and logged to ensure that only valid transactions reach the Performer.

## Queue Strategy

Each valid input record is transformed into a single Queue Item:

- Transactions are independent and stateless  
- Queue Items contain only required business data  
- Batch ID is used for traceability and audit purposes  

This design enables parallel processing and easy horizontal scaling.

## Key Design Principles

- Unattended-first, cloud-oriented design  
- Configuration-driven behavior  
- Clear separation from processing logic  
- Fail-fast approach for critical configuration issues  
- Deterministic logging and batch traceability  

---

The Dispatcher can be easily extended to support additional input formats or delivery mechanisms without impacting the Performer logic.
