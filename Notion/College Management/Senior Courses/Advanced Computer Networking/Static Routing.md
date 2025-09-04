
Perfect — this is a very hands-on module, and it builds on the fundamentals we just discussed about static vs dynamic routing. Let’s turn **Module 15: IP Static Routing** into a detailed technical lecture that fully explains the theory, command syntax, and practical application of static routes for both IPv4 and IPv6.

---

# 📖 Lecture: IP Static Routing (Module 15)

---

## 1. **Introduction**

Static routing is the most fundamental way to teach a router how to reach networks beyond its directly connected interfaces.
Unlike dynamic routing protocols, which automatically discover routes and adapt to network changes, **static routing requires explicit configuration by the network administrator**.

> 🎯 **Module Objective:** Configure IPv4 and IPv6 static routes, including default routes, floating static routes, and host routes.

---

## 2. **Types of Static Routes**

Both IPv4 and IPv6 support several types of static routes. These are not mutually exclusive — most networks use a mix of them:

1. **Standard Static Route**

   * Manually defines a path to a network.
   * Example:

     ```plaintext
     ip route 192.168.2.0 255.255.255.0 172.16.2.2
     ```

2. **Default Static Route**

   * Used when a packet’s destination is not found in the routing table.
   * Known as the **gateway of last resort**.
   * Example:

     ```plaintext
     ip route 0.0.0.0 0.0.0.0 172.16.2.2
     ```

3. **Floating Static Route**

   * A backup route with a **higher administrative distance (AD)** than the primary route.
   * It “floats” and only appears in the routing table if the primary route goes down.
   * Example:

     ```plaintext
     ip route 192.168.2.0 255.255.255.0 172.16.2.2 5
     ```

4. **Summary Static Route**

   * Aggregates multiple networks into a single route for efficiency.
   * Example:

     ```plaintext
     ip route 192.168.0.0 255.255.252.0 172.16.2.2
     ```

> ✅ **Why This Matters:** Using the right type of static route minimizes administrative overhead, improves stability, and ensures predictable routing behavior.

---

## 3. **Next-Hop Options**

When configuring a static route, the **next-hop** (where to send the packet) can be specified in three ways:

| **Type**                            | **Configuration**                                  | **Explanation**                                                                                                            |
| ----------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **Next-Hop Route**                  | `ip route NETWORK MASK NEXT-HOP-IP`                | Router only specifies the next-hop IP address. The router must resolve which interface to use via ARP or recursive lookup. |
| **Directly Connected Static Route** | `ip route NETWORK MASK EXIT-INTERFACE`             | Specifies only the outbound interface. Useful for point-to-point links, less processing overhead.                          |
| **Fully Specified Static Route**    | `ip route NETWORK MASK EXIT-INTERFACE NEXT-HOP-IP` | Specifies both next-hop IP and exit interface. Recommended when multiple paths exist through the same interface.           |

> 🔑 **Key Point:** Fully specified routes eliminate recursive lookups and are more efficient on multi-access networks (e.g., Ethernet).

---

## 4. **IPv4 Static Route Command Syntax**

### Command:

```plaintext
Router(config)# ip route network-address subnet-mask {ip-address | exit-intf [ip-address]} [distance]
```

* **network-address** – Destination network you are adding.
* **subnet-mask** – Netmask for the destination network.
* **ip-address** – Next-hop IP address.
* **exit-intf** – Local outgoing interface to use.
* **distance** – (Optional) Administrative distance (used for floating static routes).

> ⚠️ **Important:** You must specify at least the next-hop IP, or the exit interface, or both.

---

## 5. **IPv6 Static Route Command Syntax**

IPv6 uses a very similar command:

```plaintext
Router(config)# ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-intf [ipv6-address]} [distance]
```

Differences:

* Instead of network + mask, you use **prefix/prefix-length** notation.
* All other parameters behave almost identically to IPv4.

---

## 6. **Practical Topology Example**

Imagine a three-router topology (R1–R3) with IPv4 and IPv6 connectivity.
Initially, each router only knows about:

* Its **directly connected networks**
* Its **own local addresses**

**Observation:**

* R1 can ping R2 (directly connected), but not R3 LAN.
* Why? Because there is no route in R1’s routing table to reach R3 LAN.

---

### Example: IPv4 Routing Table Before Static Route

```
R1# show ip route | begin Gateway
Gateway of last resort is not set
172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
C    172.16.2.0/24 is directly connected, Serial0/1/0
L    172.16.2.1/32 is directly connected, Serial0/1/0
C    172.16.3.0/24 is directly connected, GigabitEthernet0/0/0
L    172.16.3.1/32 is directly connected, GigabitEthernet0/0/0
```

* **C** = Connected network
* **L** = Local interface address

R1 tries to ping 192.168.2.1 (R3 LAN) but fails because it has no route to that network.

---

### Example: IPv6 Routing Table Before Static Route

```
R1# show ipv6 route | begin C
C 2001:DB8:ACAD:2::/64 [0/0]
via Serial0/1/0, directly connected
L 2001:DB8:ACAD:2::1/128 [0/0]
via Serial0/1/0, receive
C 2001:DB8:ACAD:3::/64 [0/0]
via GigabitEthernet0/0/0, directly connected
L 2001:DB8:ACAD:3::1/128 [0/0]
via GigabitEthernet0/0/0, receive
```

Same issue — R1 can only reach directly connected subnets, not R3 LAN.

---

## 7. **Solution: Configure a Static Route**

For IPv4:

```plaintext
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2
```

For IPv6:

```plaintext
R1(config)# ipv6 route 2001:DB8:CAFE:2::/64 2001:DB8:ACAD:2::2
```

Now, R1 can reach R3 LAN because the routing table has a path to forward those packets.

---

## 8. **Verification**

* Use `show ip route` or `show ipv6 route` to confirm the route is installed.
* Use `ping` or `traceroute` to test connectivity.
* If route does not appear, check:

  * Correct network/prefix length.
  * Interface status (must be UP/UP).
  * Next-hop reachability.

---

## 9. **Floating Static Routes**

To provide a **backup path**, configure a static route with a **higher administrative distance** than the primary dynamic or static route.
If the primary route disappears from the table, the floating route is activated.

Example:

```plaintext
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2 5
```

> Here, **5** is the AD, which is higher than a directly connected network (AD = 0) or a dynamic protocol like OSPF (AD = 110).

---

## 10. **Static Host Routes**

Sometimes you need a route to a **single host**, not a whole network.
This is done with a **/32 prefix** for IPv4 or **/128 prefix** for IPv6.

Examples:

```plaintext
ip route 192.168.2.55 255.255.255.255 172.16.2.2
ipv6 route 2001:DB8:CAFE:2::55/128 2001:DB8:ACAD:2::2
```

Useful for:

* Remote management of a specific host.
* Route injection for very targeted traffic flows.

---

## 11. **Best Practices**

* **Document your static routes** — easy to forget in complex networks.
* **Prefer fully specified routes** on Ethernet segments to avoid ARP recursion issues.
* **Use floating static routes** for backup, not as primary paths (to avoid manual failover).
* **Combine static and dynamic routing** — static for critical links, dynamic for scalability.
* **Regularly verify routes** with `show ip route` to ensure correctness after topology changes.

---

## 12. **Key Takeaways**

* Static routes are manually configured paths, useful for small or predictable networks.
* Types include standard, default, floating, and summary static routes.
* Next-hop options: IP address, exit interface, or both (fully specified).
* IPv4 uses `ip route`; IPv6 uses `ipv6 route`.
* Floating static routes act as backup routes by setting a higher administrative distance.
* Static host routes are highly specific and route to a single host.

---

Would you like me to expand this further into a **lab-style walk-through**, where I show step-by-step configuration, verification, and troubleshooting commands (with sample outputs), so you can practice this in Packet Tracer or GNS3? This would make it highly practical.


Alright — let’s treat this like a proper lecture. We’re in **Module 15.2: Configuring IP Static Routes**, and I’ll guide you through **how to actually configure IPv4 and IPv6 static routes**, explaining what each configuration style means, when to use it, and why certain best practices exist.

---

# 📖 Lecture: Configuring IP Static Routes (IPv4 & IPv6)

In the previous section, we explored what static routes are and why they’re important — they’re manually configured routes that tell the router **exactly how to forward packets to a destination network**. Now, we move to the **practical side**: *how to configure them properly*.

---

## 1. **Next-Hop Static Routes (IPv4 & IPv6)**

### **Concept**

A **next-hop static route** is the simplest and most common type of static route.
Here, you specify only the **next-hop IP address** — not the interface.
The router will figure out which interface to use by looking up the next-hop address in its routing table (recursive lookup).

---

### **IPv4 Example**

```bash
R1(config)# ip route 172.16.1.0 255.255.255.0 172.16.2.2
R1(config)# ip route 192.168.1.0 255.255.255.0 172.16.2.2
R1(config)# ip route 192.168.2.0 255.255.255.0 172.16.2.2
```

Here’s what’s happening:

* `ip route` → Command to create an IPv4 static route
* `172.16.1.0` → Destination network
* `255.255.255.0` → Subnet mask
* `172.16.2.2` → Next-hop IP address (on the directly connected router)

💡 **Key point:** The router must have a way to reach `172.16.2.2` (it must be on a directly connected interface).

---

### **IPv6 Example**

```bash
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:1::/64 2001:db8:acad:2::2
R1(config)# ipv6 route 2001:db8:cafe:2::/64 2001:db8:acad:2::2
```

Differences:

* The command is `ipv6 route`.
* The network is expressed as `prefix/prefix-length` rather than subnet mask.
* You must enable IPv6 unicast routing first (`ipv6 unicast-routing`).

---

### **When to Use Next-Hop Routes**

✅ Best practice for most situations.
✅ Works well with point-to-point and multi-access networks.
⚠️ But note: The router must perform an **extra lookup** to determine the outgoing interface (recursive lookup). This is usually negligible on modern routers but still something to be aware of.

---

## 2. **Directly Connected Static Routes**

Instead of specifying the next-hop IP, you can specify the **exit interface directly**.

---

### **IPv4 Example**

```bash
R1(config)# ip route 172.16.1.0 255.255.255.0 s0/1/0
R1(config)# ip route 192.168.1.0 255.255.255.0 s0/1/0
R1(config)# ip route 192.168.2.0 255.255.255.0 s0/1/0
```

Here, `s0/1/0` is the **serial interface** used to send packets out.

---

### **IPv6 Example**

```bash
R1(config)# ipv6 route 2001:db8:acad:1::/64 s0/1/0
R1(config)# ipv6 route 2001:db8:cafe:1::/64 s0/1/0
R1(config)# ipv6 route 2001:db8:cafe:2::/64 s0/1/0
```

---

### **When to Use Directly Connected Routes**

* Recommended **only on point-to-point interfaces** (serial links).
* Not recommended on Ethernet or multi-access networks because the router might need to ARP for every destination network, which can be inefficient.

---

## 3. **Fully Specified Static Routes**

This type combines both approaches: you specify **both the exit interface and the next-hop IP**.

---

### **IPv4 Example**

```bash
R1(config)# ip route 192.168.10.0 255.255.255.0 g0/0/0 192.168.20.2
```

* Here, the router knows *exactly* which interface to use (`g0/0/0`) **and** which next hop to send to (`192.168.20.2`).

---

### **IPv6 Example**

A special case in IPv6: If you use a **link-local address** as the next hop, you *must* specify the exit interface, because:

* Link-local addresses are not unique across the router.
* The router must know which interface to use for that link-local next hop.

```bash
R1(config)# ipv6 route 2001:db8:acad:1::/64 g0/0/0 fe80::1
```

Here `fe80::1` is the link-local address of the next-hop router.

---

### **When to Use Fully Specified Routes**

* Required for IPv6 when using link-local addresses.
* Recommended for multi-access Ethernet networks to avoid ambiguity.

---

## 4. **Verification Tools**

Once you configure static routes, always verify them:

* `show ip route` or `show ipv6 route` → Displays full routing table.
* `show ip route static` → Shows only static routes.
* `ping` and `traceroute` → Test connectivity.
* `show running-config | section ip route` → Verify configuration commands.

---

## 📌 Key Takeaways

* **Next-Hop Routes** – Most common, simple to use. Specify just next hop IP.
* **Directly Connected Routes** – Specify just interface. Best only for point-to-point links.
* **Fully Specified Routes** – Specify both interface and next hop. Required for IPv6 link-local routes, recommended for multi-access networks.
* Always verify routes with `show`, `ping`, and `traceroute`.
* Best practice: Prefer next-hop routes or fully specified routes over interface-only routes for scalability and efficiency.

---

Would you like me to cover the **next section (15.3 – Configuring Default and Floating Static Routes)** as well so you get the full picture of static route usage in real networks (including redundancy)? This ties together everything by showing how you handle failover and unknown networks.


Excellent — now we’re moving into the **final two critical sections** of Module 15: **Floating Static Routes** (Section 15.4) and **Static Host Routes** (Section 15.5).
These two topics are where static routing becomes *really powerful* and practical in production networks, because now we’re talking about **redundancy** (failover) and **precision routing** (host-specific routes).

Let’s treat this as a complete lecture.

---

# 📖 Lecture: Floating Static Routes & Static Host Routes

---

## 1. **Floating Static Routes**

### **Definition**

A **floating static route** is a static route that acts as a **backup path** to a destination network.
It is called *floating* because it is **not normally used**; it only “floats” into the routing table when the **primary route becomes unavailable**.

---

### **The Role of Administrative Distance**

* **Administrative Distance (AD):** A numerical value representing the *trustworthiness* of a route.

  * Lower AD = more preferred.
  * Example: Connected routes (AD=0) are preferred over static routes (AD=1), which are preferred over dynamic routing protocols like RIP (AD=120).
* By default:

  * Static routes have an AD of **1** (very trustworthy).
  * Dynamic routes usually have higher ADs, so static routes normally override them.

To make a static route *less preferred* than an existing route (so it’s only used as backup), we manually set its AD **higher** than the primary route.

---

### **Key Principle**

> The floating static route will only be installed into the routing table if there is no other route with a lower AD to the same destination.

---

### **IPv4 Configuration Example**

```bash
R1(config)# ip route 0.0.0.0 0.0.0.0 172.16.2.2
R1(config)# ip route 0.0.0.0 0.0.0.0 10.10.10.2 5
```

Explanation:

* First route → Primary default route (AD=1 by default).
* Second route → Floating static route with AD=5.
  This route will only take over if the first one disappears from the routing table.

---

### **IPv6 Configuration Example**

```bash
R1(config)# ipv6 route ::/0 2001:db8:acad:2::2
R1(config)# ipv6 route ::/0 2001:db8:feed:10::2 5
```

* `::/0` is the IPv6 default route.
* The second route uses a higher AD (5) to act as a backup.

---

### **Verification**

You can verify routes with:

* `show ip route` or `show ipv6 route` → Look for which default route is installed.
* If primary is available, only the primary route will show in the table.
* If primary fails (interface down, neighbor unreachable), the floating route will be installed automatically.

---

### **Testing Floating Routes**

* Administrators often simulate failure by **shutting down an interface** on the primary path.
* Router logs syslog messages showing link-down events.
* A new `show ip route` output will reveal that the floating route has taken over.

💡 **Real-World Use Case:**
Floating static routes are excellent for **failover scenarios** — such as backing up a dynamic routing protocol path or providing a backup route through a slower secondary ISP.

---

---

## 2. **Static Host Routes**

### **Definition**

A **host route** is the most specific type of route — it points to **a single host address** rather than an entire network.

* **IPv4 Host Route:** A route with a `/32` subnet mask (255.255.255.255).
* **IPv6 Host Route:** A route with a `/128` prefix length.

---

### **Three Ways Host Routes Are Added**

1. **Automatically Installed:**

   * When you assign an IP address to a router interface, IOS automatically installs a host route for that interface’s address.
   * This allows the router to process packets destined *for itself* more efficiently.
   * These appear with an `L` (local) in the routing table.

2. **Manually Configured as Static Host Route:**

   * Admin explicitly defines a route to a specific host.
   * Useful for directing traffic to a single important device, such as a server, through a specific next hop.

3. **Obtained Through Other Means:**

   * Learned via other mechanisms (covered in later modules).

---

### **Routing Table Representation**

* **Connected Routes:** Marked with `C` — network-level information.
* **Local Host Routes:** Marked with `L` — interface’s own IP address.

Example of `show ip route`:

```
C 192.168.10.0/24 is directly connected, GigabitEthernet0/0
L 192.168.10.1/32 is directly connected, GigabitEthernet0/0
```

---

### **Static Host Route Configuration**

#### **IPv4 Example**

```bash
Branch(config)# ip route 209.165.200.238 255.255.255.255 198.51.100.2
```

* `209.165.200.238` → The specific host to reach (e.g., a server)
* `255.255.255.255` → Host route mask
* `198.51.100.2` → Next hop

#### **IPv6 Example**

```bash
Branch(config)# ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2
```

* Same principle, but with `/128`.

---

### **IPv6 Link-Local Next-Hop Requirement**

If using an **IPv6 link-local address** as next hop, you must specify the interface because:

* Link-local addresses are not globally unique.
* The router must know which interface to use.

Example:

```bash
Branch(config)# no ipv6 route 2001:db8:acad:2::238/128
Branch(config)# ipv6 route 2001:db8:acad:2::238/128 fe80::1 g0/0/0
```

Here:

* `fe80::1` is the link-local address of the next-hop router.
* `g0/0/0` explicitly tells IOS which interface to use.

---

### **Verification**

Use:

* `show ip route 209.165.200.238`
* `show ipv6 route 2001:db8:acad:2::238/128`
  to confirm that host routes are installed and active.

---

---

## 📌 Key Takeaways

* **Floating Static Routes:**

  * Provide **backup connectivity**.
  * Use a higher **administrative distance** than the primary route.
  * Only active when the primary route fails.

* **Host Routes:**

  * Direct traffic to a **specific host**.
  * IPv4 uses `/32`, IPv6 uses `/128`.
  * Often used for precision routing or managing traffic to important endpoints.

* **Best Practice:**

  * Always verify configuration with `show` commands.
  * Test failover for floating static routes to ensure redundancy works as expected.
  * Use fully specified routes with link-local addresses in IPv6.

---

Would you like me to cover **Module 15.3 (Default Static Routes)** as well so you get the full picture of how all three — next-hop, floating, and default — work together in designing a complete static routing strategy? It will help you see how networks handle **unknown destinations** and provide a single path to the Internet.
