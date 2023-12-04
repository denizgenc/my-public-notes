# VPC
Allows you to create a private network address range in which to put your resources (or subnets
etc).

Therefore you have to choose something from 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16
(https://en.wikipedia.org/wiki/Private_network#Private_IPv4_addresses)
- Of course, you can restrict the VPC IP range further than this - e.g. 10.1.2.0/24

- NAT Gateway to only allow outbound access to the internet for your instances. (Instances only need
  a private IP.)
- Internet Gateway to allow both inbound and outbound access to the internet (instances need a
  public IP to be routed to).
- Network ACLs that control which traffic is allowed into and out of a given VPC/subnet
  - They are different from Security Groups in that ACLs apply to the whole VPC/subnet, whereas you
    need to assign instances to a security group.

# CloudFront
CDN for websites you serve from AWS. Alongside regular AWS Regions, CloudFront also works from 225
"Edge Locations" that might be closer to users, reducing latency further.

Other features/benefits:
- Protects website from malicious/benign Denial of Service
- Lambda@Edge - run Lambda code from Edge Locations to reduce latency to users
- Metrics available

# Route53
Provides multiple different routing policies:
- Simple routing: return a single IP address, given a domain. As you'd expect.
- Weighted policy: return one of multiple IP addresses, based on a weight value given to those
  addresses (from 0 to 255). Uses:
  - A/B testing different websites, or gradually transitioning over from one to the other
- Geolocation policy: return a different IP address based on the location of the requester
- Latency policy: return the IP address of AWS resources with the lowest latency to the user
- Failover policy: return a single IP address, but if the server at that address goes offline,
  return the failover record.
- Multivalue answer policy: Return one of multiple IP addresses, depending on which of those IP
  addresses are healthy.
  - Not a substitute for a load balancer, but can help with improving availability.
