# 🎫 Ticket #001 — No Internet Connection

## 📋 Ticket Information

| Field             | Details                   |
| ----------------- | ------------------------- |
| **Ticket Number** | 001                       |
| **Priority**      | Normal                    |
| **Department**    | IT Support                |
| **User**          | Jordan Williams           |
| **Device**        | Windows 11 Workstation    |
| **Issue**         | Unable to access websites |

---

# 📝 User's Report

The user reported that their Windows 11 workstation was connected to Wi-Fi, but they were unable to access any websites.

The user stated that everything had been working the previous day and that other people in the office were able to access the internet.

The user had not attempted any troubleshooting before contacting the Help Desk.

A computer restart was performed approximately 10 minutes before the ticket was reported, but the issue continued.

---

# 🔧 Troubleshooting Process

## Step 1 — Checked IP Configuration

I used PowerShell and the following command:

```powershell
ipconfig /all
```

### Results

```text
IPv4 Address:    192.168.4.37
Subnet Mask:     255.255.252.0
Default Gateway: 192.168.4.1
DHCP Server:     192.168.4.1
DNS Server:      192.168.4.1
```

### 🔎 Finding

The workstation had received valid network configuration information.

The default gateway, Dynamic Host Configuration Protocol (DHCP) server, and Domain Name System (DNS) server were all using `192.168.4.1`.

I initially questioned whether the gateway, DHCP server, and DNS server should have different addresses. I learned that this is normal when a router provides multiple network services.

---

## Step 2 — Tested the Default Gateway

I used:

```powershell
ping 192.168.4.1
```

### Results

* **Packets sent:** 4
* **Packets received:** 4
* **Packets lost:** 0
* **Response time:** Approximately 2–3 milliseconds

### 🔎 Finding

The workstation could successfully communicate with the local network and its default gateway.

This indicated that the problem was probably not a basic Wi-Fi or local network connectivity issue.

---

## Step 3 — Tested DNS Resolution

I used:

```powershell
nslookup google.com
```

### Results

```text
Server:  router.local
Address: 192.168.4.1

Non-authoritative answer:
Name:    google.com
Address: 142.250.72.14
```

### 🔎 Finding

Domain Name System (DNS) resolution was working.

The computer was able to translate the domain name `google.com` into an IP address.

---

## Step 4 — Tested External Internet Connectivity

I used the IP address returned by the DNS lookup:

```powershell
ping 142.250.72.14
```

### Results

* **Packets sent:** 4
* **Packets received:** 4
* **Packets lost:** 0
* **Response time:** Approximately 22–25 milliseconds

### 🔎 Finding

The workstation was able to communicate with an external IP address on the internet.

This further indicated that the computer had internet connectivity even though websites were not loading in the affected browser.

---

## Step 5 — Tested HTTPS Connectivity

I remembered that HTTPS commonly uses Transmission Control Protocol (TCP) port 443.

I used PowerShell to test the connection:

```powershell
Test-NetConnection google.com -Port 443
```

### Results

```text
ComputerName     : google.com
RemoteAddress    : 142.250.72.14
RemotePort       : 443
InterfaceAlias   : Wi-Fi
SourceAddress    : 192.168.4.37
TcpTestSucceeded : True
```

### 🔎 Finding

The workstation was able to establish a Transmission Control Protocol (TCP) connection to port 443.

This showed that the computer could establish the type of connection normally used for HTTPS web traffic.

---

# 🌐 Browser Troubleshooting

## Step 6 — Tested Another Browser

I tested another web browser on the same workstation.

### Result

The other browser was able to access websites successfully.

### 🔎 Finding

The internet connection itself appeared to be working.

The problem appeared to be specific to Microsoft Edge.

---

## Step 7 — Tested Microsoft Edge InPrivate Mode

I opened an InPrivate window in Microsoft Edge.

### Keyboard Shortcut

```text
Ctrl + Shift + N
```

I then attempted to access Google.

### Result

Google loaded successfully in the InPrivate window.

### 🔎 Finding

Microsoft Edge was capable of accessing the internet, but something affecting the normal Edge browsing session was preventing websites from loading.

---

## Step 8 — Checked Browser Extensions

I checked the installed Edge extensions.

The only extension installed was:

> **Google Docs Offline**

I temporarily disabled the extension and tested Edge again.

### Result

Google still did not load normally.

### 🔎 Finding

The browser extension was not the cause of the problem.

---

## Step 9 — Cleared Browser Cache

I cleared the Microsoft Edge browser cache and tested the browser again.

### Result

Google still did not load normally.

### 🔎 Finding

Cached browser data was not the cause of the issue.

---

# 🛡️ Proxy Troubleshooting

## Step 10 — Checked Proxy Configuration

Because normal Edge was not working while InPrivate mode was working, I continued looking for settings that could affect the browser's internet connection.

I checked the Windows proxy settings.

### Proxy Setting

```text
Use a proxy server: ON
```

A proxy server can act as a middleman between a computer and the internet.

The expected traffic path was:

```text
Computer
    ↓
Proxy Server
    ↓
Internet
```

A problem with the proxy configuration could prevent the browser from reaching websites.

---

## Step 11 — Disabled the Proxy

I temporarily disabled the manually configured proxy.

I then:

1. Closed Microsoft Edge.
2. Reopened Microsoft Edge.
3. Navigated to Google.
4. Tested additional websites.

### ✅ Result

Google loaded successfully.

Other websites also loaded successfully.

### 🔎 Finding

Disabling the proxy resolved the problem.

---

# 🎯 Root Cause

The workstation had a manually configured proxy enabled.

The proxy configuration was preventing Microsoft Edge from accessing websites normally.

---

# 🛠️ Resolution

The manually configured proxy was disabled.

---

# ✅ Verification

After disabling the proxy, I tested multiple websites in Microsoft Edge.

The websites loaded successfully.

The user was able to access the internet normally.

### **Status: RESOLVED ✅**

---

# 🧠 Troubleshooting Logic

The troubleshooting process followed a systematic approach instead of immediately assuming that the internet connection was down.

```text
User reports no internet
        ↓
Restart computer
        ↓
Check IP configuration
        ↓
Test default gateway
        ↓
Test DNS resolution
        ↓
Test external IP connectivity
        ↓
Test TCP port 443
        ↓
Test another browser
        ↓
Test Edge InPrivate
        ↓
Check extensions
        ↓
Clear browser cache
        ↓
Check proxy settings
        ↓
Disable proxy
        ↓
Test websites
        ↓
Issue resolved
```

---

# 📚 What I Learned

This ticket taught me how to troubleshoot an internet connectivity problem systematically instead of assuming the internet connection itself was the problem.

I learned how to use `ipconfig /all` to examine a computer's network configuration and how to use `ping` to test connectivity to the default gateway and an external IP address.

I also learned how `nslookup` can be used to determine whether Domain Name System (DNS) resolution is working.

Using `Test-NetConnection` helped me test whether a Transmission Control Protocol (TCP) connection could be established to port 443.

Another important lesson was learning how to isolate a problem to a specific application. Testing another browser and Microsoft Edge InPrivate mode helped show that the computer's overall internet connection was working.

I also learned what browser extensions, cached browser data, Virtual Private Networks (VPNs), and proxy servers are and how they can potentially affect connectivity.

Most importantly, I learned that help desk troubleshooting should be based on testing and evidence rather than assumptions. Each test helped eliminate possible causes and narrow the problem down until the proxy configuration was identified as the root cause.

---

# 💻 Skills Practiced

### Windows & PowerShell

* Windows 11 troubleshooting
* PowerShell
* `ipconfig /all`
* `ping`
* `nslookup`
* `Test-NetConnection`

### Networking

* IP addressing
* Default gateways
* Domain Name System (DNS)
* Dynamic Host Configuration Protocol (DHCP)
* Transmission Control Protocol (TCP)
* TCP port 443
* Network troubleshooting

### Browser Troubleshooting

* Microsoft Edge
* InPrivate browsing
* Browser extensions
* Browser cache
* Proxy configuration

### Help Desk Skills

* Root cause analysis
* Systematic troubleshooting
* Technical documentation
* Help Desk ticket management

---

# 🎓 Key Takeaway

This ticket demonstrated that a computer can have a working internet connection while a specific application is unable to access websites.

By testing the connection layer by layer and using the results from each test, I was able to narrow the problem down from a possible network issue to a browser-specific configuration problem and ultimately identify the manually configured proxy as the root cause.

**Ticket #001 — Resolved ✅**
