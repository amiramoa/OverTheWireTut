# Natas 5

## What This Level Teaches

This level introduces **cookies** and shows why login state should not be stored in a simple client-controlled value.

The main lesson is:

> **A cookie value sent by the browser should not be blindly trusted.**

Users can inspect and change cookies stored in their own browser.

---

## Goal

Find the password for **`natas6`**.

## Login

- **URL:** `http://natas5.natas.labs.overthewire.org`
- **Username:** `natas5`
- **Password:** `e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M`

---

## Walkthrough

### 1. Open the Natas 5 Page

Go to:

```text
http://natas5.natas.labs.overthewire.org
```

Log in with the username and password above.

The page says that you are not logged in.

### 2. Inspect the Cookies

Open Developer Tools and look at the cookies for the site.

In Chrome or Edge:

```text
Developer Tools -> Application -> Cookies
```

In Firefox:

```text
Developer Tools -> Storage -> Cookies
```

Look for a cookie named:

```text
loggedin
```

Its value should be:

```text
0
```

### 3. Change the Cookie Value

Change the value from:

```text
loggedin=0
```

to:

```text
loggedin=1
```

Refresh the page.

If the server trusts that cookie directly, it will treat you as logged in and show the password for the next level.

### 4. Test the Same Idea With `curl`

You can also send the cookie manually with `curl`:

```bash
curl -u natas5:e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M \
  -H "Cookie: loggedin=1" \
  http://natas5.natas.labs.overthewire.org/
```

The `-u` option sends the username and password for HTTP Basic Authentication.

The `-H` option adds a custom HTTP header.

The custom header is:

```http
Cookie: loggedin=1
```

---

## How Did We Know to Check Cookies?

The page says that we are not logged in. Login state is often remembered using cookies or sessions.

The reasoning is:

1. The page says we are not logged in.
2. Websites often use cookies to remember state between requests.
3. Developer Tools shows a cookie named `loggedin`.
4. Its value is `0`, which commonly means false or no.
5. Changing it to `1`, which commonly means true or yes, changes what the server receives.
6. The server trusts that value and returns the next password.

---

## What Is a Cookie?

A cookie is a small piece of data stored by the browser for a website.

After a website sets a cookie, the browser sends it back to the same website on later requests.

For example, the browser might send:

```http
Cookie: loggedin=1
```

Cookies are often used for:

- Login sessions
- Preferences
- Shopping carts
- Tracking state between page loads

Cookies are useful, but they are controlled by the user's browser. That means the server should treat cookie values as user input.

---

## What Is a Session?

A session is a safer way for a website to remember who you are.

In a better design, the browser would not be trusted to say:

```text
loggedin=1
```

Instead, the browser might receive a random session ID:

```text
session=abc123...
```

The server would store the real login state on the server side and look it up using that session ID.

---

## Security Lesson

Do not store important authorization decisions in simple client-controlled cookie values.

A cookie such as `loggedin=1` is easy for the user to create or modify. If the server trusts it directly, the user can change their own access.

Important login and permission checks should be validated on the server.

---

## Key Takeaways

- Cookies are sent from the browser to the server.
- Users can inspect and edit their own cookies.
- `Cookie: loggedin=1` is the important header for this level.
- Login state should be validated on the server.
- Client-controlled values should not be trusted blindly.
