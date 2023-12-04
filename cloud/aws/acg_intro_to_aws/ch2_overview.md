# Regions and Availability Zones
## Region
- physical location where they put their data centres
- There are regions in North and South America, Europe, Middle East, Africa, and Asia-Pacific

## Availability zones
- multiple per region.
- Physically isolated from one another (in different parts of the city).
- Connected via redundant high speed network connections
- These availability zones will be grouped and then presented to the user as AZs a, b, and c (e.g.
  `eu-west2a`, `eu-west2b`, `eu-west2c`).
  - Note that one customer's `a` AZ may be different to another customer's! This is to spread out
    the use of these data centres (otherwise the `a` availability zone would be hit pretty hard
    compared to the others)
- The point of having access to multiple AZs per region is redundancy
  - It's up to the user to set this up, but if they make sure that their applications are
    distributed over multiple AZs, then said applications will be resilient to power outages etc
    (since it's unlikely that an outage or other event will affect multiple AZs simultaneously)
