# Scenario: Scaling a Startup Web App

In this scenario, users of a startup’s **web application** complain that the app becomes **slow during peak hours** and occasionally **crashes when too many people sign in**.  
The company cannot afford a large IT team, but requires **reliability, scalability, and cost efficiency**.  

---

## Proposed Solution
- **Application Gateway**: Distributes traffic between servers with **Layer 7 load balancing** (better suited for web apps than a basic Load Balancer). 
- **Virtual Machine Scale Sets (VMSS)**: Automatically scales the number of VMs up or down based on demand, ensuring cost control.  
- **Availability Zones**: Deploy VMs across multiple zones within the same region to increase resiliency and protect against datacenter-level failures.  

---

## Implementation Steps

### Step 1 - Create a Resource Group
- Create a new Resource Group named **`scaling-rg`** in the closest Azure region to the bulk of customers (e.g., `UK South`).  

---

### Step 2 - Create a Virtual Network
- Create a **VNet** within the resource group.  
- Add two subnets:  
  - **AGSubnet** → for the Application Gateway.  
  - **BESubnet** → for the VM Scale Set instances.  

---

### Step 3 - Create an Application Gateway
- Deploy an **Application Gateway** into the **AGSubnet**.  
- Assign a **Public IP address** so customers can access the web app.  
- Add a public **Frontend IP**.  
- Create a **Backend Pool** without targets for now (adding VMSS on next step).  
- Create a **Listener** for incoming web requests.  
- Set up a **Routing Rule** to forward traffic from the listener to the backend pool.
    - For this scenario, the rule is configured to use the HTTP protocol on port 80 to keep things straightforward. In a production environment, HTTPS would be the preferred option to ensure traffic is encrypted end-to-end. This requires uploading an SSL/TLS certificate to the Application Gateway and binding it to the HTTPS listener.
- Configure a **Health Probe** (e.g., HTTP probe on port 80) to monitor app availability.  

---

### Step 4 - Deploy a Virtual Machine Scale Set (VMSS)
- Deploy `webapp-server-vmss` into the **BackendSubnet**.  
- Select **Availability Zones 1, 2, and 3** for reliability.  
- Use a default marketplace image (e.g., Ubuntu 22.04 LTS or Windows Server 2022).  
- Configure authentication (for this example, password-based rather than SSH).  
- Attach a managed disk to host the company’s web application.  
- Set up **autoscale rules** (e.g., scale out when CPU > 70%, scale in when CPU < 30%).  

---

### Step 5 - Attach Application Gateway to VMSS
- Go to the **VMSS Networking** settings.  
- Add the **Application Gateway backend pool** as the destination for the VMSS.  
- Confirm the VMSS instances are registered with the Application Gateway backend.  
- Verify health probes detect the web app as **healthy** before routing traffic.  

---

## Outcome
With this setup:  
- Incoming traffic is distributed through the **Application Gateway**, ensuring reliability and scalability.  
- The **VMSS automatically scales** up or down based on load, reducing costs.  
- Deployment across **Availability Zones** ensures the app remains available even if one datacenter fails.  
