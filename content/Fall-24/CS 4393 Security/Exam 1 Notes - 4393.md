# Software Security - Buffer Overflow

- What is a buffer overflow vulnerability
  - Memory locations beyond the boundary of a buffer allocation, are accessed and written too
- If they have enough privilege in a system they can use the SETUID keyword to change process values in a system.
- Buffer overflow method was the first form of malicious attack used in 1988 by Morris Internet Worm using the "fingerd"
- Goal of Buffer Overflow
  - Take control over a program by changing its control flow to run malicious code
  - inserting some malicious code that takes information or steals resources, and reroutes the pointer of the program to run the injected code.
  - Could also take control of host by using the root shell with the user's privileges
- The easiest way to change the pointer of a program, when injecting a virus or whatever is to change the return address of a function to where you have inserted your code

## Evolution of Buffer Overflow Methods

### Stack Smashing

- The CPU of course keeps its stack for the current line that is being ran in a program. It uses this as we learned in other classes to keep track of function calls and local variables.
- Stack smashing takes advantage of this by writing enough to change the value of the return address. The CPU will be none the wiser and start reading in the new, illegitimate location
- The most challenging part is the find the address of your malicious code, so that you can place the correct return address onto the stack
  - In practice you can never really find exactly where your code starts, only the vicinity

### NOP Sled

- Every processor has a NOP Instruction, which makes the processor idle for that instruction
- The idea being to attach many NOP instructions before your malicious code, that way if your guess on the address is anywhere within the range of the first NOP instruction until the actual first line, your malicious code will still execute properly.

## Defenses

- My ideas
  - limit input length so overflows will be cut off
  - input validation to prevent certain inputs ( like SQL injection ). input cleansing.
  - check sum or data validation so you know when the data has been edited
- How to defend? What approaches?
  - Compile time defenses:
    - Double check any unsafe function, like library calls, string copy or string inputs should be checked to be benign.
    - Java is a safe language because it hides memory pointers, so to know where to insert your attack is much more difficult
    - compilers warn about unsafe code
      - can also perform manual inspection which takes much longer but perhaps better
    - compilers replace functions with their safer counterparts when possible.
      - adding range checking like (strncpy)
      - Originally they would just set a max length of a string, but attackers can just add max length twice and reach into other portions of the code still
  - Run Time Defense:
    - detect an exploit while some process is running, and abort it
    - The OS can just prevent any kind of execution from the buffer, as we never need to do that when we have the proper authority
    - Non-Executable stack
      - The OS can explicitly not allow processes to run commands or instructions that are currently being ran from the stack, or from a stack pointer. which is where attackers are trying to exploit
    - Address randomization
      - because the attacker needs to know the logical address as you don't know where the stack is, you can let the OS randomize the addresses a bit so that the attacker must know more information about the system first
      - There was an attack on 75,000 sql servers with one program because they all had the same underlying code.
    - NX Bit
      - In order to prevent execution from the stack or unauthorized areas in the memory we can mark the page as NX (Non-Executible). The OS knows if coming from a certain area any exec calls should be blocked
      - can still exploit some library calls, and some languages like lisp require execution from stack to still be possible
    - Array reads and write can all be bound checked, Java and .NET languages all implement this
    - can also double check any calls for unsafe methods/functions like strcpy
    - Can hide all pointers but it is expensive to check every single memory call
    - Insert a canary between the buffer and rest of the memory. If any kind of buffer overflow is used, that canary will break, which can be used to alert the OS of an attack.
      - canary can be exploited by inserting your own canary, and then hijacking the exception handler to again reach into other pages you shouldn't be permitted into.

# Malware

## Overview

- Types
  - Worm
  - Virus
  - Adware
  - Logic bomb
  - Trojan
- A program that is inserted into a system, usually covertly, with the intent of compromising the confidentiality, integrity, or availability of the victim's data.
- Classified into two categories
  - Based on how it spreads or propagates
  - Also on the actions of payloads is performs
- Defined also by
  - If it needs a host program, such as viruses
  - Those that are independent, such as trojans and worms
  - Malware that does not replicate like spam emails
  - Worms and viruses do replicate
- Propagation techniques
  - Infection of existing content by viruses that is then spread to other systems.
  - Exploit of software vulnerabilities by worms or drive-by-downloads t o allow malware to replicate
  - Social engineering attacks such as phishing to install trojan horses
- Payload Actions
  - Encrypt of important files to hold as ransom ( Ex. WannaCry )
  - Corruption of system or data files
  - Theft of information from a system
- Attack Kits
  - Development of malware is hard so whole kits are sold
  - Toolkits are often known as crimeware
    - Easy for anyone to use, and also flexible allowing the attacker to modify the kit and make it much harder for security measures to catch
  - Examples
    - Zeus
    - Blackhole
    - Sakura
    - Phoenix
- Advanced Persistent Threat (APT)
  - well resources persistent application of a wide variety of intrusion technologies

## Worms

- What are worms
  - Worms are like viruses, but they are self-contained, and don't need a host program
  - exploit vulnerabilities in client or server programs
  - use network connections to spread from system to system or shared devices ( USB, CD)
  - upon activation the worm may replicate and propagate again
- Worm Replication
  - Email
  - file sharing
  - remote execution capability
  - remote login
- Target Discovery
  - Topological
    - this method uses information contained on an infected victim machine to find more hosts to scan
  - Local subnet
    - if a host can be infected behind a firewall that host then looks for targets in its own local network

## Antivirus Approaches

- Prevention, detection and identification, and removal
- Four generations of antivirus
  - Simple scanner
    - uses "signatures" of known viruses ( structure based)
  - Heuristic Scanners
  - Activity Traps
  - Full-featured protection
    - combination of many antivirus techniques

## Components and Phases of viruses

- Viruses
  - Piece of software that infects programs
  - modifies them to replicate the cirus
  - When attaches to an executable program, a virus can do anything the program has perms to do

### Components

- Infection Mechanism
  - Means by which a virus spreads or propagates ( Infection vector )
- Trigger
  - event or condition that determines when the payload is activated or delivered
  - sometimes known as logic bomb
- Payload
  - what the virus does ( besides spreading)

### Phases

1. Dormant Phase
   1. virus is idle and
2. Triggering Phase
3. Propagation Phase
4. Execution phase

### Virus Classifications

- By Target
  - Boot sector infector
    - Infects a master boot record or boot record and spread when a system is booted
  - File infector
  - Macro Virus
    - Very common in the mid-1990s
    - Platform independent, infect documents not executable code
    - easily spread.
    - Exploits capability of MS Office applications
  - Multipartite virus
- by concealment
  - encrypted virus
  - stealth virus
  - polymorphic virus

## Botnet

- Botnet is a network of autonomous programs that can be activated remotely.
- Used to platform various attacks
  - spam and click fraud
  - DDOS
- History of Botnet
  - first arose in 2006
  - Eggdrop in 1993 was the first IRC bot
  - DDoS bots and Trojans arose in the late 90s
  - by 2006 millions of distinct bots used for fraud and extortion
- Life cycle of an IRC Bot
  - Exploit some vulnerability to execute a short program.
  - shellcode downloads and installs the bot on the victim machine
  - bot disables firewall or antivirus
  - bot locates IRC server and connects to a channel
  - The botmaster can then send command to the bots through the IRC server
  - The bots can spread after they infect a victim
- How to detect
  - IRC and DNS activity used to control the bots is very visible, so we can look for hosts who are asking many queries but few are asked about it. This means it is looking for machines
  - Botnets are evaded by using encryption or P2P networks
