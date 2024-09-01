- attendance quiz

### Instructor Information

- Prof. Nhut Nguyen
- Office ECSS 3.607
- must reserve exam seats

## <span style="color:#9dcfff">Introduction</span>

- what will be covered
  - Security concepts
    - threats, attacks, mechanism, services, architecture
  - Computer systems and network attacks,
  - Tool and techniques against security attacks
- not be covered
  - Advanced cryptography, cryptocurrencies, blockchain, how to hack, how to ace cybersecurity certification exams
- Exams
  - Three exams with the last being comprehensive
  - must reserve time in the testing center
  - Exam 1
    - September 24th
    - 10% final grade
  - Exam 2
    - November 5th
    - 20% final grade
  - Exam 3
    - December 5th
    - 30% final grade
- Assignments are 25% final grade. 5 assignments throughout semester
- Attendance is 15% final grade

## <span style="color:#9dcfff">Overview</span>

- What is security?
  - Protect some Asset
- Key Definitions
  - Systems engineering
    - large complex engineering problems involving various hardware and software components.
  - Security Engineering
    - Popularized by Ross Anderson as a branch of systems engineering related to security of real systems
  - Information security
    - refers to the trustworthiness of data. Protection of both data that is stored and data that is in transit between nodes.
    - Often used interchangeably with computer security but may include networks as well.
  - Network security refers solely to transmission security between end points
- Key Attributes of a security asset
  - CIA
  - Confidentiality
    - Privacy and secrecy of the data
  - Integrity
    - both data integrity, and origin integrity. That the data doesn't change, and where the data comes from does not change
  - Availability
    - Whomever is supposed to be able to access the data can, and who isn't supposed to be able to see the information can't
- What is the OSI Security Architecture?
  - Follows the [X.800](X.800-Spec.pdf) specification
  - Which defines concepts and requirements for securing data networks
  - provides a useful overview of security issues
  - provides definitions and instructions regarding important security concepts
    - Security attack
      - any action that exploits one or more vulnerabilities
      - Eavesdropping, DOS / DDOS, Masquerading / spoofing, Man-in-middle
    - Security Mechanism
      - Feature designed to detect, prevent, or recover from an attack
      - no single mechanism will support all services required, however they nearly all use some sort of cryptographic algorithm under the hood
      - Ex: Encryption, digital signatures, access controls, traffic padding, routing control etc.
    - Security Services
      - X.800 provides information on Confidentiality, Integrity, Authentication, Access Control, and Non-Repudiation
        - Non-repudiation is the protection against denial by one of the parties in a communication

# IT Revolution

- There are two pillars holding up the IT Revolution, Computing and Networking
- Computer Security
  - The cross-road of computing and security
- Network Security
  - the cross-road of networking and security
- Information Security
  - the cross-road of computer security and network security

## Threats and Prevention

- Types of Threats
  - Natural Causes
    - Fire, power failure, earthquake etc
  - Human Causes
    - Benign
      - Accidental deletion of data
    - Malicious
      - Random
        - code on a general website
      - Directed
        - targeted DOS attack or malware etc
- Policy and Control
  - Policy dictates what is and what is not allowed
    - Ex: Password complexity requirements, with the hopes that simple passwords aren't possible to be created and accepted
  - Controls enforce policies
    - Control Types
      - Physical / Technical / Procedural
    - if policies conflict it can create confusion and security vulnerabilities
    - Goals of Security Controls
      - Prevention / Detection / Recovery
  - Security Control Life Cycle
    - Similar to waterfall software model
      - Threats create policy which creates specification which defines a design, which is used in an implementation that is put into operation
    - Assurance
      - a measure of how well the system meets its requirements, and how well the system does what it is supposed to do
- Operational Issues
  - <span style="color:#aaaaff; font-weight:bold">Cost benefit analysis:</span> There is some thought that needs to go on because it may be cheaper and easier to just allow the violation, and fix the problem after the fact rather than prevent or visa versa
  - <span style="color:#aaaaff; font-weight:bold">Risk analysis:</span> If something is to break how dangerous is the fault?
  - <span style="color:#aaaaff; font-weight:bold">Laws and customs:</span> are desired security measures illegal?
- Fundamental Security Design Principles
  - There are many here are just a few
  - Separation of privilege
  - Encapsulation
  - Isolation
  - Modularity
  - Layering
  - Security by Obscurity
    - Assumption that security is effective if the mechanisms are confusion or supposedly not generally known
    - Not as applicable now as gaining knowledge is easier and many vendors want to participate so everyone having their own standards creates roadblocks
