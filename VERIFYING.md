# Checking a download

Every release I put out comes with the SHA-256 checksums of its files. Here's how to check that a file
on your PC is exactly the one I published, for any of my projects. It takes a couple of minutes, and you
don't need to install anything.

## What a checksum is

A SHA-256 checksum is a 64-character fingerprint of a file, computed from every byte in it. Change
one byte and the checksum changes completely. It's practically impossible to make a different file
with the same checksum. If your file's checksum matches the published one, you have the same file I
published, byte for byte.

## 1. Find the published checksums

Each project's repository has a `SHA256SUMS.txt` file at its top, with one line for each file of the
current version: the version number and the checksum.

## 2. Find your file

- **A file you downloaded directly**, for example from a release page, is wherever you saved it.
- **A Steam Workshop download** is in your Steam library, in
  `steamapps\workshop\content\<game number>\<item number>\`.
  - The item number is the number after `?id=` in the Workshop page's address.
  - The game number is the game's Steam app ID.

  The usual Steam library is `C:\Program Files (x86)\Steam`. The Workshop gives you the file exactly as
  it was uploaded, so its checksum is the published one.
- **A file a project writes onto your PC** (for example a DLL) is described in that project's README,
  with where it goes.

## 3. Compute the checksum of your file

**Windows.** Open the folder that holds the file. Click the address bar, type `powershell` and press
Enter. Then run:
```
Get-FileHash "the file's name"
```
The checksum is under **Hash**. In Command Prompt instead: `certutil -hashfile "the file's name" SHA256`.

**macOS:** `shasum -a 256 "the file's name"` in Terminal.

**Linux:** `sha256sum "the file's name"`.

## 4. Compare

Compare the whole checksum with the line for that file in `SHA256SUMS.txt`. All 64 characters must
be the same; upper or lower case doesn't matter.

On Windows, PowerShell can compare it for you. This prints `True` when your file's checksum is in the
list (replace `PROJECT` with the repository's name):
```
(Invoke-WebRequest -UseBasicParsing https://raw.githubusercontent.com/barkovaz/PROJECT/main/SHA256SUMS.txt).Content -match (Get-FileHash "the file's name").Hash
```

## If it doesn't match

1. **Check the version.** The list is for the current version: if yours is older, update it (Steam
   does this for a Workshop item), then check again.
2. **Get a fresh copy.** Download it again. For a Workshop item, unsubscribe, delete the item's folder
   from `steamapps\workshop\content`, and subscribe again.
3. **Still different?** Don't use the file. Tell me on the project's GitHub page (Issues), or wherever
   you got it. Say where the file came from, and paste the checksum you got.

## What a match tells you, and what it doesn't

A match proves your file is the one I published. On its own, it doesn't prove the published file is
harmless. For that, the whole source is public, so anyone can read it, and where a project can be
rebuilt from its source to the identical file, its README says how.
