# 🧠 To-Do – OWASP Juice Shop Challenge Walkthroughs

This document outlines the upcoming web exploitation challenges planned for analysis and documentation as part of the OWASP Juice Shop series. Each challenge will be thoroughly tested, with payloads crafted to mimic real-world attack vectors. Upon successful exploitation, each vulnerability will be documented in the repository with:

- Technical summary of the vulnerability
- Payload used and its mechanism of action
- Exploitation methodology and reasoning
- Screenshots and evidence of successful exploitation
- Potential mitigation strategies

---

## 🔐 Upcoming Challenges

### [ ] Reflected XSS
- **Goal**: Identify and exploit reflected cross-site scripting in URL parameters.
- **Attack Vector**: Malicious JavaScript executed via query string reflection.
- **Objective**: Trigger JavaScript execution and bypass client-side filters.

---

### [ ] Stored XSS
- **Goal**: Inject a persistent payload stored in a backend database or memory.
- **Attack Vector**: Payload saved via comment/review/feedback fields.
- **Objective**: Execute on victim page load to simulate real-world session hijack or defacement.

---

### [ ] SQL Injection – Login Bypass
- **Goal**: Bypass authentication using SQLi in login form inputs.
- **Attack Vector**: `' OR 1=1--` in username or password fields.
- **Objective**: Authenticate as admin without valid credentials.

---

### [ ] Improper Error Handling
- **Goal**: Provoke and analyze verbose errors returned by the application.
- **Attack Vector**: Invalid input to unhandled routes or APIs.
- **Objective**: Gain insights into file paths, technologies, or stack traces useful in further exploitation.

---

### [ ] Broken Access Control
- **Goal**: Access protected resources by modifying IDs or tokens.
- **Attack Vector**: Horizontal/Vertical privilege escalation.
- **Objective**: Demonstrate lack of proper authorization checks.

---

## 🧪 Notes

As each challenge is completed:
- Its status will be marked ✅
- A corresponding markdown entry will be added to the `README.md`
- Screenshots will be added under `/screenshots/`

---

## 🧑‍🎓 Educational Purpose

This checklist is part of an ongoing research and documentation project focused on mastering the exploitation of common web application vulnerabilities through practical, hands-on challenges.

The challenge will be solved the moment you paste this in URL in the dearch bar - 

"http://localhost:3000/#/search?q=%3Ciframe%20src%3D%22javascript%3Aalert(%60xss%60)%22%3E"

POC - ![image](https://github.com/user-attachments/assets/11546c62-9647-4c12-bca3-977f15f26cf0)

Explanation - 

✅ Correct Solution Path: Injecting via DOM Sink (Search Page)
🔹 Step 1: Visit This URL (with payload in q parameter)
plaintext
Copy
Edit
http://localhost:3000/#/search?q=%3Ciframe%20src%3D%22javascript%3Aalert(%60xss%60)%22%3E
🔐 Payload (Decoded):

html
Copy
Edit
<iframe src="javascript:alert(`xss`)">
This uses an iframe tag with javascript: URI, which runs in the DOM sink.

Juice Shop watches for alert(xss) from HTML injection into the DOM, not console directly.

🔹 Step 2: What You Should See
An alert box pops up: xss

Juice Shop should mark the DOM XSS challenge as solved

Confirm in the Score Board (http://localhost:3000/#/score-board)

✅ Pro Tips
If the above doesn’t mark the challenge as solved:

Make sure you open the URL directly, don’t just paste the HTML into the console.

Clear your session cookies and refresh (Ctrl+Shift+R) before retrying.

Ensure the Juice Shop container is running the latest version (bkimminich/juice-shop).

Your Juice Shop must not be running in CTF mode — that disables challenge auto-solves.

