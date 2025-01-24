DHCP Snooping and Server Configuration

## Lab Objective
This lab focuses on configuring:
1. A router (R1) as a DHCP server.
2. Implementing DHCP snooping on SW1 and SW2 with trusted uplink ports.
3. Verifying DHCP functionality using `ipconfig /renew`.
4. Troubleshooting and resolving configuration issues to ensure DHCP works as intended.

---

## Explicit Instructions
To complete this lab, follow these steps:

1. **Configure R1 as a DHCP server:**
   - Exclude IP addresses 192.168.1.1 to 192.168.1.9 from the pool.
   - Create a DHCP pool named `POOL1` with a network address of 192.168.1.0/24.
   - Assign the default gateway 192.168.1.1 to DHCP clients.
   - Save the configuration.

2. **Enable DHCP Snooping on SW1 and SW2:**
   - Enable DHCP snooping globally.
   - Enable DHCP snooping for VLAN 1.
   - Set the uplink interfaces (G0/2 on SW1 and G0/1 on SW2) as trusted.
   - Save the configuration.

3. **Test DHCP functionality:**
   - Run `ipconfig /renew` on PC1 to verify DHCP assigns an IP address.

4. **Troubleshoot and fix any issues:**
   - Verify trusted port configurations and correct as needed.
   - Ensure DHCP snooping is enabled on the correct VLAN.
   - Retest `ipconfig /renew` to confirm the fix.

---

## Task Breakdown

### Task 1: Configure R1 as a DHCP Server
#### Commands:
1. Exclude addresses from the pool:
   ```
   R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.9
   ```
2. Create a DHCP pool named `POOL1`:
   ```
   R1(config)# ip dhcp pool POOL1
   ```
3. Specify the network and subnet mask:
   ```
   R1(dhcp-config)# network 192.168.1.0 255.255.255.0
   ```
4. Set the default gateway:
   ```
   R1(dhcp-config)# default-router 192.168.1.1
   ```
5. Save the configuration:
   ```
   R1# wr
   ```

#### Explanation:
- The excluded addresses prevent overlapping with static IPs (e.g., router interfaces).
- The DHCP pool assigns IP addresses dynamically to devices in the network.

---

### Task 2: Enable DHCP Snooping on SW1 and SW2
#### SW1:
1. Enable DHCP snooping globally:
   ```
   SW1(config)# ip dhcp snooping
   ```
2. Enable DHCP snooping for VLAN 1:
   ```
   SW1(config)# ip dhcp snooping vlan 1
   ```
3. Configure the uplink interface (G0/2) as trusted:
   ```
   SW1(config)# interface g0/2
   SW1(config-if)# ip dhcp snooping trust
   ```
4. Save the configuration:
   ```
   SW1# wr
   ```

#### SW2:
1. Enable DHCP snooping globally:
   ```
   SW2(config)# ip dhcp snooping
   ```
2. Enable DHCP snooping for VLAN 1:
   ```
   SW2(config)# ip dhcp snooping vlan 1
   ```
3. Configure the uplink interface (G0/1) as trusted:
   ```
   SW2(config)# interface g0/1
   SW2(config-if)# ip dhcp snooping trust
   ```
4. Save the configuration:
   ```
   SW2# wr
   ```

#### Explanation:
- DHCP snooping ensures that only authorized DHCP servers can respond to requests.
- Uplink ports to the router must be trusted; otherwise, DHCP messages will be dropped.

---

### Task 3: Test DHCP Functionality
#### Steps:
1. On PC1, open the Command Prompt and run:
   ```
   ipconfig /renew
   ```
2. Observe the output:
   - If the DHCP server is properly configured and DHCP snooping is correctly set up, PC1 will receive an IP address (e.g., `192.168.1.10`).

#### Initial Result:
- The `DHCP request failed.` This indicates a misconfiguration in DHCP snooping or untrusted uplink interfaces.

---

### Task 4: Troubleshoot and Fix Configuration
#### Issues Identified:
1. The uplink interfaces (G0/2 on SW1 and G0/1 on SW2) were not set as trusted.
2. DHCP snooping might not have been applied to the correct VLAN.

#### Fix:
1. Configure the uplink ports as trusted:
   - On **SW1**:
     ```
     SW1(config)# interface g0/2
     SW1(config-if)# ip dhcp snooping trust
     ```
   - On **SW2**:
     ```
     SW2(config)# interface g0/1
     SW2(config-if)# ip dhcp snooping trust
     ```
2. Ensure DHCP snooping is enabled for VLAN 1:
   - On both switches:
     ```
     SW1(config)# ip dhcp snooping vlan 1
     SW2(config)# ip dhcp snooping vlan 1
     ```
3. Save configurations:
   ```
   SW1# wr
   SW2# wr
   ```
4. Retest DHCP on PC1:
   ```
   ipconfig /renew
   ```

#### Final Result:
- PC1 successfully received an IP address (`192.168.1.10`), confirming the issue was resolved.

---

## Summary
This lab demonstrated:
- Configuring a DHCP server on R1.
- Implementing DHCP snooping on switches to prevent unauthorized DHCP responses.
- Troubleshooting and resolving issues by ensuring proper trust settings and VLAN configurations.

### Key Takeaways
- **DHCP Snooping:** Protects against rogue DHCP servers.
- **Trusted Ports:** Always configure uplinks to the router as trusted.
- **Testing:** Use `ipconfig /renew` to verify IP address assignment.

![image](https://github.com/user-attachments/assets/0b57caf0-ef01-45ec-aed5-509a1194162c)

