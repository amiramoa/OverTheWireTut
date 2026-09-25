# Natas 6

## What This Level Teaches

This level introduces **server-side source code**, PHP `include` files, and the difference between code that runs on the server and HTML that is sent to the browser.

The main lesson is:

> **Source code can reveal how an application checks your input.**

If a challenge gives you access to the source code, read it carefully and follow any referenced files.

---

## Goal

Find the password for **`natas7`**.

## Login

- **URL:** `http://natas6.natas.labs.overthewire.org`
- **Username:** `natas6`
- **Password:** `7mhjtShJAcld2NYbKHEadnhEwRn2P8VT`

---

## Walkthrough

### 1. Open the Natas 6 Page

Go to:

```text
http://natas6.natas.labs.overthewire.org
```

Log in with the username and password above.

The page shows a form asking for a secret.

### 2. View the Source Code

The page provides a link to view the source code.

Open that source code and look for how the form input is checked.

You should see logic similar to this:

```php
if(array_key_exists("submit", $_POST)) {
    if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is ...";
    } else {
        print "Wrong secret";
    }
}
```

This tells us the submitted form value is compared against a variable named:

```text
$secret
```

### 3. Find Where `$secret` Comes From

Near the top of the source code, there is an `include` line:

```php
include "includes/secret.inc";
```

This means the PHP file loads extra code from another file.

The interesting path is:

```text
includes/secret.inc
```

### 4. Open the Included File

Open the included file directly in the browser:

```text
http://natas6.natas.labs.overthewire.org/includes/secret.inc
```

Inside that file, the secret is:

```text
FOEIUWGHFEEUHOFUOIU
```

### 5. Submit the Secret

Go back to the Natas 6 page and enter:

```text
FOEIUWGHFEEUHOFUOIU
```

Submit the form.

The page returns the password for **Natas 7**.

---

## How Did We Know to Check `includes/secret.inc`?

The source code tells us that the form checks user input against `$secret`.

The reasoning is:

1. The page asks for a secret.
2. The source code shows the submitted value is compared to `$secret`.
3. The code imports another file with `include "includes/secret.inc"`.
4. That included file likely defines the `$secret` variable.
5. If the file is publicly accessible, we can open it directly.
6. The file reveals the secret needed by the form.

---

## What Is Server-Side Code?

Server-side code runs on the web server before the browser receives the page.

PHP is a server-side language. The server executes the PHP code and sends the result, usually HTML, to the browser.

Normally, users do not see the PHP source code. They only see the output.

In this level, the challenge intentionally provides a source-code view. That lets us understand how the application works.

---

## What Is `include`?

In PHP, `include` loads another file into the current script.

For example:

```php
include "includes/secret.inc";
```

This is useful for reusing code or storing values in separate files.

The risk is that if the included file is placed somewhere public, a visitor may be able to request it directly by URL.

---

## Security Lesson

Do not place sensitive files where the web server can serve them directly.

The secret was stored in a separate file, but that file was still inside a web-accessible directory. Because the server allowed us to open it, the secret was exposed.

Sensitive configuration files should be stored outside the public web root, or the server should be configured to block direct access to them.

---

## Key Takeaways

- Read provided source code carefully.
- Look for variables used in security checks.
- Follow included or referenced files.
- Server-side code usually runs before the browser sees anything.
- Files used by server-side code should not be directly accessible from the web.
