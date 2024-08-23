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
