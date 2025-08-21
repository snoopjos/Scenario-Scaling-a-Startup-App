# Scenario: Scaling-a-Startup-App
In this scenario users for a startup are complaining that their app is slow during peak hours and sometimes crashes when too many people sign in. The company don’t have money for a big IT team, but need reliability and scalability.

## Proposed-Solution
- Load Balancer to spread traffic.
- VM Scale Sets to scale automatically (cost control).
- Availability Zones for resilience in the closest region to bulk of users.

### Step 1 - Create Resource Group
- "scaling-rg" RG created

### Step 2 - Deploy a Virtual Machine Scale Set (VMSS)
- Availability 1, 2 and 3 selected for reliability
- Explain a marketplace image was used for this example
- Password used for this example instead of SSH
- Add disk for app

### Step 3 - Add a Load Balancer
- 