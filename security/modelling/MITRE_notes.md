MITRE ATT&CK Framework Overview and Application
=====
Presented by Jon Oltsik and Travis Farral, 2019-10-09
-----
Definitions:
- Behavioural Intelligence
    - Ability to understand the behaviours of network traffic, then make
      decisions based on that behaviour

-----

Cybersecurity analytics/operations is getting more difficult - 72% agree

Why?
- Threat landscape is evolving and changing too rapidly to keep up with trends
- The volume of security alarms have increased, which should be prioritised?

Biggest challenges in security operations:
- Total cost
- Most of time spent on addressing emergency issues, not enough time on
  strategy and process improvement
    - Don't just fight fires, figure out how to fight them better
- Takes too long to detect incidents
- Organisations don't have tools and processes in place to operationalise
  threat intelligence

People have a lot of tools - 70% of respondents have between 10 to 50 tools.
Doesn't scale.

MITRE ATT&CK Framework
-----
- Goal: Detect, categorise, and document adversary behaviour
- Use categorised behaviour of known adversaries to emulate attacks

- Organised by technology domains
    - Enterprise and mobile
- Tactics, techniques
    - The "why" and "how" an adversary achieves a tactical objective
- Groups
    - Information about known adversaries
    - Tactics, techniques, remediation for those techniques, etc

What can you do with ATT&CK?
-----
- Use ATT&CK's taxonomy to categorise defenses
    - Where are defensive strengths
    - Where are gaps and weaknesses
- Provide a common language for describing malicious behaviour
    - Between tools
    - For incident response
    - For threat intelligence
    - For sharing
- Develop better detections/protection

Defensive Controls
-----
- Catalogue defensive controls
- Align controls to techniques in ATT&CK
    - What does this control detect or prevent?
- For good measure:
    - Simulate an attack!
    - Test techniques against the controls providing protection
    - Measure effectiveness
    - Also helps find configuration errors

Testing against your environment
-----
- Simulate techniques to test detections/mitigations
- There are already open source tools to do this:
    - MITRE Caldera
    - Uber Metta
    - Endgame Red Team Automation (RTA)
    - Red Canary Atomic Red
- You can also simulate with an internal red team - can also be done ad hoc
    - ATT&CK helps you communicate to a red team with a common language
- Have pen test actions mapped to ATT&CK

Why would experienced red teams/pen testers need ATT&CK?
- They get set in their ways (because it works!)
- Because ATT&CK is a living document, they'll try different things and keep
  up with trends in techinques
- They can also refer to ATT&CK when explaining what did and didn't work,
  enabling the blue teams to be able to understand quicker and find solutions
  sooner

Adversarial Simulation with ATT&CK
-----
Simulate -> Hunt -> Detect

Simulate:
- Perform adversarial simulation
- With tools/red team/pen testers

Hunt:
- Look for evidence
- Logs and tools

Detect:
- Add detection
- Note gaps
- Note tools (which are working well, providing useful information, and
  which aren't?)

MITRE Cyber Analytics Repository (CAR)
-----
A collection of different ways of having analytics to detect certain techniques
Gives an idea of what sort of things to make detection for

MITRE ATT&CK Navigator
-----
Lay out what sort of controls you have, to easily see where you may have gaps

ATT&CK with Threat Intelligence
-----
- Match tactics and techniques to SOC/IR observations
- Use tactics and techniques to describe malware behaviours
- When reviewing public or private intelligence reports:
    - Match details to tactics (and techniques if possible)
    - You can use this to understand your adversaries better, in general.
      What are you adversaries focusing on?
- Include tactics and techniques in reporting 
    - Apply knowledge of internal controls in reporting

ATT&CK Actors/Groups
-----
- List of APTs on the wiki
- They are associated with the techniques they use, which link to the wiki page
  for the technique

- Palo Alto Unit 42 Playbook Viewer is based on this (?). It's a free tool.

Anomali Weekly Threat Briefing
-----
MITRE ATT&CK technique IDs added to the bottom of articles, to enrich the report

ATT&CK Best Practices
-----
- Use tactics where techniques are ambiguous or difficult to pin down
- Follow external research around detection/mitigation
    - Great pre-existing corpus, make use of it to inform how to detect certain
      techniques
- *Share* any discovered methods of detection/mitigation
    - Don't leave it to everyone else to duplicate your work
- Leverage ATT&CK integration with existing tools
    - Encourage vendors to add support for ATT&CK

Challenges with ATT&CK
-----
- Not all techniques are always malicious
    - Example: Data from Network Shared Drive (T1039)

      What's important is how the technique is being invoked
- Not all techniques are easy to detect
    - Example: Spearphishing Link (T1192)

      Key to detection: Events surrounding the technique execution

MITRE ATT&CK: What is next
-----
- Introduction of a single matrix (filterable by platform)
    - Currently there are multiple matricies (enterprise/mobile, Windows/Linux...)
- Addition of sub-techniques
    - Addresses current techniques that are too broad
    - Example: Credential Dumping (T1003)
        - Multiple methods exist to dump credentials
        - Varies by platform (Windows, Linux, MacOS etc)
- Migration from Wiki to dynamically generated pages from STIX/TAXII 2.0
    - You can digest the entire corpus right now through STIX/TAXII though, there's
      a JSON API
- Evaluation of security tools against ATT&CK techniques

Q&A
-----
- Why isn't it supporting IOT?
    - Linux-based IOTs might find some use in the Linux techniques
    - But a IOT matrix will be made - coming in the future

- Please compare ATT&CK to Cyber kill chain
    - There's a thing called "PRE-ATT&CK" which covers the first 3 steps of the
      kill chain. Combining with Enterprise, it covers the kill chain rather nicely
    - Look up "MITRE ATT&CK framework compared to kill chain" online, lots of
      comparisons

- How do you feed ATT&CK data into your SIEM?
    - Splunk are doing work into automating this.

Can e-mail attack@mitre.org with any questions.

