# Natas 3

## What This Level Teaches

This level introduces **`robots.txt`**, a file websites can use to give instructions to search-engine crawlers.

The main lesson is:

> **Hiding a page from search engines is not the same as protecting it.**

A path listed in `robots.txt` can still be opened by anyone if the web server allows access.

---

## Goal

Find the password for **`natas4`**.

## Login

- **URL:** `http://natas3.natas.labs.overthewire.org`
- **Username:** `natas3`
- **Password:** `K30JrSRHzjxq3paUQuwozY4MNvmNFyhI`

---

## Walkthrough

### 1. Open the Natas 3 Page

Go to:

```text
http://natas3.natas.labs.overthewire.org
```

Log in with the username and password above.

The page does not immediately show anything useful, so we need to investigate.

### 2. View the Page Source

Right-click the page and choose **View Page Source**, or use:

```text
Ctrl+U
```

On macOS, depending on the browser, you can usually use:

```text
Option+Command+U
```

Inside the HTML source, you will find a comment similar to:

```html
<!-- No more information leaks!! Not even Google will find it this time... -->
```

This is the important clue.

### 3. Think About the Google Hint

The comment specifically mentions **Google**.

Google discovers web pages using automated programs called **web crawlers**. Before crawling a website, well-behaved crawlers commonly check a file named:

```text
robots.txt
```

This file is normally located at the root of a website.

So let's check whether the Natas 3 website has one.

### 4. Open `robots.txt`

Add `/robots.txt` to the website address:

```text
http://natas3.natas.labs.overthewire.org/robots.txt
```

You should see:

```text
User-agent: *
Disallow: /s3cr3t/
```

Let's break that down:

- `User-agent: *` means the rule applies to all crawlers.
- `Disallow: /s3cr3t/` asks crawlers not to visit or index the `/s3cr3t/` directory.

The important part is:

```text
/s3cr3t/
```

`robots.txt` does **not** prevent us from visiting that directory ourselves.

### 5. Open the Hidden Directory

Visit:

```text
http://natas3.natas.labs.overthewire.org/s3cr3t/
```

The server allows directory listing, so you should see a file named:

```text
users.txt
```

Open it:

```text
http://natas3.natas.labs.overthewire.org/s3cr3t/users.txt
```

The file contains the username `natas4` followed by the password needed for the next level.

Use that password to log in to **Natas 4**.

---

## How Did We Know to Check `robots.txt`?

The reasoning is more important than memorizing the solution.

We started with this clue:

```text
Not even Google will find it this time...
```

From there:

1. Google is a search engine.
2. Search engines use crawlers to discover pages.
3. Crawlers commonly read `/robots.txt`.
4. `robots.txt` can tell crawlers which paths they should avoid.
5. Those paths may still be publicly accessible.
6. Therefore, checking `/robots.txt` may reveal the location the challenge is trying to hide.

This gives us the complete path:

```text
Page source
  ->
Google hint
  ->
Search-engine crawlers
  ->
/robots.txt
  ->
Disallow: /s3cr3t/
  ->
/s3cr3t/
  ->
users.txt
  ->
natas4 password
```

---

## What Is `robots.txt`?

`robots.txt` is a plain-text file used to give instructions to web crawlers.

For example:

```text
User-agent: *
Disallow: /private/
```

This essentially tells crawlers:

> "Please do not crawl `/private/`."

But this is only an instruction. It is **not a security control**.

If the server allows access to `/private/`, a person can still type the URL directly into a browser or request it using tools such as `curl`.

For example:

```bash
curl http://example.com/private/
```

If the server returns the page, then the resource is accessible regardless of what `robots.txt` says.

---

## Security Lesson

Never use `robots.txt` to protect sensitive information.

A line such as:

```text
Disallow: /secret-admin-panel/
```

may actually reveal an interesting path to anyone who reads the file.

Sensitive resources should instead be protected using real security controls such as:

- Authentication
- Authorization
- Proper server permissions

`robots.txt` controls **crawler behavior**, not **user access**.

---

## Key Takeaways

- Check the HTML source for comments and clues.
- A hint involving Google or search engines may point toward `robots.txt`.
- `robots.txt` is normally available at `/robots.txt`.
- `Disallow` tells compliant crawlers not to crawl a path.
- `Disallow` does not prevent a human from opening that path.
- Security through obscurity is not a replacement for access control.
