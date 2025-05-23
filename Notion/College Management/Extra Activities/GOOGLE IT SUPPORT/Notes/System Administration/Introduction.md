  

  

**IT Infrastructure:**

- Encompasses software, hardware, networks, and services required for an organization's operations.
- Includes everything from employee computers to website servers and file-sharing systems.

  

**Sysadmins: The Backbone of IT:**

- Sysadmins manage and maintain IT infrastructure, ensuring everything runs smoothly.
- They work behind the scenes, preventing IT disasters and keeping things operational.

  

  

1. **Nature of IT Services:**
    - IT services, such as email, file storage, and websites, are crucial for employee productivity.
    - These services need to be stored and provided by servers.
2. **Definition of Servers:**
    - A server, whether software or a machine, provides services to other software or machines.
    - Examples include web servers, email servers, and SSH servers.
3. **Clients and Servers:**
    - Clients request services from servers.
    - Servers respond by providing the requested services.
    - A server can serve multiple clients, and a client can use multiple servers.
4. **Server Hardware:**
    - Industry-standard servers are powerful and reliable machines.
    - They come in various forms, including towers, rack servers, and blade servers.
    - Hardware can be customized based on the specific service requirements.
5. **Connecting to Servers:**
    
    - Remotely connecting to servers is a common practice, especially in small IT organizations.
    - SSH is a widely used protocol for secure remote connections.
    - Having a KVM switch is industry practice, allowing control of multiple servers with one set of keyboard, video, and mouse.
    
      
    
      
    
    The Cloud
    
    1. **Cloud Computing:**
        - Access data, applications, and services from anywhere with an Internet connection.
        - The Cloud is essentially a network of servers.
    2. **Datacenters:**
        - Facilities that store numerous servers.
        - Companies, like Google and Facebook, often own their data centers due to the scale of users.
        - Smaller companies may rent space in data centers.
    3. **Advantages of Cloud Services:**
        - Instead of managing servers, organizations can use services that handle hardware, updates, and security.
        - Popular file storage services, like Dropbox, store data in managed locations.
    4. **Considerations and Drawbacks:**
        - **Cost:** Upfront hardware costs for self-managed servers vs. ongoing subscription costs for cloud services.
        - **Dependency:** Relying on cloud platforms means entrusting your data to their infrastructure.
    5. **Responsibility:**
        
        - Regardless of the chosen method, organizations are responsible for addressing issues promptly.
        - Backup strategies, including both Cloud and physical storage, can mitigate risks.
        
          
        
          
        
    
    Organizational Policies
    
    1. **Software Installation:**
        - _Question:_ Should users be allowed to install software?
        - _Consideration:_ Limiting user installation reduces the risk of unintentional installation of malicious software.
    2. **Password Complexity:**
        - _Question:_ Should users have complex passwords?
        - _Consideration:_ Complex passwords with a mix of symbols, numbers, and letters enhance security. A minimum length requirement adds an extra layer of protection.
    3. **Internet Usage:**
        - _Question:_ Should users access non-work-related websites?
        - _Consideration:_ Balancing productivity and freedom—some organizations restrict, while others allow access for personal or business promotion.
    4. **Device Security:**
        - _Question:_ Should company phones have a device password?
        - _Consideration:_ Essential for protecting company data in case of device loss or theft.
    5. **Documentation:**
        - _Consideration:_ Policies, procedures, and other crucial information need to be documented. Accessibility through internal Wiki sites or file servers ensures that everyone is on the same sheet of music.
    
    In larger companies, the Chief Security Officer often shoulders the responsibility of policy decisions. However, in smaller setups, collaboration between the sysadmin and organizational leaders is key. It's a bit like a tight-knit band where each member contributes to the overall sound.
    
      
    
      
    
    User and Hardware Provisioning
    
    1. **User Maestro:**
        - _Creation and Removal:_ Sysadmins compose the creation and removal of user accounts for seamless transitions.
    2. **Hardware Virtuoso:**
        
        - _Provisioning and Setup:_ Allocating, tagging, imaging, and setting up machines for new employees—all part of the symphony.
        - _Standardization:_ Crafting harmony by ensuring hardware and settings follow a standardized rhythm.
        - _Lifecycle Choreography:_ Sysadmins dance through the hardware lifecycle, considering procurement, deployment, maintenance, and retirement.
        - _Streamlined Processes:_ In smaller setups, each machine's journey, from allocation to retirement, is personalized. Automation takes the stage in larger setups, orchestrating efficient provisioning and replacements.
        
          
        
    
    In system administration, the mantra is "update and maintain." Managing a fleet involves strategic batch updates, typically monthly, to minimize downtime and ensure security. Not every software update requires immediate attention; priority goes to critical and security updates. The golden rule: Routinely install the latest security patches for robust and resilient systems.
    
      
    
    Vendors
    
    **I. Introduction**
    
    Sys admins in small companies manage user computers and diverse hardware, including phones, printers, and conferencing systems.
    
    **VI. Procurement of Hardware**
    
    - Procure hardware in enterprise settings.
    - Collaborate with vendors, establish business accounts, and leverage discounts.
    
    **VII. Strategic Hardware Planning**
    
    - Consider scalability and weigh options before procurement.
    - Adapt to changes in technology, ensuring suitable backups.
    
    **VIII. Approval Processes**
    
    - Seek formal approval for vendor relationships.
    - Ensure compliance with budget constraints and organizational goals.
    
      
    
      
    
    The two skills important for sysadmins are troubleshooting and customer service.
    
      
    
      
    
    **Responsible Use of Administrative Rights**
    
    **I. Introduction**
    
    Administrative rights, whether for one machine or a large fleet, come with responsibilities. Avoid unnecessary use and be cautious in your actions.
    
    **II. Minimize Administrative Sessions**
    
    - Avoid browsing the web or spending unnecessary time in an administrative session.
    - In Linux systems, use the sudo command responsibly.
    
    **III. Respect Privacy**
    
    - Do not misuse administrator rights to access private information.
    - Exercise discretion, even with filesystem access, and adhere to privacy principles.
    
    **IV. Avoid Bypassing Roles**
    
    - Do not use administrator rights to bypass established roles or policies.
    - Follow proper processes for accessing information, even for legitimate business reasons.
    
    **V. Think Before You Type**
    
    - Administrator actions have greater consequences; think through tasks and avoid rushing.
    - Write out steps before execution, providing planning and documentation benefits.
    
    **VI. Documenting Actions**
    
    - Use commands like script in Linux or Start-Transcript in Windows PowerShell to record commands and their output.
    - Documentation aids in automation and serves as a reference for troubleshooting.
    
    **VII. Principle of Great Power and Responsibility**
    
    - Acknowledge the impact of administrator rights; errors can have significant consequences.
    - Minimize risk by ensuring quick reversibility of changes through practices like version control or documentation.
    
    **VIII. Rollback Considerations**
    
    - Before making changes, assess the ease of rollback for each action.
    - Be cautious with irreversible changes and ensure copies of critical information are maintained.
    
      
    
      
    
    Production
    
    **I. Definition of Production**
    
    - In an infrastructure context, production refers to the parts of the infrastructure where a service is executed and serves users.
    
    **II. Making Important Changes**
    
    - Important changes, such as adding a new service, changing configurations, or updating the operating system, should always be tested in a dedicated test environment.
    - The test environment mirrors the production configuration but doesn't serve actual users, allowing for troubleshooting without impacting users.
    
    **III. Secondary or Stand-By Machines**
    
    - For critical services, maintain a secondary or stand-by machine identical to the production machine.
    - Test changes in the secondary environment first, enabling them on the stand-by machine before applying them to the primary machine.
    
    **IV. Canaries for Larger Services**
    
    - Larger services may use canaries—small groups of servers—to detect potential issues before deploying changes to the entire fleet.
    - Verify changes on canaries and then proceed with deployment to the rest of the servers.
    
    **V. Minor Changes in Production**
    
    - Even for minor changes, always test in a dedicated test infrastructure before applying them to the production environment.
    - The significance of the change determines whether primary and secondary machines or canaries are necessary.
    
    **VI. Importance of Testing Instances**
    
    - Regardless of service size, never make changes directly in production.
    - Always use a test instance to verify changes before deployment to the production environment.
    
      
    
      
    

**Assessing Risk and Prioritizing Changes in IT Operations**

**I. Testing and Documentation**

- Always test changes before deployment and document procedures for easy reversal.
- The level of effort invested in testing and documentation depends on the risk involved in the change.

**II. Risk Assessment**

- Evaluate the importance of a service to the infrastructure.
- Consider the number of users impacted if the service goes down.

**III. Mission-Critical Services**

- Certain services are mission-critical (e.g., centralized authentication, billing system, backups).
- Downtime in mission-critical services can have severe consequences for the organization.

**IV. Non-Mission-Critical Services**

- Not all services are mission-critical (e.g., informational website, internal ticket system).
- The impact of downtime for non-mission-critical services is comparatively lower.

**V. User Reach and Importance**

- The more users a service reaches, the more effort is needed to avoid disruptive changes.
- The importance of services to the company's operations determines the level of effort to keep the service up.

**VI. Agreed Service Availability**

- User agreements may specify expectations for service availability.
- Disruptive maintenance, if agreed upon, can be performed during designated times (e.g., weekends).

**VII. Prioritizing Problem Resolution**

- Use criteria such as the impact on users' ability to work to establish priorities for problem resolution.
- Higher priority should be given to issues hindering essential functions over minor annoyances.

  

  

**Problem Resolution in IT Support**

**I. Identifying and Fixing Problems**

- IT support specialists often handle problems in various systems, from user computers to servers and cloud-based code.
- When dealing with a problem, ensure you can recreate the error to test solutions effectively.

**II. Reproduction Case**

- Develop a reproduction case to recreate the steps leading to the unexpected outcome.
- Answer three key questions: What steps led to this point? What's the unexpected result? What's the expected result?

**III. Example: Website Access Issue**

- Example scenario: User can't access a website page.
- Reproduction case: Navigate to the failing site with a web browser.
- Bad result: Error page.
- Expected result: Visible website.

**IV. Fixing the Underlying Issue**

- Work on fixing the underlying issue in a test instance, not in production.
- Document all steps and findings during the process for future reference.

**V. Verification of Solution**

- After applying the fix, retrace the steps to the bad experience.
- Verify the solution's effectiveness by ensuring the expected experience now takes place.

**VI. Documentation Importance**

- Comprehensive documentation proves invaluable for dealing with similar issues in the future.
- Documenting steps and findings aids in troubleshooting and knowledge sharing.

  

  

  

**I. Introduction to IT Infrastructure Components**

- Physical infrastructure involves setting up servers on-site or at another location.
- Servers end-to-end management includes hardware tasks and operating system updates.

**II. Cloud Alternative: Infrastructure as a Service (IaaS)**

- IaaS provides pre-configured virtual machines for server-like functionalities.
- Popular IaaS providers: Amazon Web Services (EC2), Linode, Windows Azure, Google Compute Engine.

**III. Networking in IT Infrastructure**

- Company's internal network setup involves IP addressing, DHCP, wireless internet, DNS, etc.
- Networking as a Service (NaaS) allows offshoring networking services, avoiding expensive hardware and network security management.

**IV. Software as a Service (SaaS)**

- SaaS offers cloud alternatives for software needs (e.g., Microsoft Office 365, Google G Suite).
- Eliminates the need for installing and maintaining software on individual machines.

**V. Platform as a Service (PaaS)**

- PaaS provides an all-in-one solution for building and deploying web applications.
- Platforms like Heroku, Windows Azure, and Google App Engine streamline development, database storage, and web serving.

**VI. Directory Services for User Management**

- Directory services centralize user and computer management (e.g., Windows Active Directory, OpenLDAP).
- Directory as a Service (DaaS) offers cloud deployment options.

**VII. Importance of Understanding Services**

- While cloud services offer convenience, understanding how a service works is crucial.
- Pros of cloud services include flexibility; cons involve recurring costs and dependence on the provider's service.

  

  

**Choosing a Server Operating System**

**I. Introduction to Server Setup**

- Setting up a server involves installing a service or application on the server.
- Services provided by the server are then accessible to requesting machines.

**II. Server Operating System vs. Desktop Operating System**

- While services can be installed on desktop operating systems (e.g., Windows 10), server installations are more common in organizations.
- Server operating systems are optimized for server functionality, allowing more network connections and increased RAM capacity.

**III. Server Operating Systems Examples**

- Windows Server: A server operating system by Microsoft designed for server use.
- Linux Distributions: Many Linux distributions have server counterparts (e.g., Ubuntu Server) optimized for server functionality.

**IV. Optimizations for Server Use**

- Server operating systems are tailored for increased security and efficiency.
- Built-in services: Server operating systems often come with additional services pre-installed, reducing the need for separate setups.

  

  

**The Role of Virtualization in Infrastructure Services**

**I. Introduction to Virtualization**

- Virtualization involves running services on either dedicated hardware or virtualized instances on a server.
- Each virtual instance represents a service, providing flexibility and efficiency.

**II. Pros and Cons of Dedicated Hardware vs. Virtualized Instances**

**A. Performance:**

- Dedicated hardware offers better performance for a single service due to exclusive resource usage.
- Virtualized instances share resources, affecting performance but enabling cost-effective multiple services on one physical server.

**B. Cost:**

- Dedicated hardware can be expensive, especially when each service requires a separate piece of hardware.
- Virtualization allows multiple services (virtual instances) to run on one physical server, reducing costs.

**C. Resource Utilization:**

- In a typical server, a single service may use only 10 to 20% of CPU utilization.
- Virtualization enables efficient use of hardware resources by accommodating multiple services on one machine.

**D. Maintenance:**

- Hardware maintenance and routine OS updates are necessary for services on dedicated hardware.
- Virtualized services can be quickly stopped or migrated to another server for maintenance, simplifying the process.

**E. Points of Failure:**

- Dedicated hardware poses a risk of service disruption if the machine encounters issues.
- Virtualized servers allow easy migration of services to a backup machine, preventing a single point of failure.

**F. Redundancy:**

- To prevent a single point of failure in physical machines, redundancy involves setting up duplicate servers as backups.

  

  

- Remote access is crucial for systems administrators to troubleshoot issues and perform maintenance from any location.
- OpenSSH is a popular tool for remote access in Linux.

  

  

**Exploring File Transfer Services in Network Infrastructure**

**I. Introduction**

- In organizational settings, dedicated file transfer services streamline data sharing between computers.
- Options include traditional FTP, secure SFTP, and simpler TFTP.

**II. File Transfer Protocol (FTP)**

**A. Overview:**

1. Legacy method for transferring files over the Internet.
2. Lacks data encryption, making it less secure.
3. Commonly used for sharing web content.

**B. FTP Process:**

1. Clients install FTP clients.
2. Server software shares information in a designated directory.

**III. Secure File Transfer Protocol (SFTP)**

**A. Characteristics:**

1. Secure version of FTP.
2. Data is sent through SSH, ensuring encryption.
3. Recommended for secure data transfers.

**IV. Trivial FTP (TFTP)**

**A. Overview:**

1. Simpler file transfer method than FTP.
2. Doesn't require user authentication.
3. Ideal for hosting generic files, such as installation files.

**B. PXE (Pixie Boot):**

1. Pre Boot Execution for network booting.
2. Enables launching software available over the network.
3. Commonly used for automatic installation of operating systems.

  

**Synchronizing Time with NTP in Network Infrastructure**

**I. Introduction**

- Network Time Protocol (NTP) is a vital internet protocol for maintaining synchronized clocks on networked machines.
- NTP ensures accurate timekeeping, crucial for various security services and network authentication protocols.

**II. Importance of NTP**

**A. Real-world Example:**

1. Airports use synchronized clocks powered by NTP to display accurate departure and arrival information.
2. Consistent time is essential for air traffic controllers coordinating flights.

**B. IT World Significance:**

1. Security services like Kerberos rely on consistent time across the network.
2. NTP is crucial for maintaining accuracy across an organization's fleet of machines.

**III. Setting Up NTP**

**A. Local NTP Server:**

1. Install NTP server software on a managed server.
2. Install NTP clients on machines, specifying the local NTP server for time synchronization.
3. Allows end-to-end management of the synchronization process.

**B. Public NTP Server:**

1. Connect client machines to public NTP servers managed by external organizations.
2. Efficient for smaller setups but may lack control over synchronization.
3. Larger fleets benefit from running a dedicated NTP server for better control.

**C. Best Practices:**

1. Run a local NTP server pointing to a public NTP server for optimal control and synchronization.
2. Avoid connecting all clients directly to a public NTP server.

  

  

DNS

1. **Domain Name:**
    - Purchase from a registrar (e.g., Godaddy.com).
    - Example: SettingUpDNSIsFun.example.com.
2. **Hosting:**
    - Choose cloud or self-hosting (weigh pros and cons).
3. **Web Server:**
    - Set up server to store and serve content.
4. **DNS Settings:**
    - Get settings from registrar or set up authoritative DNS server.
5. **Point Domain:**
    - Associate domain with web content's IP address.
6. **Considerations:**
    
    - Evaluate self-hosting vs. cloud based on cost, control, and maintenance.
    
      
    

Setting up DNS for internal mapping:

1. **Local Host File:**
    - In Linux: /etc/hosts for manual IP to host mappings.
    - Example: Change local host to [**www.google.com**](http://www.google.com/) in the host file.
2. **Limitations of Host File:**
    - Not scalable for multiple computers.
    - Manual entry required for each machine.
3. **Local DNS Server:**
    - Set up a central DNS server for organization's computers.
    - Change network settings to use this DNS server.
4. **Benefits of Local DNS Server:**
    - Centralized storage for computer names and IP addresses.
    - More scalable than host files for large fleets.
5. **Directory Service Integration:**
    
    - Integrate DNS with directory service (e.g., Active Directory, LDAP).
    - Automatic population of mappings, eliminating manual entries.
    
      
    

Managing IP addresses in IT support:

1. **Static vs. DHCP:**
    - Static: Manual IP assignment, requires tracking.
    - DHCP: Dynamic, leases IPs from a DHCP server automatically.
2. **Advantages of DHCP:**
    - Automatic IP assignment.
    - Easy scalability without manual changes.
    - Simplifies network management.
3. **Configuring DHCP:**
    - Define IP range, local DNS server, gateway, subnet mask.
    - DHCP server software varies (e.g., Windows Server).
4. **Integration with DNS:**
    - Specify DNS server locations in DHCP settings.
    - DNS and DHCP sync, updating IP mappings automatically.
5. **Popular DHCP Servers:**
    - Windows Server (built-in).
    - Various third-party options available.
6. **Importance of DHCP and DNS:**
    
    - Critical network services for streamlined infrastructure management.
    
      
    
      
    

Understanding services in IT support:

1. **Background Processes:**
    - Also known as daemons or services.
    - Run without user interaction, managed by the operating system.
2. **Configuration Files:**
    - Each service has configuration files for behavior settings.
    - Some may offer interactive interfaces; others rely on system infrastructure.
3. **Management Tasks:**
    - Start and stop services.
    - Inspect logs for current or past activities.
4. **Boot Configuration:**
    - Services usually configured to start on machine boot.
    - Can be adjusted to start manually if desired.
5. **Fault Tolerance:**
    - Services configured to restart if they crash unexpectedly.
    - System configuration may need adjustment for these properties.
6. **Uniform Concepts:**
    
    - While specific services may vary, general concepts of managing and configuring services apply universally.
    
      
    
      
    

In Linux, managing services like NTP involves key commands:

1. **Check Service Status:**
    - Use `**sudo service ntp status**` to confirm NTP daemon's status.
    - Example: NTP service is running.
2. **Automatic Clock Adjustments:**
    - NTP daemon continuously adjusts time in small increments (0.5 milliseconds per second) for clock synchronization.
    - Prevents sudden time adjustments to avoid impacting other services.
3. **Threshold for Adjustments:**
    - If drift is within 128 milliseconds, NTP adjusts the clock.
    - Beyond the threshold, no interference.
4. **Manual Date Modification Test:**
    - Set date to January 1st, 2017; observe no immediate adjustment.
5. **Restarting the NTP Service:**
    - Use `**sudo service ntp stop**` to halt the service.
    - Set date to the past; observe.
    - Use `**sudo service ntp start**` to start the service and adjust the clock.
6. **Alternative: Restart Action:**
    
    - `**sudo service ntp restart**` combines stop and start actions.
    - Efficient for quick restarts.
    
      
    

You can use the Services GUI in Windows.