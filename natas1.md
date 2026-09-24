# Natas 1

## What This Level Teaches

This level shows that **client-side restrictions**, such as disabling right-click, do not protect the page source.

The main lesson is:

> **Browser tricks are not security controls.**

If the browser has the HTML, the user can still inspect it.

---

## Goal

Find the password for **`natas2`**.

## Login

- **URL:** `http://natas1.natas.labs.overthewire.org`
- **Username:** `natas1`
- **Password:** `<password from natas0>`

---

## Walkthrough

### 1. Open the Natas 1 Page

Go to:

```text
http://natas1.natas.labs.overthewire.org
```

Log in with the username `natas1` and the password found in Natas 0.

### 2. Notice That Right-Click Is Blocked

The page disables the right-click menu.

This is meant to stop you from using **View Page Source** through the mouse menu.

That does not mean the source is protected. It only means one browser action has been blocked.

### 3. View the Source Another Way

Use a keyboard shortcut instead.

On Windows or Linux:

```text
Ctrl+U
```

On macOS, depending on the browser:

```text
Option+Command+U
```

You can also open Developer Tools with `F12` or `Ctrl+Shift+I` on Windows/Linux, or `Option+Command+I` on macOS.

### 4. Find the HTML Comment

Inside the HTML source, look for a comment containing the next password.

The idea is the same as Natas 0. The difference is that this level tries to make viewing source slightly less convenient.

---

## How Did We Know Right-Click Was Not Enough?

The right-click block happens in the browser. Your browser is the **client**, meaning it is the program making requests to the website.

The website is served by a **server**, which sends the HTML to your browser.

The reasoning is:

1. The server already sent the HTML to the browser.
2. The browser needs that HTML to display the page.
3. Disabling right-click does not remove the HTML.
4. Other ways to inspect the HTML still work.
5. The password is still visible in the source.

---

## What Is Client-Side Code?

Client-side code runs in the user's browser.

Examples include:

- HTML
- CSS
- JavaScript

Client-side code can change how a page behaves, but it should not be trusted to protect secrets. Users can inspect it, modify it, disable it, or bypass it.

---

## Security Lesson

Do not rely on browser restrictions for security.

Disabling right-click may slow down a casual user, but it does not stop someone from using keyboard shortcuts, Developer Tools, or command-line tools.

Real protection must happen on the server, before sensitive data is sent to the browser.

---

## Key Takeaways

- Right-click blocking is not access control.
- Anything sent to the browser can be inspected.
- Client-side code should not contain secrets.
- Use keyboard shortcuts or Developer Tools when mouse options are blocked.

---

## Next Level

Use the password found in the HTML comment to log in to **Natas 2**.
