# Natas 2

## What This Level Teaches

This level introduces **URL paths**, linked files, and directory listing.

The main lesson is:

> **Files referenced by a web page can lead to other files on the server.**

A page may look empty, but the HTML can reveal file paths worth checking.

---

## Goal

Find the password for **`natas3`**.

## Login

- **URL:** `http://natas2.natas.labs.overthewire.org`
- **Username:** `natas2`
- **Password:** `vsDOxoXyq3wckCP1ZmTZ71ngIA606odB`

---

## Walkthrough

### 1. Open the Natas 2 Page

Go to:

```text
http://natas2.natas.labs.overthewire.org
```

Log in with the username and password above.

The page does not directly show the next password.

### 2. View the Page Source

Open the HTML source.

On Windows or Linux:

```text
Ctrl+U
```

On macOS, depending on the browser:

```text
Option+Command+U
```

### 3. Look for a Referenced File

In the source, look for an image tag.

It may look similar to this:

```html
<img src="files/pixel.png">
```

The important part is the `src` value:

```text
files/pixel.png
```

That tells the browser where to request the image from.

### 4. Open the Directory

Instead of opening only the image file, open the directory:

```text
http://natas2.natas.labs.overthewire.org/files/
```

If directory listing is enabled, the server will show the files inside that folder.

### 5. Open the Useful File

Inside the directory, look for a file named:

```text
users.txt
```

Open it in the browser.

The file contains the username `natas3` followed by the password needed for the next level.

---

## How Did We Know to Check the Directory?

The image path tells us that the server has a folder named `files`.

The reasoning is:

1. The visible page does not show the password.
2. The HTML source references an image.
3. The image is stored inside a directory.
4. That directory may contain other files.
5. If directory listing is enabled, the browser can show those files.
6. One of those files may contain the next password.

---

## What Is a URL Path?

A URL has several parts.

For example:

```text
http://example.com/files/pixel.png
```

The domain is:

```text
example.com
```

The path is:

```text
/files/pixel.png
```

The path tells the server what resource the browser is asking for.

If you remove the filename and visit `/files/`, you are asking the server for the directory itself.

---

## What Is Directory Listing?

Directory listing is when a web server shows the contents of a folder.

For example, visiting:

```text
http://example.com/files/
```

might show:

```text
pixel.png
users.txt
```

This can be useful for development, but it can also expose files that should not be public.

---

## Security Lesson

Do not leave sensitive files in public web directories.

Even if a file is not linked from the visible page, it may still be reachable if someone guesses or discovers the path.

Directory listing should usually be disabled unless there is a clear reason to allow it.

---

## Key Takeaways

- View source to find files loaded by the page.
- Image paths, script paths, and stylesheet paths can reveal directories.
- Directory listing can expose files inside a folder.
- A file that is not visible on the page may still be accessible by URL.

---

## Next Level

Use the password found in `users.txt` to log in to **Natas 3**.
