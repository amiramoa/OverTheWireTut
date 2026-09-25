# Natas 8

## What This Level Teaches

This level introduces **encoding**, **hexadecimal**, **Base64**, and reading source code to reverse how a value was transformed.

The main lesson is:

> **Encoding is not encryption.**

If the code shows exactly how a value was encoded, we can reverse those steps to recover the original value.

---

## Goal

Find the password for **`natas9`**.

## Login

- **URL:** `http://natas8.natas.labs.overthewire.org`
- **Username:** `natas8`
- **Password:** `ugXL95KQmUAJJj6bMezOlBNDyI9Imwkc`

---

## Walkthrough

### 1. Open the Natas 8 Page

Go to:

```text
http://natas8.natas.labs.overthewire.org
```

Log in with the username and password above.

The page shows a form asking for a secret.

### 2. View the Source Code

The page provides a link to view the source code.

In the source, look for the function that encodes the secret.

It looks like this:

```php
function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}
```

The source also contains an encoded secret:

```text
3d3d516343746d4d6d6c31566934356b
```

### 3. Read the Encoding Steps

The function applies three transformations:

1. `base64_encode($secret)`
2. `strrev(...)`
3. `bin2hex(...)`

That means the original secret was first Base64-encoded, then reversed, then converted to hexadecimal.

To recover the original secret, we do the opposite operations in the opposite order:

1. Convert from hex back to text.
2. Reverse the string.
3. Decode the Base64.

### 4. Decode the Secret With PHP

You can reverse the process with this PHP command:

```bash
php -r 'echo base64_decode(strrev(hex2bin("3d3d516343746d4d6d6c31566934356b")));'
```

The decoded secret is:

```text
oubWYf2kBq
```

### 5. Submit the Secret

Go back to the Natas 8 page and enter:

```text
oubWYf2kBq
```

Submit the form.

The page returns the password for Natas 9:

```text
UdxmI27dTaXmnd1rxKQTfws6jihTdcQ9
```

---

## How Did We Know How to Decode It?

The source code shows the exact order used to encode the secret.

The reasoning is:

1. The page asks for a secret.
2. The source code compares our input after encoding it.
3. The encoded secret is visible in the source.
4. The encoding function shows the transformations.
5. To undo transformations, reverse both the order and the operation.
6. The decoded value is the secret to submit.

---

## What Is Encoding?

Encoding changes data from one representation to another.

Encoding is often used so data can be stored or transmitted safely in text form.

For example, Base64 can turn raw data into characters that are safe to copy, paste, or send through systems that expect text.

Encoding is not meant to be secret. If someone knows the encoding method, they can decode it.

---

## What Is Hexadecimal?

Hexadecimal, often shortened to hex, is a base-16 number system.

It uses these characters:

```text
0 1 2 3 4 5 6 7 8 9 a b c d e f
```

In this level, `bin2hex` converts text into a hex representation.

The reverse operation is:

```text
hex2bin
```

---

## What Is Base64?

Base64 is a common way to encode data as text.

A Base64 value often ends with one or two `=` characters as padding.

In this level, the encoded value starts with:

```text
3d3d
```

The hex value `3d` represents the character `=`, so `3d3d` becomes `==` after converting from hex.

That is a hint that Base64 is involved, especially after reversing the string.

---

## Security Lesson

Do not protect secrets with reversible encoding.

Encoding can hide a value from a quick glance, but it does not provide real security. If the code and encoded value are visible, the original value can be recovered.

Use proper cryptographic hashing, encryption, or server-side secret storage depending on the situation.

---

## Key Takeaways

- Source code can reveal how a value is transformed.
- Encoding is reversible when the method is known.
- To undo several transformations, reverse the order.
- Hex and Base64 are encodings, not security controls.
- Do not confuse obfuscation with protection.
