# OWASP Juice Shop – XSS Challenge Walkthroughs

This repository is a curated collection of exploit walkthroughs for XSS (Cross-Site Scripting) vulnerabilities discovered and exploited in the [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/), a modern intentionally vulnerable web application used for security testing and training.

Each challenge covered here includes a detailed explanation of:
- The vulnerability type
- The injection point
- The payload used
- The exploit technique
- Expected behavior and browser execution flow
- How OWASP Juice Shop internally marks the challenge as solved

---

## ✅ DOM-based Cross-Site Scripting (DOM XSS)

### 🔍 Challenge Overview

The DOM XSS challenge involves exploiting a client-side vulnerability where untrusted user input is dynamically inserted into the Document Object Model (DOM) without proper sanitization or encoding, allowing arbitrary JavaScript execution in the browser.

This is different from **Reflected XSS**, as no server-side rendering or reflection is involved. Instead, the vulnerability exists purely in JavaScript-based DOM manipulation — typically in single-page applications (SPAs) using frameworks like AngularJS.

---

### ⚙️ Technical Details

- **Challenge Name**: `DOM XSS`
- **Injection Parameter**: `q` in the route `/search?q=`
- **Vulnerable Component**: AngularJS-based dynamic DOM rendering
- **Sink Function**: Angular's binding to `innerHTML` (potentially unsafe)
- **Browser Context**: Executed within the browser DOM — not reflected from server

---

### 💣 Exploit Payload Used

We used the following payload to exploit the DOM sink and trigger JavaScript execution:

```html
<iframe src="javascript:alert(`xss`)">

URL ENCODED VERISON IN THE BROWSER
http://localhost:3000/#/search?q=%3Ciframe%20src%3D%22javascript%3Aalert(%60xss%60)%22%3E


🧪 Explanation of Payload
The <iframe> element allows embedding external or JavaScript content in a subcontext.

The src="javascript:..." trick is used to inject JavaScript directly into the iframe.

alert('xss') is used because Juice Shop specifically monitors for an alert with the payload xss in order to register the challenge as solved.

The backtick version alert(xss) is also accepted by the frontend challenge validator.

When this payload is loaded in the DOM through Angular’s dynamic binding, it triggers the alert popup, proving the XSS vector worked.

✅ Challenge Solving Mechanism
Internally, Juice Shop uses a frontend Angular service called challengeService. When the DOM executes the right payload, the app calls:

ts
Copy
Edit
this.challengeService.solveFindChallenge('domXssChallenge')
This marks the challenge as solved in the Score Board. You can confirm this by:

Visiting http://localhost:3000/#/score-board

Or by querying the challenge status via the API:

bash
Copy
Edit
curl -s http://localhost:3000/api/Challenges | jq -r '.data[] | select(.name=="DOM XSS") | "\(.name) | Solved: \(.solved)"'
📷 Screenshot
A screenshot showing the DOM XSS challenge marked as solved is available in the screenshots/ folder for reference.

🔭 Future Additions
We plan to add similar writeups for:

Reflected XSS

Stored XSS

SQL Injection

Broken Access Control

Improper Error Handling

Insecure JWT token handling

Each will include:

Payloads used

Screenshots

HTTP requests/responses

Explanations of server-side/client-side behavior

👨‍💻 Author
This work is part of an ongoing offensive security research initiative aimed at understanding real-world web application flaws and documenting exploit methods in an educational and reproducible way.


