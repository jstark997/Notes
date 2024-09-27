# Current Threat Landscape

## Security Bugs and Vulnerabilities

Complexity in software gives rises to bugs some of which can be exploited to compromise security.

At particular risk are bugs in browsers or their extensions and add ons.

Two types of bugs:

1. Known - a patch is available
2. Unknown - no patch (zero day)

## Malware

The following list is not a mutually exclusive taxonomy. It is just to get familar with some of the terms for the different types of attacks.

- Macro virus - written in a macro language, runs automatically when an infected document is opened.
- Stealth virus - tricks anti-virus software by intercepting its request to the operating system.
- Polymorphic virus - creates varied operational copies of itself, making it difficult to detect.
- Self-garbling virus - modifies its code so as not to match predefined anti-virus signatures.
- Bots & Zombies - infected machines are under the control of a hacker in order to do what they want (sometimes as part of a network).
- Worms - viruses that spread from one machine to another.
- Rootkit - embedded in the kernel of an operating system, so can hide completely from the operating system.
- Firmware rootkit - embedded in firmware.
- Keylogger - logs keystrokes.
- Trojan - appears to be one thing but is actually malware.
- Remoate access tools (RAT) - allow intruders to access your system remotely.
- Ransomware - covertly encrypts all files on an infected machine, hacker provides decryption key in exhcange for money.
- Malvertisement - online advertisement infected with malware (often downloads multiple scripts via redirection to avoid detection).
- Drive-by attack - visiting a website that contains code that will exploit the visiting machine.
- Spyware - gathers information from the infected machine and sends it back to the hacker.
- Adware - forces unwanted advisterments on the infected system (sometimes by hijacking the default search engine).
- Browser hijacking - takes control of the browser to do want the hacker wants.
- Scarware - attempts to trick a person into believing a fake threat, and oftens asks for money to solve the bogus problem.
- PUPs (Potentially Unwanted Programs) - applications that may be legitimate but have functions that can be exploited without the user's consent.

## Phishing

Phishing attempts to trick the user to click a link or execute malware in some way.

One of the most common attacks because easy to perform, cheap to set up and often yields good results for hackers.

Typically carried out by sending fake emails or instant messages that direct the user to a fake website that looks like a legitimite website.

An attack targeted on a specific individual is called 'spear phishing'.

Techniques:

- Link manipulation

  - Incorrect subdomain
  - Misspelled subdomain
  - IDN homograph attack
  - Hidden URLs

- Covert URL Redirect (real website has been compromised by a hacker)

  - Open redirect
  - Cross-site scripting
  - Cross-site request forgery

- Variations

  - Vishing - using phone (voice) to call the target
  - Smishing - sending text messages to the target

  In the above variants the attacker tries to convince the target to give up sensitive information or download malware.

## Spamming

Unsolicited messages sent to a target to try to get them to do something that benefits the hacker. Popular because cheap to implement.

## Doxing

Researching an individual or company in order to discover private and possibly damaging information that can be released or threaten to be released publicly.

## Social Engineering

Hacker preys on the trust or frailty of the the target.

- Merchant scams
  - Puchased item not delivered
  - Purchased item not what was claimed
- Phishing
- Fake prizes, gifts, lotteries
  - Advance fee fraud
- Fake check scams
- Fake debt collection
- Fake refund / money recovery scams
- Fake technical support scams
- Scholarship, student loan, financial aid scams
- Online dating scams
- Fake social media friend scam
- eBay / auction seller scams
  - Ship goods before receiving payment

## CPU Hijacking

Malware that hijacks the target computers CPU in order to mine cryptocurrency.

Vectors:

- Phishing
- Bad app
- Browser (JavaScript)

## Darknet

A encrypted overlay network that can only be accessed with particular software, authorization, protocols or ports. This is done in order to provide security and maintain privacy. Used by both legitimite and criminal parties.

The regular internet is refered to as the Clearnet or surface web.

Examples:

- RetroShare
- Tor
- I2P Anonymous
- GNUnet Framework
- Freenet Project

## Interdiction

The process of installing devices for hacking or spying purposes. These devices can be used on specific targeted individuals or installed in other devices that serve many people, such as routers.

## Backdoors

A vulnerability in code that allows access to the application running the code to a hacker.

### Reproducible Builds

A software development pratice that creates a verifiable path from source code to the binary code executed as the application. Multiple parties should be able to build the code are all achieve the same result (the same binary code). The build system needs to be completely deterministic and the build environment needs to be either recorded or predefined. Also there needs to be a way to verify that all the builds are identical.

## Mitigating Threats

- Distribute trust
- Reduce attack surfaces
- Isolation and compartmentalization
- Build layers of defenses
