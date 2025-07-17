# 🧪 Open Redirect PoC via Pathname + Path Parameters

This repository demonstrates a client-side open redirect vulnerability that occurs due to improper handling of `window.location.pathname` when path parameters and protocol-relative URLs are involved.

## 💡 Summary

In certain web apps, developers use code like:

```js
window.location = window.location.pathname;
```

This seems safe — it should reload the current path. However, when a specially crafted URL is used, the browser's URL parser can interpret it in a dangerous way.

## 🚨 PoC Behavior

Open this crafted URL in the browser:

```
http://localhost:3000//;param@attacker.com/path
```

### What happens:

- `window.location.pathname` resolves to `//;param@attacker.com/path`
- The browser treats this as a **protocol-relative URL**
- According to [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986), `;param@attacker.com` is parsed as `userinfo@host`
- Result: the browser redirects to `https://attacker.com/path`

---

## 📁 Project Structure

```
open-redirect-poc/
├── public/
│   └── index.html              # Frontend file with JS redirect logic
├── server.js                   # Express server to serve static files
├── package.json                # Project metadata + dependencies
├── package-lock.json           # Exact dependency tree (keep this)
├── .gitignore                  # Ignore node_modules and other temp files
└── README.md                   # Explains the PoC and how to run it
```

---

## 🧑‍💻 How to Run Locally

### 1. Clone the repo
```bash
git clone https://github.com/cyc10n3/open-redirect-poc.git
cd open-redirect-poc
```

### 2. Install dependencies
```bash
npm install
```

### 3. Start the server
```bash
node server.js
```

### 4. Open in browser
Visit:
```
http://localhost:3000//;param@attacker.com/path
```

You will be redirected to:
```
https://attacker.com/path
```

---

## 🛡️ Mitigation Tips

To avoid such issues:
- Never assign `window.location = window.location.pathname` without validation
- Always sanitize and validate paths before redirects
- Reject unexpected URL schemes or protocol-relative paths

---

## 📜 License

This project is intended for educational and ethical research purposes only.
