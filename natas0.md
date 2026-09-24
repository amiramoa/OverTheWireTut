# Natas 0

## What This Level Teaches

This level introduces **HTML source code** and shows that a web page can contain information that is not visible in the normal browser view.

The main lesson is:

> **If the browser receives it, the user can inspect it.**

Hiding text from the visible page is not the same as protecting it.

---

## Goal

Find the password for **`natas1`**.

## Login

- **URL:** `http://natas0.natas.labs.overthewire.org`
- **Username:** `natas0`
- **Password:** `natas0`

---

## Walkthrough

### 1. Open the Natas 0 Page

Go to:

```text
http://natas0.natas.labs.overthewire.org
```

Log in with the username and password above.

The page looks simple, but the visible page is only the browser's rendered version of the HTML.

### 2. View the Page Source

Open the page source.

On Windows or Linux, use:

```text
Ctrl+U
```

On macOS, depending on the browser, use:

```text
Option+Command+U
```

You can also use Developer Tools, but viewing source is enough for this level.

### 3. Look for an HTML Comment

Inside the source, look for a comment.

An HTML comment looks like this:

```html
<!-- This is a comment -->
```

Comments are not displayed on the page, but they are still sent to the browser.

The password for the next level is inside one of these comments.

---

## How Did We Know to View Source?

Natas 0 is the first web level, so it starts with one of the most basic web security checks: inspect what the browser received.

The reasoning is:

1. The page does not show the password visibly.
2. The browser still received HTML from the server.
3. HTML can contain comments and hidden details.
4. Viewing source lets us inspect that HTML.
5. The password is inside a comment.

---

## What Is HTML Source?

HTML is the language used to describe the structure of a web page.

For example:

```html
<h1>Hello</h1>
<p>This is a paragraph.</p>
```

The browser reads that HTML and turns it into the page you see.

The **page source** is the raw HTML before the browser visually renders it. Developers use it to understand what the server sent to the browser.

---

## Security Lesson

Never put secrets in HTML comments.

Even though comments are hidden from the normal page view, they are still part of the page source. Anyone can open the source and read them.

Sensitive information should stay on the server and only be shown to users who are allowed to see it.

---

## Key Takeaways

- The rendered page is not the same as the page source.
- HTML comments are hidden visually, but not protected.
- Anything sent to the browser can be inspected by the user.
- Viewing source is one of the first things to try in web challenges.

---

## Next Level

Use the password found in the HTML comment to log in to **Natas 1**.
