---
title: "EC2 Purchase Options"
tags:
- aws
- ec2
disableToc: false
---

### Related Notes
- [EC2](/notes/aws/ec2.md)

## EC2 Purchase Options (Launch Types)

### On Demand
- Default purchase option
- Isolated instances, but multiple customer instances run on the same shared hardware
- Instances of different sizes run on the same EC2 hosts - consuming a defined allocation of resources
- Uses per-second billing while an instance is running. Associated resources such as storage consume capacity, so bill, regardless of instance state.
- No interruption
- No capacity reservation
- Predictable pricing
- No upfront cost
- No discounts
- Good for
	- Short term workloads
	- Unknown workloads
	- Apps which can't be interrupted

### Spot
- Cheapest way to access EC2 capacity
- Spot pricing is AWS selling unused EC2 host capacity for up to 90% discount - the spot price is based on the spare capacity at a given time
- Non time critical
- Anything which can be rerun
- Bursty capacity needs
- Cost sensitive workloads
- Anything which is stateless

### Standard Reserved
- Highest priority compute option
- Long term, consistent usage of EC2
- Reduces or removes per second price for that instance
- Unused reservations are still billed
- Reservations are for 1 year or 3 year terms
	- If you pay nothing upfront, you save money for agreeing to the term, but are still charged a per second fee.
	- If you pay all upfront, you gain the benefit of no per second fee. This is the cheapest option.
	- You can also pay partial upfront, for a reduced per second fee.
- With reserved instances, you won't have interruption

### Scheduled Reserved Instances
- Scheduled Reserved Instances (Scheduled Instances) enable you to purchase capacity reservations that recur on a daily, weekly, or monthly basis, with a specified start time and duration, for a one-year term. You reserve the capacity in advance, so that you know it is available when you need it. You pay for the time that the instances are scheduled, even if you do not use them.
- Ideal for long term usage which doesn't run constantly
- Example: Batch Processing daily for 5 hours starting at 23:00
- Example: Weekly data, sales analysis .. every Friday for 24 hours
- Doesn't support all instance types or regions. 1,200 hours per year & 1 year term minimums

### Convertible Reserved Instances
Convertible Reserved instances allow you to exchange for another Convertible Reserved instance with a different instance type and tenancy.

### Dedicated Hosts
- No instance charges
- Pay for host
- Host affinity links instances to hosts
- Generally used when you have strict licensing requirements, based on Sockets/Cores

### Dedicated Instances
- Your Instances run on an EC2 host with other instances of yours, but no other customers can have access to that hardware.
- You don't own, or share the host
- You have extra charges for instances, but dedicated hardware
- One-off hourly fee for any regions where you use Dedicated Instances
- Used when you're required not to share hardware, but you don't want to manage the host itself.

### Capacity Reservations
- Different from Reserved Instance Purchases
- Regional Reservation provides a billing discount for valid instances launched in any AZ in that region.
	- While this is flexible, it doesn't reserve capacity within an AZ - which is risky during major faults when capacity is limited.
- Zonal Reservations only apply to one AZ providing billing discounts and capacity reservation in that AZ
	- Full Price & No Capacity Reservation if you launch in a different AZ
- Both Regional and Zonal Reservations require a 1 or 3 year commitment to AWS.
- On-Demand Capacity Reservations can be booked to ensure you always have access to capacity in an AZ when you need it - but at full on-demand price. No term limits - but you pay regardless of if you consume it or not.
	- No cost reductions