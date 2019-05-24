# This is a TEST of GITPOD #

# Runbook VIO Assesment:

## Section1:

### Explanation Overall Architecture

Scope of this section is to show case the overall system architecture, give an overview of all the involved components and highlight the baseline integration parts.

#### Installation Check

- Open vCenter
    - Home Icon
    -- VMware Integrated OpenStack
    -- check configuration
    -- check Openstack Deployments - VIO install

- Open Horizon
    - Verify the User
    - Verify the Computes / Nova: Admin -> Compute -> Hypervisors
    - check baseline config: Project -> Networks ; Project -> Compute

- Open NSX-T Manager
    - Show Dashboard
    -- Verify System
    - Show Fabric - Hosts
    - Show Switching
    - Show Routing
    - Show Firewalling

## Section2:
### Manual OpenStack Provisioning:

Scope of this section is to use VIO via the Horizon GUI and provisioning a baseline Network/Service/VM. Afterwards all the corresponding configuration should be also reflected in NSX-T and will be double-checked and explained.

#### Configuration via Horizon UI Manual:

- Create a Network
	- Project -> Network -> Networks -> Create a Network - manual_web
	-- manual_web_sn - Name
	-- 172.16.1.0/24 - Address
	-- 172.16.1.1 - Gateway
	- enable DHCP
	- enter dummy DNS
- Confirm Network Creation inside the project
- Create a Router
	- Project -> Network -> Networks -> Create Router
	-- manual_tenant_router - Name
	-- external-network - set
	-- leave rest
- Add Interface to Router
	- Interfaces under Router manual_tenant_router
	-- Add Interface
	-- select manual_web_sn
- Review Network Topology (keep in mind there is existing Config)
- Create a new Security Group
	- Project -> Network -> Security Groups -> Create Security Group
	-- Name - web-sg
	-- description - web
	- Manage Rules
	-- Add Rule
	--- All TCP / Ingress / CIDR / 0.0.0.0/0
	--- All ICMP / Ingress / CIDR / 0.0.0.0/0
	- Verification in UI

#### Verification in NSX-T:

- Switching
	- Verify Switch manual_web_sn is created as overlay with a VNI
	- Verify Switch is part of the transport Zone
	- Check Switch properties
	-- Admin Status
	-- Creation
	- Verify Routing
	-- Check if new T1 Router was created
	-- Check Mode of the Router
	-- Check the TZ
	-- Check Services / NAT for external reachability
	- Check Firewall
	-- Check if web-sg was created
	-- check if rules according to OpenStack are correct

#### Launch Instance

- Log into Horizon
	- Project -> Compute -> Instances -> Launch Instance
	-- Instance Name - manual_web
	-- Count - 2
	-- Image - cirros_new
	-- Flavor - m1.small Flavor
	-- Networks - manual_web
	-- Security Groups - web-sg
	- Launch Instance

#### Verify Instances

- Check Projects -> Compute -> Instances if the instance is up and running
- Check Overview
-- Specs
-- IP Addresses
-- Security Groups
- Login via Console
- Test the environment via Ping

## Section3:
### Automated OpenStack Provisioning:

Scope of this section is to show case the usage of OpenStack Automation patterns like HEAT templates and the corresponding automated provisioning of resources and configuration inside VIO and NSX-T

#### Stack1

- Open VIO
	- Project -> Orchestration -> Stacks
	- run onevm-1lnet-1floatingip Stack
	- Check the corresponding resources like in Section2
		- VIO
		-- Compute
		-- Networking
- Open NSX-T Manager
	- Check the corresponding resources like in Section2
		-- Switching
		-- Routing

#### Stack2

- Open VIO
	- Project -> Orchestration -> Stacks
	- run  Stack
	- Check the corresponding resources like in Section2
		- VIO
		-- Compute
		-- Networking
- Open NSX-T Manager
	- Check the corresponding resources like in Section2
		-- Switching
		-- Routing



