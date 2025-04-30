
### 1️⃣ **What is DNS and why is it important?**

**DNS (Domain Name System)** is like the **phonebook of the internet**.  
When you type a website address like `www.google.com`, your computer needs the **IP address** (like `142.250.64.78`) to connect to the server. DNS translates domain names to IP addresses.

Why is it important?

Without DNS, you’d have to remember IP addresses for every website (like calling a phone number instead of a contact name).

Ensures websites, emails, and services work correctly.

Example:
When you type youtube.com, DNS translates it to an IP like 208.65.153.238 so your browser can load the site.

🧠 **Easy to remember**:  
Just like you don’t remember your friend’s phone number but use their name in your contacts, your browser uses DNS to find websites by name.

📌 **Real Example**:  
You type `www.amazon.com`, DNS finds its IP, and connects your browser to the Amazon server.

---

### 2️⃣ **Types of DNS Records**

DNS records are like instructions telling servers how to handle requests:

A Record: Maps a domain to an IPv4 address (e.g., example.com → 192.0.2.1).

AAAA Record: Maps to IPv6 (e.g., example.com → 2001:db8::1).

CNAME Record: Alias of one domain to another (e.g., www.example.com → example.com).

MX Record: Directs emails to mail servers (e.g., gmail.com → mail.google.com).

TXT Record: Stores text (used for verification, SPF, DKIM).

NS Record: Specifies authoritative DNS servers for the domain.

Here are the **common DNS record types**:

| Type | Full Form | Purpose | Example |
|------|-----------|---------|---------|
| **A** | Address Record | Maps domain to IPv4 address | `google.com → 142.250.64.78` |
| **AAAA** | Quad A | Maps domain to IPv6 address | `example.com → 2001:db8::1` |
| **CNAME** | Canonical Name | Alias to another domain | `blog.example.com → example.com` |
| **MX** | Mail Exchange | Directs emails to the right mail server | `gmail.com → mail.gmail.com` |
| **NS** | Name Server | Identifies DNS servers for a domain | `example.com → ns1.dns.com` |
| **TXT** | Text | Stores any text, used for SPF, DKIM (email security) | Verification |
| **PTR** | Pointer | Used in reverse DNS lookups (IP → domain) | `192.0.2.1 → example.com` |
| **SRV** | Service Locator | Defines location of services (VoIP, etc.) | `_sip._tcp.example.com` |

---

### 3️⃣ **What is the difference between Recursive & Iterative DNS Queries?**

- **Recursive Query**: DNS server takes **full responsibility** to get the answer and responds with the final IP.
- **Iterative Query**: DNS server **returns a referral** to another DNS server and leaves the next query up to the client.

📌 **Example**:
- Recursive: You ask a friend to find an address. They go and return with the answer.
- Iterative: Your friend gives you the next person to ask.


Recursive Query: Your DNS resolver (like your ISP) does all the work to find the IP. If it doesn’t know, it asks other servers until it gets an answer.

Example: Asking a librarian to find a book—they search until they get it for you.

Iterative Query: The DNS server gives the best answer it has or directs you to another server.

Example: The librarian says, "I don’t know, but try the science section."

---

### 4️⃣ **What are Primary, Secondary & Stub Zones?**

Primary Zone: The main DNS database where records are created/edited (e.g., your company’s main DNS server).

Secondary Zone: A read-only copy for backup/load balancing (syncs from Primary).

Stub Zone: Only keeps track of authoritative DNS servers for a domain (lightweight version).

Example:
A company’s Primary Zone is in HQ, while Secondary Zones exist in branch offices for redundancy.


| Type | Meaning | Example |
|------|---------|---------|
| **Primary Zone** | Main zone file where DNS records are stored and edited | `example.com` on main DNS server |
| **Secondary Zone** | Read-only copy of Primary zone (for backup/load balancing) | Backup of `example.com` |
| **Stub Zone** | Only has Name Server (NS) records of another zone (used for referrals) | Only knows who is responsible for `abc.com` |

🧠 Think of:
- Primary = Master copy  
- Secondary = Backup copy  
- Stub = Shortcut to reach another DNS server

---

### 5️⃣ **What is Forward & Reverse DNS Lookup?**

Forward Lookup: Converts domain → IP (e.g., facebook.com → 69.63.176.13).

Reverse Lookup: Converts IP → domain (e.g., 69.63.176.13 → facebook.com).

Example:
Forward: Typing a website name → loads the site.
Reverse: Security tools check if an IP matches a known domain


- **Forward Lookup**: Domain → IP address  
  👉 `google.com → 142.250.64.78`
- **Reverse Lookup**: IP address → Domain  
  👉 `142.250.64.78 → google.com`

📌 Reverse lookup uses **PTR records**.

---

### 6️⃣ **What is a DNS Resolver?**

A DNS resolver is like a detective that finds the IP for a domain. Your ISP or public DNS (like Google’s 8.8.8.8) acts as a resolver.

Example:
When you visit netflix.com, your resolver asks root servers, TLD servers (.com), and finally Netflix’s DNS to get the IP.

A **DNS Resolver** is a server that receives your DNS request and finds the IP address of the domain you typed.

🧠 Think of it as your **DNS helper** that asks other DNS servers and gets you the final answer.

📌 Example: When you use a public DNS like `8.8.8.8` (Google DNS), that's a DNS resolver.

---

### 7️⃣ **What is TTL (Time to Live) in DNS?**

**TTL** tells DNS servers how long to cache (store) a DNS record before asking again.

- If TTL = 3600 seconds → the record is cached for **1 hour**
- Lower TTL = records update faster  
- Higher TTL = better performance (less DNS traffic)

📌 Example:
Changing your website’s IP? Set a **low TTL temporarily**, so changes reflect quickly.

---

### 8️⃣ **What is Split-Horizon DNS?**

**Split-Horizon DNS** means **serving different DNS results based on who is asking**.

📌 Example:
- Internal employees → `intranet.example.com → 192.168.1.10`
- External users → `intranet.example.com → 203.0.113.5`

🧠 Used for security and network management.

---

### 9️⃣ **How do you flush the DNS cache on a Windows system?**

Open **Command Prompt as Administrator** and run:

```bash
ipconfig /flushdns
```

🧠 This clears the cached DNS entries to force a fresh lookup.

---

### 🔟 **How do you test DNS resolution?**

Use these commands:

- `nslookup google.com`  
- `ping google.com`  
- `dig google.com` (on Linux or using WSL)  
- `tracert google.com`

📌 **Example**:  
If `nslookup example.com` fails, DNS isn't resolving.

---

### 1️⃣1️⃣ **How to troubleshoot DNS issues?**

✅ **Step-by-step**:
1. Check DNS settings: `ipconfig /all`
2. Flush cache: `ipconfig /flushdns`
3. Try alternate DNS: `8.8.8.8` or `1.1.1.1`
4. Use `nslookup` or `dig` to verify
5. Restart network or router
6. Check `/etc/resolv.conf` (Linux) or adapter settings (Windows)

📌 Use browser’s Incognito or another network to test.

---

### 1️⃣2️⃣ **What is DNS Poisoning & How to Prevent It?**

**DNS Poisoning (Cache Poisoning)**:  
Fake DNS data is inserted into cache, leading users to malicious sites.

📌 **Example**:  
You type `facebook.com`, poisoned DNS redirects you to a phishing site.

🛡️ **Prevention**:
- Use **DNSSEC**
- Clear DNS cache regularly
- Keep OS/DNS server software updated
- Avoid unsecured public networks

---

### 1️⃣3️⃣ **What is DNSSEC?**

**DNSSEC (DNS Security Extensions)** adds **security and authenticity** to DNS.

✅ It uses **digital signatures** to ensure DNS data is not tampered with.

📌 Example:
With DNSSEC, even if someone tries DNS poisoning, your system will detect invalid signatures.

---

### 1️⃣4️⃣ **How to change the DNS server on a Windows machine?**

✅ Steps:
1. Open **Control Panel → Network and Sharing Center**
2. Click **Change Adapter Settings**
3. Right-click on your **active network → Properties**
4. Select **Internet Protocol Version 4 (TCP/IPv4) → Properties**
5. Choose **Use the following DNS server addresses**
   - Preferred: `8.8.8.8`
   - Alternate: `1.1.1.1`
6. Click OK and restart browser.

🧠 Use Google DNS (8.8.8.8) or Cloudflare DNS (1.1.1.1) for speed and security.

---

Would you like me to create a PDF or a printable summary sheet with these DNS concepts for easy revision?
