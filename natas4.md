# Natas 4

## What This Level Teaches

This level introduces **HTTP headers**, especially the `Referer` header.

The main lesson is:

> **Headers sent by the browser can often be changed by the user.**

If a website trusts a header as proof of authorization, that can create a security problem.

---

## Goal

Find the password for **`natas5`**.

## Login

- **URL:** `http://natas4.natas.labs.overthewire.org`
- **Username:** `natas4`
- **Password:** `JDrPnuZAKyl6MkiqQGFIddrqpvgOASth`

---

## Walkthrough

### 1. Open the Natas 4 Page

Go to:

```text
http://natas4.natas.labs.overthewire.org
```

Log in with the username and password above.

The page says that access is not allowed because you are not visiting from the correct place.

### 2. Read the Error Message Carefully

The page says that authorized users should come from:

```text
http://natas5.natas.labs.overthewire.org/
```

The phrase "come from" is the important clue.

In HTTP, the header that tells a server what page the request came from is called:

```text
Referer
```

The name is historically misspelled as `Referer`, not `Referrer`.

### 3. Send a Request With a Custom `Referer`

Use `curl` to send the request manually:

```bash
curl -u natas4:JDrPnuZAKyl6MkiqQGFIddrqpvgOASth \
  -H "Referer: http://natas5.natas.labs.overthewire.org/" \
  http://natas4.natas.labs.overthewire.org/
```

The `-u` option sends the username and password for HTTP Basic Authentication.

The `-H` option adds a custom HTTP header.

The custom header is:

```http
Referer: http://natas5.natas.labs.overthewire.org/
```

### 4. Read the Response

If the `Referer` header matches what the server expects, the response contains the password for the next level.

---

## How Did We Know to Change the `Referer` Header?

The page tells us that access depends on where the request came from.

The reasoning is:

1. The page says we are not coming from the correct address.
2. HTTP has a header that can describe the previous page.
3. That header is `Referer`.
4. The server appears to trust the `Referer` value.
5. We can send a request with the expected `Referer`.
6. The server returns the next password.

---

## What Is an HTTP Header?

HTTP is the protocol browsers use to communicate with web servers.

An HTTP request contains more than just the URL. It can also include headers, which are extra pieces of information.

Examples of request headers include:

- `Host`: tells the server which website the browser wants.
- `Authorization`: sends login information for HTTP Basic Authentication.
- `Cookie`: sends stored cookie values back to the server.
- `Referer`: tells the server which page linked to the current request.

For this level, the important part of the request is:

```http
GET / HTTP/1.1
Host: natas4.natas.labs.overthewire.org
Authorization: Basic ...
Referer: http://natas5.natas.labs.overthewire.org/
```

The request method is `GET`, which means "please send me this resource."

---

## Security Lesson

Do not use the `Referer` header as real access control.

The `Referer` header can be missing, changed, or manually created. It can be useful for analytics or navigation context, but it should not decide whether someone is allowed to access sensitive data.

Real access control should be based on authentication, authorization, and server-side checks.

---

## Key Takeaways

- HTTP headers are part of the request sent to the server.
- The `Referer` header can describe where a request came from.
- Users can modify headers with tools like `curl`.
- Trusting client-controlled headers for authorization is unsafe.
- The correct spelling of the header is `Referer`.
