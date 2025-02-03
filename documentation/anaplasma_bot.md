# Ananplasma Bot Documentation

## Table of Contents
1. [Non-Technical Overview](#non-technical-overview)
2. [Technical Documentation](#technical-documentation)
3. [Developer Reference](#developer-reference)
4. [System Diagrams](#system-diagrams)

# Non-Technical Overview

## Purpose
Ananplasma Bot reviews anaplasma cases and reports using emails to NBS personnel. It processes cases, validates data, and flags issues for manual review.

## Key Features
- Automated case processing
- Data validation
- Email notifications
- Manual review flagging
- Patient data verification
- Lab data verification
- Investigator verification

## Business Benefits
- Reduced manual processing time
- Consistent data validation
- Automated quality checks
- Streamlined case management
- Improved data accuracy

# Technical Documentation

## System Overview
Ananplasma Bot extends Ananplasma which inherits from Base Bot allowing access to all its functionality. It includes:
- Automated login and navigation
- Comprehensive data validation
- Error handling and recovery
- Reporting functionality
- Integration with external services (USPS, SMTP)

## Core Workflows

### 1. Case Processing
- Queue management
- Case sorting
- Data validation
- Issue flagging

### 2. Reporting
- Email notifications
- Manual review logging
- Status updates

# Developer Reference

## Code Structure

### Base Class
```python
class Anaplasmacasereview_revised(NBSdriver):
    """ A class inherits all basic NBS functionality from NBSdriver and adds
    methods for reviewing COVID case investigations for data accuracy and completeness. """
```

### Key Components
1. Configuration Management
   - Environment selection
   - API credentials

2. Navigation Methods
   - Queue management
   - Case navigation
   - Form handling

3. Data Validation
   - Demographics
   - Investigator checks
   - Lab data
   - Patient status
   - Dates
   - Addresses.

## Validation Checks

### Demographic Validation
1. Personal Information
   - Name verification
   - DOB validation
   - Age verification
   - Race validation
   - Address verification

2. Location Data
   - ZIP code validation
   - County verification
   - State/country checks

### Investigator Checks
1. Investigator 
   - Inestigation duration
   - Investigator assignment
   - Jurisdiction validation

### Lab Data Management
1. Lab Reading
   - Associated labs
   - Assign lab types
   - Case status check
   - Collection Date
   - Report Date 

### Medical Information
1. Patient status
   - Hospitalization
   - Illness duration
   - Physician checks
   - Case status

### Required Fields
- Personal identifiers
- Location data
- Medical information
- Reporting details
- MMWR data

## Error Handling

### Retry Logic
- Configurable attempts
- Timeout management
- Session recovery

### Issue Tracking
- Validation issues log
- Manual review queue
- Lab data issues

## Integration Points

### External Services
- USPS API
- SMTP email
- Chrome WebDriver
- RSA authentication

### Data Exchange
- Lab data processing
- Address verification
- Email notifications

# System Diagrams

## Class Structure
```mermaid
classDiagram
    class Anaplasmacasereview_revised {
        +production: bool
        +num_approved: int
        +num_rejected: int
        +num_fail: int
        
        +__init__(production)
        +CheckAge()
        +CheckAgeType()
        +CheckRaceAna()
        +CheckPhone()
        +read_city()
        +CheckCityCountyMatch()
        +CheckCurrentSex()
        +GoToTickBorne()
        +CheckJurisdiction()
        +CheckInvestigationStartDate()
        +CheckInvestigatorAna()
        +CheckInvestigatorAssignDateAna()
        +CheckDeath()
        +CheckHospitalization()
        +CheckIllnessDurationUnits()
        +CheckTickBite()
        +CheckOutbreak()
        +CheckImmunosupressed()
        +CheckLifeThreatening()
        +CheckPhysicianVisit()
        +CheckSerology()
        +CheckClinicallyCompatible()
        +CheckIllnessLength()
        +CheckSymptoms()
        +CheckCase()
        +RejectNotification()
        +ApproveNotification()
    }
    
    class AnaplasmaBot {

        +patients_to_skip: list
        +tb: string
        +error: bool
        +n: int
        +attempt_counter: int

        +set_credentials(username, passcode)
        +log_in()
        +GoToApprovalQueue()
        +SortApprovalQueue()
        +SendManualReviewEmail()
        +Sleep()
        +CheckFirstCase()
        +GoToFirstCaseInApprovalQueue()
        +StandardChecks()
        +ApproveNotification()
        +ReturnApprovalQueue()
        +RejectNotification()
    }
    AnaplasmaBot --|> Anaplasma
    
    note for AnaplasmaBot "Extends Anaplasma which extends Base Bot\nHandles NBS initialization, authentication and miscellaneous methods"
```

## Process Flow
```mermaid
flowchart TD
    A[Initialize Athena] --> B[Set Credentials]
    B --> C[Login to NBS]
    C --> D[Navigate to Approval Queue]
    D --> E[Sort Queue]
    E --> F{Queue Load?}
    F -->|Yes| G[Check First Case]
    F -->|No| H[Send Review Email]
    G --> I{Patient condition is Covid-19}
    H --> J[Sleep Bot]
    J --> E
    I -->|Yes| K[Navigate to first case in queue]
    I -->|No| L{Check Number of Attempts}
    K --> N[Standard Checks]
    L -->|Less than| M[Increase counter]
    L -->|Greater or Equal to| H
    M --> E
    N --> O{Check for Issues}
    O -->|No issues| P[Approve Notification]
    O --> Q[Return to queue]
    P --> Q
    Q --> R[Sort Queue]
    R --> S[Check First case]
    S --> T{Final name same as Initial Name?}
    T -->|Yes| U[Reject Notification]
    T -->|No| V[Increase fail counter]
    U --> E
    V --> E
```

# Maintenance Guidelines

## Configuration Updates
- Regular review of timeouts
- Email list maintenance
- API credential updates
- Retry attempt optimization

## Monitoring
- Manual review frequency
- Timeout occurrences
- Email notification success
- SSL certificate status

## Performance Optimization
- Wait time review
- Queue processing efficiency
- Batch size adjustments
- Resource utilization

# Support and Troubleshooting

## Common Issues
1. Login Failures
   - Check RSA token
   - Verify SSL certificates
   - Confirm credentials

2. Timeout Issues
   - Review wait settings
   - Check network connectivity
   - Verify NBS availability

3. Validation Errors
   - Check data completeness
   - Verify field formats
   - Review business rules