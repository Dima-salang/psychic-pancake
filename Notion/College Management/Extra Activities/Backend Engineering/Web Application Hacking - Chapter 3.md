I'll create a comprehensive, in-depth lecture on **Chapter 3: Web Application Technologies** from The Web Application Hacker's Handbook. This chapter provides the essential technical foundation you need to understand how web applications work before you can effectively attack them.

---

# 📚 LECTURE: Web Application Technologies
## Chapter 3 — The Web Application Hacker's Handbook (2nd Edition)

---

## 🎯 LEARNING OBJECTIVES

By the end of this lecture, you will understand:
1. The HTTP protocol in depth—requests, responses, methods, and headers
2. How state is maintained in a stateless protocol (cookies, sessions)
3. Encoding schemes and why they matter for security
4. Server-side vs. client-side functionality
5. How to analyze and manipulate web traffic

> **Principle:** You cannot hack what you do not understand. This chapter builds your technical vocabulary.

---

## 🔷 PART 1: THE HTTP PROTOCOL (Pages 39-50)

### 1.1 What is HTTP?

**HTTP (HyperText Transfer Protocol)** is the foundation of web communication. It's a **text-based, request-response protocol** that operates over TCP/IP (typically port 80 for HTTP, 443 for HTTPS).

**Key Characteristics:**

| Characteristic | Implication for Security |
|---------------|------------------------|
| **Stateless** | Each request is independent; sessions must be artificially maintained |
| **Text-based** | Easy to read and modify (good for attackers) |
| **Connectionless** | Server doesn't remember previous requests |
| **Extensible** | Headers allow arbitrary data to be passed |

### 1.2 HTTP Requests (Page 40)

**Anatomy of an HTTP Request:**

```
┌─────────────────────────────────────────────────────────────┐
│  REQUEST LINE (Required)                                     │
│  GET /search?q=test HTTP/1.1                                │
│  └──┘ └─────────────────┘ └──────┘                          │
│   │         │                │                              │
│  Method   Path/Resource    Protocol Version                 │
│           + Query String                                    │
├─────────────────────────────────────────────────────────────┤
│  HEADERS (Context information)                               │
│  Host: www.example.com                                      │
│  User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)      │
│  Accept: text/html,application/xhtml+xml                    │
│  Accept-Language: en-US,en;q=0.9                            │
│  Cookie: sessionid=abc123; preferences=darkmode             │
│  Content-Type: application/x-www-form-urlencoded            │
│  Content-Length: 23                                         │
├─────────────────────────────────────────────────────────────┤
│  EMPTY LINE (Marks end of headers)                          │
├─────────────────────────────────────────────────────────────┤
│  BODY (Optional - for POST/PUT requests)                     │
│  username=admin&password=secret                             │
└─────────────────────────────────────────────────────────────┘
```

**Practical Exercise: Viewing HTTP Requests**

Using **Burp Suite** or browser DevTools (F12 → Network tab):

1. Open any website
2. Open DevTools → Network tab
3. Refresh the page
4. Click any request to see the full HTTP request

---

### 1.3 HTTP Responses (Page 41)

**Anatomy of an HTTP Response:**

```
┌─────────────────────────────────────────────────────────────┐
│  STATUS LINE (Required)                                      │
│  HTTP/1.1 200 OK                                            │
│  └──────┘ └──┘ └─┘                                          │
│    │       │    │                                           │
│  Protocol  Code  Reason Phrase                              │
│  Version                                                    │
├─────────────────────────────────────────────────────────────┤
│  HEADERS (Metadata about response)                           │
│  Date: Mon, 15 Jan 2024 12:00:00 GMT                        │
│  Server: Apache/2.4.41 (Ubuntu)                             │
│  Content-Type: text/html; charset=UTF-8                     │
│  Content-Length: 1234                                       │
│  Set-Cookie: sessionid=xyz789; HttpOnly; Secure             │
│  Cache-Control: no-cache, no-store                          │
├─────────────────────────────────────────────────────────────┤
│  EMPTY LINE                                                 │
├─────────────────────────────────────────────────────────────┤
│  BODY (The actual content)                                   │
│  <!DOCTYPE html>                                             │
│  <html>...                                                  │
└─────────────────────────────────────────────────────────────┘
```

### 1.4 HTTP Methods (Page 42)

| Method | Purpose | Safe? | Idempotent? | Security Notes |
|--------|---------|-------|-------------|----------------|
| **GET** | Retrieve resource | ✅ Yes | ✅ Yes | Parameters in URL (visible in history/logs) |
| **POST** | Submit data, create resource | ❌ No | ❌ No | Data in body, can change server state |
| **PUT** | Update/replace resource | ❌ No | ✅ Yes | Often restricted, can overwrite data |
| **DELETE** | Remove resource | ❌ No | ✅ Yes | Dangerous if improperly protected |
| **HEAD** | Get headers only | ✅ Yes | ✅ Yes | Useful for reconnaissance |
| **OPTIONS** | Get supported methods | ✅ Yes | ✅ Yes | Reveals server capabilities |
| **PATCH** | Partial modification | ❌ No | ❌ No | Less common, implementation varies |

> **Safe** = Doesn't modify server state  
> **Idempotent** = Multiple identical requests have same effect as one

**Practical Attack Scenario:**

```
Attacker discovers admin panel at /admin/delete-user

GET /admin/delete-user?id=5    ← Might fail (GET is "safe")
POST /admin/delete-user        ← Might succeed if access control fails
DELETE /users/5                ← Might succeed if REST API exposed
```

---

### 1.5 URLs (Uniform Resource Locators) (Page 44)

**URL Structure:**

```
https://admin.example.com:8443/app/users?id=123#profile
└─┬─┘   └────┬────────┘    └┬┘ └──┬─┘ └─┬──┘ └──┬───┘
  │          │              │     │     │       │
Protocol   Host/Domain    Port   Path  Query  Fragment
           (subdomain)                    String  (client-side only)
```

**URL Encoding (Percent-Encoding):**

Special characters are encoded as `%` followed by hex value:

| Character | Encoded | When Needed |
|-----------|---------|-------------|
| Space | `%20` or `+` | In URLs, form data |
| `&` | `%26` | When `&` is data, not delimiter |
| `=` | `%3D` | When `=` is data, not delimiter |
| `?` | `%3F` | When `?` is data, not query start |
| `/` | `%2F` | When `/` is data, not path separator |

**Double-Encoding Attack:**

```
Original:    ../../../etc/passwd
Encoded:     %2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
Double:      %252e%252e%252f%252e%252e%252f...

Attack: Application decodes once → still encoded
        Second decode (or misconfiguration) → dangerous path
```

---

### 1.6 REST Architecture (Page 44)

**REST (Representational State Transfer)** is an architectural style where:

- Resources are identified by **URLs**
- Operations performed via **HTTP methods**
- State transferred in **representations** (JSON, XML, HTML)

**REST vs Traditional:**

| Aspect | Traditional Web | REST API |
|--------|----------------|----------|
| URL pattern | `/getUser.php?id=5` | `/users/5` |
| Action | Determined by parameter | Determined by HTTP method |
| Methods | Mostly GET/POST | GET, POST, PUT, DELETE, PATCH |
| Data format | HTML | JSON, XML |

**REST Security Implications:**

```
Traditional:  POST /transfer-money
              body: from=123&to=456&amount=1000

REST:         POST /transactions
              body: {"from": "123", "to": "456", "amount": 1000}
              
              Or worse:
              PUT /accounts/123/balance
              body: {"amount": 999999}
```

---

### 1.7 HTTP Headers (Page 45)

**Request Headers (Client → Server):**

| Header | Purpose | Security Relevance |
|--------|---------|-------------------|
| `Host` | Specifies target domain | Virtual hosting, cache poisoning |
| `User-Agent` | Client software identification | Fingerprinting, sometimes access control |
| `Accept` | Preferred content types | Content negotiation attacks |
| `Accept-Language` | Preferred language | Locale-based attacks |
| `Cookie` | Session/state data | Primary attack target |
| `Authorization` | Authentication credentials | Bearer tokens, Basic auth |
| `Referer` | Previous page URL | CSRF protection, information leakage |
| `X-Forwarded-For` | Original client IP | IP spoofing, bypassing IP restrictions |
| `Content-Type` | Body format | Content-type confusion attacks |

**Response Headers (Server → Client):**

| Header | Purpose | Security Relevance |
|--------|---------|-------------------|
| `Set-Cookie` | Establish session | Secure, HttpOnly, SameSite flags |
| `Cache-Control` | Caching behavior | Sensitive data caching |
| `Content-Security-Policy` | XSS mitigation | Restricts resource loading |
| `X-Frame-Options` | Clickjacking protection | `DENY`, `SAMEORIGIN` |
| `Strict-Transport-Security` | HTTPS enforcement | HSTS preload |
| `X-Content-Type-Options` | MIME sniffing protection | `nosniff` prevents type confusion |
| `Server` | Server software info | Information disclosure |

**Cookie Security Flags (Critical!):**

```
Set-Cookie: sessionid=abc123; Secure; HttpOnly; SameSite=Strict; Path=/; Domain=.example.com; Expires=Wed, 21 Oct 2025 07:28:00 GMT
            └────────┬──────┘  └──┬──┘  └───┬───┘  └────┬─────┘  └──┬─┘  └─────┬─────┘  └─────────────────────────────────┘
                     │            │         │           │           │          │
                  Name=Value   Secure   HttpOnly    SameSite     Path      Domain      Expires/Max-Age
                               (HTTPS   (No JS      (CSRF        (Scope    (Scope
                                only)   access)     protection)   of cookie) of cookie)
```

| Flag | Attack Prevented | If Missing |
|------|-----------------|------------|
| `Secure` | Cookie sent over HTTP (eavesdropping) | Cookie stolen on HTTP connection |
| `HttpOnly` | XSS stealing cookies via JavaScript | `document.cookie` reveals session |
| `SameSite=Strict` | CSRF attacks | Cross-site requests include cookie |
| `SameSite=Lax` | CSRF on POST requests | GET-based CSRF possible |

---

### 1.8 HTTPS (Page 49)

**HTTPS = HTTP + TLS/SSL Encryption**

**What HTTPS Protects:**
- ✅ Confidentiality (encryption prevents eavesdropping)
- ✅ Integrity (tampering detection)
- ✅ Authenticity (server identity via certificates)

**What HTTPS Does NOT Protect:**
- ❌ SQL injection
- ❌ XSS
- ❌ CSRF
- ❌ Logic flaws
- ❌ Attacks from authenticated users

**TLS Handshake (Simplified):**

```
Client                           Server
  │                                 │
  │──── ClientHello ──────────────▶│  "I support TLS 1.3, cipher suites A,B,C"
  │                                 │
  │◄─── ServerHello ───────────────│  "Let's use TLS 1.3, cipher suite B"
  │    Certificate                 │  "Here's my certificate (public key)"
  │                                 │
  │──── ClientKeyExchange ────────▶│  "Here's encrypted pre-master secret"
  │    [ChangeCipherSpec]          │
  │                                 │
  │◄─── [ChangeCipherSpec] ────────│  "Switching to encrypted communication"
  │                                 │
  │════ ENCRYPTED APPLICATION DATA ═│
```

**Certificate Validation:**

```
Browser checks:
1. Is certificate signed by trusted CA?
2. Is certificate expired or revoked?
3. Does certificate match the domain?
4. Has certificate been pinned (HPKP - deprecated)?

If any check fails → Warning page (which users often ignore)
```

---

## 🔷 PART 2: WEB FUNCTIONALITY (Pages 51-66)

### 2.1 Server-Side Functionality (Page 51)

**The Server's Job:**

```
┌─────────────────────────────────────────────────────────────┐
│                    HTTP REQUEST                              │
│  GET /products.php?category=electronics&page=2              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  WEB SERVER (Apache, Nginx, IIS)                            │
│  • Parse request                                            │
│  • Route to appropriate handler                             │
│  • May apply rewrite rules                                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  APPLICATION SERVER (PHP, Python, Java, Node.js)            │
│  • Execute server-side code                                 │
│  • Process business logic                                   │
│  • Interact with databases                                  │
│  • Generate dynamic content                                 │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  DATABASE (MySQL, PostgreSQL, MongoDB, etc.)                │
│  • Store/retrieve persistent data                           │
│  • Execute queries (potential SQL injection point)          │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  HTTP RESPONSE                                               │
│  HTML/JSON/XML content                                       │
└─────────────────────────────────────────────────────────────┘
```

**Common Server-Side Technologies:**

| Technology | File Extensions | Common Vulnerabilities |
|-----------|-----------------|----------------------|
| PHP | `.php`, `.php3`, `.phtml` | File inclusion, SQLi, RCE |
| Java/JSP | `.jsp`, `.do`, `.action` | Deserialization, XXE, SSTI |
| ASP.NET | `.aspx`, `.ashx`, `.asmx` | ViewState manipulation, RCE |
| Python | `.py`, Django/Flask routes | SSTI, pickle deserialization |
| Ruby | `.rb`, Rails routes | Mass assignment, RCE |
| Node.js | `.js`, Express routes | Prototype pollution, RCE |

**Identifying Server Technologies:**

```
HTTP/1.1 200 OK
Server: Apache/2.4.41 (Ubuntu)                    ← Server software
X-Powered-By: PHP/7.4.3                           ← Application platform
X-AspNet-Version: 4.0.30319                       ← .NET version
X-Generator: Drupal 7                             ← CMS identification
```

---

### 2.2 Client-Side Functionality (Page 57)

**The Client's Job:**

```
┌─────────────────────────────────────────────────────────────┐
│  BROWSER RECEIVES HTML                                       │
│                                                              │
│  <html>                                                      │
│    <head>                                                    │
│      <script src="app.js"></script>        ← Load JavaScript │
│      <link rel="stylesheet" href="style.css"> ← Load CSS    │
│    </head>                                                   │
│    <body>                                                    │
│      <form onsubmit="return validate()">   ← Event handlers │
│        <input type="hidden" value="100">   ← Hidden data    │
│      </form>                                                 │
│    </body>                                                   │
│  </html>                                                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  DOCUMENT OBJECT MODEL (DOM)                                │
│  • Tree structure representing page                         │
│  • JavaScript can modify dynamically                        │
│  • All client-side security is bypassable!                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  JAVASCRIPT EXECUTION                                        │
│  • Form validation (cosmetic only)                          │
│  • Dynamic content loading (AJAX/fetch)                     │
│  • DOM manipulation                                         │
│  • Client-side storage (localStorage, cookies)              │
│  • Communication with server APIs                           │
└─────────────────────────────────────────────────────────────┘
```

**JavaScript Security Reality:**

```javascript
// Client-side validation (EASILY BYPASSED)
function validate() {
    var price = document.getElementById('price').value;
    if (price < 0) {
        alert("Price cannot be negative!");
        return false;  // Prevents form submission
    }
    return true;
}

// Attacker simply:
// 1. Opens DevTools (F12)
// 2. Modifies the value directly, OR
// 3. Sends request with Burp Suite, bypassing JavaScript entirely
```

**AJAX (Asynchronous JavaScript and XML):**

```javascript
// Modern fetch API
fetch('/api/user/123', {
    method: 'GET',
    headers: {
        'Authorization': 'Bearer ' + localStorage.getItem('token')
    }
})
.then(response => response.json())
.then(data => displayUser(data));

// Security implications:
// - API endpoints exposed to direct access
// - Tokens stored in JavaScript-accessible locations
// - CORS policies may be misconfigured
```

---

### 2.3 State and Sessions (Page 66)

**The Stateless Problem:**

```
Request 1:  GET /login
            Response: "Please log in"
            
Request 2:  POST /login (credentials)
            Response: "Welcome, admin!"
            
Request 3:  GET /admin
            Server thinks: "Who is this? I've never seen them before!"
```

**Session Solutions:**

| Method | How It Works | Security |
|--------|-------------|----------|
| **Cookies** | Server sets `Set-Cookie: sessionid=abc123` | Standard, flags control security |
| **URL Parameters** | `?sessionid=abc123` | Dangerous—leaks in Referer, logs |
| **Hidden Form Fields** | `<input type="hidden" name="sess" value="abc">` | Must be included in every form |
| **HTTP Authentication** | `Authorization: Basic/Negotiate/Bearer` | No session state needed |
| **Token-based (JWT)** | `Authorization: Bearer eyJhbG...` | Self-contained, but risks if stolen |

**JWT (JSON Web Tokens) Structure:**

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.  ← Header (algorithm, type)
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.  ← Payload (claims)
SflKxwRJSMeKKF2QT4fwpMe...  ← Signature (verification)

Base64Url encoded, NOT encrypted! Anyone can read the payload.
Only signature prevents tampering (if properly implemented).
```

---

## 🔷 PART 3: ENCODING SCHEMES (Pages 66-69)

Encoding transforms data for safe transmission or storage. **Understanding encoding is crucial for bypassing filters.**

### 3.1 URL Encoding (Percent-Encoding) (Page 67)

**Purpose:** Encode special characters for safe URL transmission

```
Original:  hello world & goodbye!
Encoded:   hello%20world%20%26%20goodbye%21

Space → %20 or +
&     → %26
!     → %21
```

**Security Application:**

```
Attack:   <script>alert(1)</script>
Encoded:  %3Cscript%3Ealert(1)%3C%2Fscript%3E

Filter looks for: <script>
Sees: %3Cscript%3E
Result: Bypass if filter doesn't decode first!
```

### 3.2 Unicode Encoding (Page 67)

**UTF-8 Multi-byte Encoding:**

| Character | UTF-8 Bytes | Hex |
|-----------|-------------|-----|
| `A` | 1 byte | `41` |
| `é` | 2 bytes | `C3 A9` |
| `中` | 3 bytes | `E4 B8 AD` |
| `𐍈` | 4 bytes | `F0 90 8D 88` |

**Unicode Normalization Attacks:**

```
Character:  ＜ (FULLWIDTH LESS-THAN SIGN, U+FF1C)
Looks like: < (LESS-THAN SIGN, U+003C)

Filter sees: Different character, allows it
Browser sees: Visually similar, renders as <

Result: ＜script＞alert(1)＜/script＞ executes!
```

### 3.3 HTML Encoding (Page 68)

**Purpose:** Display special characters safely in HTML

```
Original:  <script>alert("XSS")</script>
Encoded:   &lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt;

<  → &lt;    >  → &gt;
"  → &quot;  '  → &#x27; or &apos;
&  → &amp;   space → &nbsp;
```

**Context Matters:**

```html
<!-- HTML content context -->
<div>USER_INPUT_HERE</div>           ← Use HTML encoding

<!-- JavaScript context -->
<script>var x = 'USER_INPUT_HERE';</script>  ← HTML encoding NOT enough!
                                                    Need JavaScript encoding

<!-- URL context -->
<a href="https://example.com/search?q=USER_INPUT">  ← Need URL encoding

<!-- CSS context -->
<style>.user-theme { color: USER_INPUT; }</style>  ← Need CSS encoding
```

### 3.4 Base64 Encoding (Page 69)

**Purpose:** Encode binary data as ASCII text

```
Encoding process:
Binary data → Groups of 6 bits → Map to A-Z, a-z, 0-9, +, /

Example: "Hello" → "SGVsbG8="

H   e   l   l   o
01001000 01100101 01101100 01101100 01101111
010010 000110 010101 101100 011011 000110 1111xx
S      G      V      s      b      G      8      =
```

**Security Applications:**

```
Basic Auth: Authorization: Basic YWRtaW46cGFzc3dvcmQ=
            Decodes to: admin:password

Data URIs:  data:image/png;base64,iVBORw0KGgo...
            Can embed entire files in URL

JWT Tokens: Header.Payload.Signature (all Base64Url encoded)
```

### 3.5 Hex Encoding (Page 69)

```
Text:     hello
Hex:      68656c6c6f

Common in:
- URL encoding (%68%65%6c%6c%6f)
- Memory addresses
- Hash values (MD5, SHA-256)
- Binary data representation
```

---

## 🔷 PART 4: PRACTICAL APPLICATIONS

### 4.1 Analyzing HTTP Traffic (Hands-On)

**Using Browser DevTools:**

1. Open Chrome/Firefox → Press F12
2. Go to Network tab
3. Check "Preserve log"
4. Navigate to a website
5. Click any request to inspect:

```
General:
  Request URL: https://example.com/api/users
  Request Method: GET
  Status Code: 200 OK

Request Headers:
  Accept: application/json
  Authorization: Bearer eyJhbG...

Response Headers:
  Content-Type: application/json; charset=utf-8
  X-Frame-Options: DENY

Response:
  [{"id": 1, "name": "Admin", "role": "administrator"}...]
```

**Using Burp Suite:**

```
1. Configure browser proxy: 127.0.0.1:8080
2. Install CA certificate for HTTPS
3. Intercept requests in Proxy → Intercept
4. Send to Repeater for manual modification
5. Use Intruder for automated attacks
```

### 4.2 Identifying Technologies (Reconnaissance)

| Technique | What to Look For | Tool |
|-----------|-----------------|------|
| HTTP headers | `Server`, `X-Powered-By`, `X-AspNet-Version` | curl, Burp |
| File extensions | `.php`, `.jsp`, `.aspx` | Dirb, Gobuster |
| Error pages | Stack traces, default error pages | Manual browsing |
| Cookie names | `PHPSESSID`, `JSESSIONID`, `ASP.NET_SessionId` | DevTools |
| HTML comments | `<!-- Generated by WordPress 5.8 -->` | View source |
| JavaScript files | Framework signatures, API endpoints | Wappalyzer |

**Wappalyzer Browser Extension:**

Shows technology stack at a glance:
- CMS (WordPress, Drupal)
- JavaScript frameworks (React, Angular, Vue)
- Web servers (Apache, Nginx)
- Analytics (Google Analytics, Mixpanel)

### 4.3 Encoding/Decoding for Attacks

**Scenario: Bypassing a WAF (Web Application Firewall)**

```
Target:  http://example.com/search?q=<script>alert(1)</script>

Attempt 1: Direct
Result: Blocked by WAF

Attempt 2: URL encoding
?q=%3Cscript%3Ealert(1)%3C%2Fscript%3E
Result: Blocked (WAF decodes)

Attempt 3: Double URL encoding
?q=%253Cscript%253Ealert(1)%253C%252Fscript%253E
Result: Server decodes once → %3Cscript%3E... 
        Application decodes again → <script>...
        SUCCESS if WAF only checks once!

Attempt 4: Unicode normalization
?q=%EF%BC%9Cscript%EF%BC%9Ealert(1)%EF%BC%9C/script%EF%BC%9E
(Fullwidth characters)
Result: May bypass if WAF doesn't normalize
```

---

## 🔷 PART 5: SECURITY IMPLICATIONS SUMMARY

### Key Attack Vectors by Technology

```
┌─────────────────────────────────────────────────────────────┐
│  HTTP PROTOCOL ATTACKS                                       │
├─────────────────────────────────────────────────────────────┤
│  • Verb tampering: Change GET to POST/PUT/DELETE            │
│  • Header injection: CRLF in headers                        │
│  • Request smuggling: Content-Length vs Transfer-Encoding   │
│  • Cache poisoning: Manipulating caching headers            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  COOKIE/SESSION ATTACKS                                      │
├─────────────────────────────────────────────────────────────┤
│  • Session fixation: Force known session ID on victim       │
│  • Cookie theft: XSS, network sniffing (if no Secure flag)  │
│  • Cookie tampering: Modify values (if no integrity check)  │
│  • CSRF: Cross-site requests with victim's cookies          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  ENCODING-BASED BYPASSES                                     │
├─────────────────────────────────────────────────────────────┤
│  • Double encoding: %2520 instead of %20                    │
│  • Unicode normalization: Different character codes         │
│  • Mixed encoding: Combine multiple schemes                 │
│  • Null byte injection: %00 to truncate strings             │
│  • Overlong UTF-8: Non-standard byte sequences              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│  CLIENT-SIDE ATTACKS                                         │
├─────────────────────────────────────────────────────────────┤
│  • XSS: Inject scripts via unencoded output                 │
│  • DOM manipulation: Modify client-side logic               │
│  • Local storage theft: Steal from localStorage/sessionStorage│
│  • Prototype pollution: Modify JavaScript object prototypes │
└─────────────────────────────────────────────────────────────┘
```

---

## 📋 CHAPTER 3 CHECKLIST

**HTTP Protocol:**
- [ ] Understand request/response structure
- [ ] Know all HTTP methods and their properties
- [ ] Can identify and explain common headers
- [ ] Understand cookie security flags

**Encoding:**
- [ ] Can manually URL encode/decode
- [ ] Understand Base64 encoding
- [ ] Know when to use different encoding schemes
- [ ] Can identify encoding-based bypass opportunities

**Architecture:**
- [ ] Understand client-server separation
- [ ] Know why client-side validation is insufficient
- [ ] Understand session management challenge
- [ ] Can identify server technologies

**Practical Skills:**
- [ ] Can use browser DevTools to analyze traffic
- [ ] Can use Burp Suite to intercept/modify requests
- [ ] Can identify technologies using Wappalyzer
- [ ] Can construct encoding-based attacks

---

## 🎯 REVIEW QUESTIONS

1. **Why is HTTP called "stateless"? How do applications maintain state?**

2. **Explain the difference between these cookie flags:**
   - Secure
   - HttpOnly
   - SameSite=Strict vs SameSite=Lax

3. **A WAF blocks `<script>alert(1)</script>`. Describe three encoding-based bypass attempts.**

4. **Why is client-side JavaScript validation insufficient for security?**

5. **What information can you gather from these headers:**
   ```
   Server: nginx/1.18.0
   X-Powered-By: PHP/7.4.3
   Set-Cookie: PHPSESSID=abc123; path=/
   ```

6. **Explain the security implications of JWT tokens being "self-contained."**

---

## 📚 CONNECTIONS TO OTHER CHAPTERS

| This Chapter | Connects To | Why |
|-------------|-------------|-----|
| HTTP methods | Chapter 4 (Mapping) | Discovering hidden functionality |
| Cookies | Chapter 7 (Session Management) | Attacking session tokens |
| Encoding | Chapter 9 (SQL Injection), Chapter 12 (XSS) | Bypassing input filters |
| Headers | Chapter 13 (Other Techniques) | Header injection attacks |
| REST APIs | Chapter 11 (Application Logic) | API-specific vulnerabilities |

---

## 🔧 RECOMMENDED TOOLS FOR PRACTICE

| Tool | Purpose | Free? |
|------|---------|-------|
| **Burp Suite Community** | HTTP proxy, request modification | Yes (limited) |
| **Firefox/Chrome DevTools** | Traffic analysis, debugging | Yes |
| **curl** | Command-line HTTP requests | Yes |
| **Wappalyzer** | Technology identification | Yes |
| **CyberChef** | Encoding/decoding operations | Yes |
| **Postman** | API testing | Yes |

---

This lecture provides the complete theoretical foundation from Chapter 3 while emphasizing practical applications. Master these concepts before proceeding to attack techniques—every vulnerability exploitation requires understanding these underlying technologies.