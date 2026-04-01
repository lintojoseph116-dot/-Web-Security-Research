 Reflected XSS into HTML Context (Unencoded)

Target: PortSwigger Academy Lab
Vulnerability Type: Reflected Cross-Site Scripting (XSS)
Severity: High

1. Description

The application’s search functionality is vulnerable to Reflected Cross-Site Scripting. User-supplied input in the search parameter is reflected back to the user within the HTML response without being encoded or sanitized. An attacker can leverage this to execute arbitrary JavaScript in the victim's browser.

2. Proof of Concept (PoC)
   
    Vulnerable URL: https://0a8c00c5032c30c386adcffc00f00010.web-security-academy.net/
    Vulnerable Parameter: search
    Payload: <script>alert(1)</script>

Steps to Reproduce:
    Navigate to the lab homepage.
    In the search box, enter the following payload: <script>alert(1)</script>
    Click "Search."
    Observe the browser executing the script and displaying an alert box.

3. Technical Analysis
When the search request is sent, the server processes the search parameter. Because there is "nothing encoded" (as specified in the lab title), the raw HTML tags are rendered by the browser.

<img width="1368" height="735" alt="xss1" src="https://github.com/user-attachments/assets/dca33aa2-98eb-461f-8eba-4a746502d640" />
<img width="1368" height="735" alt="xss" src="https://github.com/user-attachments/assets/fff1604a-e29e-4a4d-a5d9-bcf59143cfdb" />

4. Impact

An attacker could utilize this vulnerability to:
    Steal session cookies (document.cookie) to hijack user accounts.
    Capture sensitive data through DOM manipulation (Keylogging).
    Perform unauthorized actions on behalf of the user (e.g., changing passwords).

5. Remediation
To fix this vulnerability, the application should perform HTML Entity Encoding on all user-supplied data before rendering it in an HTML context.
    Convert < to &lt;
    Convert > to &gt;
    Convert " to &quot;
   
