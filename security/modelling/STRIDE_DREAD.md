Threat modelling: STRIDE and DREAD
-----
In the process of threat modelling one examines the application and deconstructs
it to smaller parts - features and modules - that do a certain thing. From these
parts threats are identified and from these threats vulnerabilities. This
process can continue, which each part being further deconstructed to even
smaller parts.

To simplify modelling, there are multiple ways of classifying threats. Two
examples are the STRIDE threat model, and the DREAD risk assessment model.

To understand the difference between threat and risk:
- Threats are potential problems that can arise from a system.
- Risk is how severe a threat is

-----
The STRIDE Threat Model

STRIDE stands for the following:
- Spoofing
    - Accessing a system using another user's authentication information
- Tampering
    - Unauthorised changes made to persistent data (whether at rest or in
      transit)
      (violates *integrity*)
- Repudiation
    - A system should be able to trace user operations to provide evidence
      of what has happened
      (A security concept is "non-repudiation" - a user cannot deny
      knowledge of performing an action on a system. For example, they
      should not be able to deny making a payment after the bank has
      transferred funds)
- Information Disclosure
    - The exposure of information of data to unauthorised people, whether
      the data is at rest or in transit
      (violates *confidentiality*)
- Denial of Service
    - The system being rendered unavailable
      (violates *availability*)
- Elevation of Privilege
    - An unprivileged user finds a way to gain sufficient privileges to
      compromise the system.

-----
The DREAD Risk Assessment Model

DREAD prioritises threats based on their severity, and stands for:
- Damage
- Reproducibility
- Exploitability
- Affected Users
- Discoverability

A scale of 0-10 is usually used in all categories, save for Discoverability
which is commonly set to 10 on the grounds that any threat will eventually be
discovered.

