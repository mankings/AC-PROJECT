#### Objective:

Establish basic connectivity between the core router and PoPs (Lisbon, Porto, Barcelona, Chicago).

---

#### **Configuration for the Core Router (Router Core):**

1. **Configure the interfaces connected to PoPs:**
    
    ```plaintext
    enable
    configure terminal
    interface FastEthernet0/0
     ip address 10.0.0.80 255.255.255.240
     no shutdown
    interface FastEthernet1/0
     ip address 10.0.0.96 255.255.255.240
     no shutdown
    interface FastEthernet1/1
     ip address 10.0.0.128 255.255.255.240
     no shutdown
    interface FastEthernet2/0
     ip address 10.0.0.112 255.255.255.240
     no shutdown
    ```
    
2. **Activate OSPF on the Core Router:**
    
    ```plaintext
    router ospf 1
     network 10.0.0.80 0.0.0.15 area 0
     network 10.0.0.96 0.0.0.15 area 0
     network 10.0.0.128 0.0.0.15 area 0
     network 10.0.0.112 0.0.0.15 area 0
    ```
    

---

#### **Configuration for the PoPs:**

Each PoP router needs its respective interface configured and OSPF activated.

- **Lisbon Router:**
    
    ```plaintext
    enable
    configure terminal
    interface FastEthernet0/0
     ip address 10.0.0.82 255.255.255.240
     no shutdown
    interface FastEthernet1/0
     ip address 10.0.0.83 255.255.255.240
     no shutdown
    router ospf 1
     network 10.0.0.80 0.0.0.15 area 0
    ```
    
- **Porto Router:**
    
    ```plaintext
    enable
    configure terminal
    interface FastEthernet0/0
     ip address 10.0.0.98 255.255.255.240
     no shutdown
    interface FastEthernet1/0
     ip address 10.0.0.99 255.255.255.240
     no shutdown
    router ospf 1
     network 10.0.0.96 0.0.0.15 area 0
    ```
    
- **Barcelona Router:**
    
    ```plaintext
    enable
    configure terminal
    interface FastEthernet0/0
     ip address 10.0.0.130 255.255.255.240
     no shutdown
    interface FastEthernet1/0
     ip address 10.0.0.131 255.255.255.240
     no shutdown 
    router ospf 1
     network 10.0.0.128 0.0.0.15 area 0
    ```
    
- **Chicago Router:**
    
    ```plaintext
    enable
    configure terminal
    interface FastEthernet0/0
     ip address 10.0.0.114 255.255.255.240
     no shutdown
    interface FastEthernet1/0
     ip address 10.0.0.115 255.255.255.240
     no shutdown
    router ospf 1
     network 10.0.0.112 0.0.0.15 area 0
    ```
    

---

#### **Verification:**

- Use the `ping` command from the core router to ensure connectivity:
    
    ```plaintext
    ping 10.0.1.2
    ping 10.0.2.2
    ping 10.0.3.2
    ping 10.0.4.2
    ```
    
- Confirm that all routers can communicate through OSPF.

To confirm all routers are communicating through OSPF, follow these steps:

---

### **Step 1: Verify OSPF Neighbors**

Check if OSPF neighbors are established on each router (core and PoPs):

- **Command:**
    
    ```plaintext
    show ip ospf neighbor
    ```
    
- **Expected Output:**
    
    - The output should list all connected OSPF neighbors with their states.
    - Neighbor states such as `Full` or `2-Way` indicate proper communication.

---

### **Step 2: Check OSPF Routing Table**

Verify if OSPF has populated the routing table with the correct routes.

- **Command:**
    
    ```plaintext
    show ip route ospf
    ```
    
- **Expected Output:**
    
    - All connected subnets from other routers should appear in the routing table as OSPF-learned routes (`O`).
    - Example:
        
        ```plaintext
        O 10.0.1.0/24 [110/20] via 10.0.1.2, 00:00:12, FastEthernet0/0
        O 10.0.2.0/24 [110/20] via 10.0.2.2, 00:00:12, FastEthernet1/0
        ```
        

---

### **Step 3: Debug OSPF (if necessary)**

If there are issues, enable debugging to observe OSPF messages.

- **Command:**
    
    ```plaintext
    debug ip ospf adj
    ```
    
    - This will show the adjacency formation process and identify any potential issues.
- **Stop Debugging:**
    
    ```plaintext
    undebug all
    ```
    

---
### **Step 4: Check OSPF Process**

Verify that the OSPF process is running correctly.

- **Command:**
    
    ```plaintext
    show ip ospf
    ```
    
- **Expected Output:**
    
    - Displays OSPF process ID, router ID, and details about OSPF areas and neighbors.

---

If all these steps are successful, the routers are correctly communicating through OSPF. Let me know if further assistance is needed!