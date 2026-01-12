#Linux 

Adding a key to `/etc/apt/trusted.gpg.d` is **insecure** because it adds the key for all repositories. This is exactly why `apt-key` had to be _deprecated_.

## Short version

Do similar to what [Signal](https://signal.org/download/) does. If you want to use the key at `https://example.com/EXAMPLE.gpg` for a repository listed in `/etc/apt/sources.list.d/EXAMPLE.sources`, use:

```bash
sudo mkdir -m 0755 -p /etc/apt/keyrings/

curl -fsSL https://example.com/EXAMPLE.gpg |
    sudo gpg --dearmor -o /etc/apt/keyrings/EXAMPLE.gpg

echo "Types: deb
URIs: https://example.com/apt
Suites: stable
Components: main
Signed-By: /etc/apt/keyrings/EXAMPLE.gpg" |
    sudo tee /etc/apt/sources.list.d/EXAMPLE.sources > /dev/null
```

```bash
# Optional (you can find the email address / ID using `apt-key list`)
sudo apt-key del support@example.com
```

```bash
# Optional (not necessary on most systems)
sudo chmod 644 /etc/apt/keyrings/EXAMPLE.gpg
sudo chmod 644 /etc/apt/sources.list.d/EXAMPLE.sources
```

> ℹ️ You can also embed the key directly into the `.sources` file! See the section "Embedding the key" below.

> ℹ️ We're using the `.sources` format here, not the old `.list` format. This is supported on basically all systems today. See the section "Old one-line format" below for more info.

## Long version

While the deprecation notice recommends adding the key to `/etc/apt/trusted.gpg.d`, this is an insecure solution, and [deprecated as of apt 2.9.24](https://salsa.debian.org/apt-team/apt/-/raw/2.9.24/debian/NEWS) (released January 2025). To quote [this article from Linux Uprising](https://www.linuxuprising.com/2021/01/apt-key-is-deprecated-how-to-add.html):

> The reason for this change is that when adding an OpenPGP key that's used to sign an APT repository to `/etc/apt/trusted.gpg` or `/etc/apt/trusted.gpg.d`, the key is unconditionally trusted by APT on all other repositories configured on the system that don't have a `signed-by` (see below) option, even the official Debian / Ubuntu repositories. As a result, any unofficial APT repository which has its signing key added to `/etc/apt/trusted.gpg` or `/etc/apt/trusted.gpg.d` can replace any package on the system. So this change was made for security reasons (your security).

The proper solution is explained in [that Linux Uprising article](https://www.linuxuprising.com/2021/01/apt-key-is-deprecated-how-to-add.html) and on the [Debian Wiki](https://wiki.debian.org/DebianRepository/UseThirdParty#OpenPGP_certificate_distribution): Store the key in `/etc/apt/keyrings/` (or `/usr/share/keyrings/` if keys are managed by a package), and then reference the key in the apt source list.

Therefore, the appropriate method is as follows:

1. **Create directory**  
    Create the directory for PGP keys if it doesn't exist, and set its permissions. This step explicitly sets the [recommended permissions](https://wiki.debian.org/DebianRepository/UseThirdParty#OpenPGP_certificate_distribution), just in case you've changed your [umask](https://en.wikipedia.org/wiki/Umask) using sudo's `umask_override`. Creating the directory is actually only necessary in releases older than Debian 12 and Ubuntu 22.04, but it can't hurt to run this line either way.
    

- ```bash
    sudo mkdir -m 0755 -p /etc/apt/keyrings/
    ```
    
- **Download key**  
    Download the key from `https://example.com/EXAMPLE.gpg` and store it in `/etc/apt/keyrings/EXAMPLE.gpg`. By giving options `-fsSL` to `curl` we enable error messages, ensure redirects are followed, and reduce output so you can see sudo's password prompt. The [Debian wiki explains](https://wiki.debian.org/DebianRepository/UseThirdParty#OpenPGP_Key_distribution) that you should dearmor the key (i.e. convert it from base64 to binary) for compatibility with older software.
    

1. ```bash
    curl -fsSL https://example.com/EXAMPLE.gpg |
        sudo gpg --dearmor -o /etc/apt/keyrings/EXAMPLE.gpg
    ```
    
    Optionally, you can verify that the file you downloaded is indeed a PGP key by running `file /etc/apt/keyrings/EXAMPLE.gpg` and inspecting the output.
    
2. **Register repository**  
    A key has been added, but `apt` doesn't know about the repository yet. To add the repository, you should create a `.sources` file in `/etc/apt/sources.list.d/` that describes how to use the repository, and where to find the key. (You may also have `.list` files in that directory. See the section "Old one-line format" below for more info.) The contents of the created `.sources` file should look something like this:
    
    ```
    Types: deb
    URIs: https://example.com/apt
    Suites: stable
    Components: main
    Signed-By: /etc/apt/keyrings/EXAMPLE.gpg
    ```
    
    The `Signed-By` field should link to the key you just downloaded.
    
    If a repository wants you to specify an architecture, or you want to use multiple components (e.g. `main contrib`), the contents may instead be something like
    
    ```
    Types: deb
    URIs: https://example.com/apt
    Suites: stable
    Components: main universe
    Architectures: amd64 i386
    Signed-By: /etc/apt/keyrings/EXAMPLE.gpg
    ```
    
    A "flat" repository doesn't work with suites and components, and instead specifies an exact path. In the DEB822 format, this is represented by setting `Suites:` to that path, and omitting the `Components:` field entirely. In this case, the `Suites:` field **must** end with a `/`. For example:
    
    ```
    Types: deb
    URIs: https://example.com/apt
    Suites: deb/
    Signed-By: /etc/apt/keyrings/EXAMPLE.gpg
    ```
    
    If you are adapting the file from an existing repo, they may be using the old one-line format instead. See the section "Old one-line format instead" below for more info.
    
    For more examples, see the [`sources.list(5)` man pages](https://manpages.debian.org/stable/apt/sources.list.5.en.html).
    
3. **(optional) Remove old key**  
    If you previously added a third-party key with `apt-key`, you should remove it. Run `sudo apt-key list` to list all the keys, and find the one that was previously added. Then, using the key's email address or fingerprint, run `sudo apt-key del support@example.com`.
    
4. **(optional) Force-set permissions**  
    If you have a custom `umask_override` set for `sudo`, or if you use ACLs, files will be created with different permissions than usual. In those cases, explicitly set permissions for `EXAMPLE.gpg` and `EXAMPLE.list` to `644`.
    

## Embedding the key

[apt 2.3.10 and newer support embedding the public key directly in the `sources.list`](https://salsa.debian.org/apt-team/apt/-/merge_requests/176). You can check your version of apt by running `apt -v`. Debian 11 "Bullseye" LTS (EOL: 2026-08-31) and Ubuntu 20.04 "Focal Fossa" (EOL: 2025-04-30) are too old, but Debian 12 "Bookworm" and Ubuntu 22.04 "Jammy Jellyfish" are good to go!

To embed a key, replace the path in `Signed-By: /etc/apt/keyrings/EXAMPLE.gpg` with the raw key, and delete the file `/etc/apt/keyrings/EXAMPLE.gpg`. Importantly, you **must** indent each line of the key block by (at least) one space, and you **must** put an indented `.` instead of an empty line. (Removing the empty line at the start of the key invalidates the key!) For example, you may have a `.sources` file like below. (Real keys should be much longer than this. This one is too short to be secure.)

```
Types: deb
URIs: https://example.com/apt
Suites: stable
Components: main
Signed-By:
 -----BEGIN PGP PUBLIC KEY BLOCK-----
 .
 mI0EZWiPbwEEANPyu6pUQEydxvf2uIsuuYOernFUsQdd8GjPE5yjlxP6pNhVlqNo
 0fjB6yk91pWsoALOLM+QoBp1guC9IL2iZe0k7ENJp6o7q4ahCjJ7V/kO89mCAQ09
 yHGNHRBfbCo++bcdjOwkeITj/1KjYAfQnzH5VbfmgPfdWF4KqS/TmJP9ABEBAAG0
 G0phbmUgRG9lIDxqYW5lQGV4YW1wbGUub3JnPojMBBMBCgA2FiEEK8v49DttJG7D
 35BwcvTpbeNfCTgFAmVoj28CGwMECwkIBwQVCgkIBRYCAwEAAh4BAheAAAoJEHL0
 6W3jXwk4YLID/0arCzBy9utS8Q8g6FDtWyJVyifIvdloCvI7hqH51ZJ+Zb7ZLwwY
 /p08+Xnp4Ia0iliwqSHlD7j6M8eBy/JJORdypRKqRIbe0JQMBEcAOHbu2UCUR1jp
 jJTUnMHI0QHWQEeEkzH25og6ii8urtVGv1R2af3Bxi9k4DJwzzXc5Zch
 =8hwj
 -----END PGP PUBLIC KEY BLOCK-----
```

The script below will create a new `.sources` file at `/etc/apt/sources.list.d/EXAMPLE.sources` with the key at `https://example.com/EXAMPLE.gpg` embedded into it:

```bash
echo "Types: deb
URIs: https://example.com/apt
Suites: stable
Components: main
Signed-By:
$(wget -O- https://example.com/EXAMPLE.gpg | sed -e 's/^$/./' -e 's/^/ /')" | sudo tee /etc/apt/sources.list.d/EXAMPLE.sources > /dev/null
```

```bash
# Optional (see above)
sudo apt-key del support@example.com
sudo chmod 644 /etc/apt/sources.list.d/EXAMPLE.sources
```

## Old one-line format

Thus far, this answer has used the "new" DEB822 format (`.sources` files) to specify repositories, instead of the old one-line format (`.list` files). The DEB822 format has been supported since apt 1.1 (released in 2015). [Debian and Ubuntu plan to use DEB822 as the default format starting late 2023.](https://discourse.ubuntu.com/t/spec-apt-deb822-sources-by-default/29333) [Repolib's documentation has a nice comparison and covers the motivation behind the new format.](https://repolib.readthedocs.io/en/latest/deb822-format.html) If you are running Debian 9 "Stretch" or newer, or Ubuntu 16.04 "Xenial Xerus" or newer, you're good to go!

This section is intended for those who cannot use DEB822, or for those who want to migrate to DEB822.

### Using the old one-line format

The following script replaces the DEB822 script from the section "Short version", at the top of this answer.

```bash
sudo mkdir -m 0755 -p /etc/apt/keyrings/

curl -fsSL https://example.com/EXAMPLE.gpg |
    sudo gpg --dearmor -o /etc/apt/keyrings/EXAMPLE.gpg

echo "deb [signed-by=/etc/apt/keyrings/EXAMPLE.gpg] https://example.com/apt stable main" |
    sudo tee /etc/apt/sources.list.d/EXAMPLE.list > /dev/null
```

```bash
# Optional (see above)
sudo apt-key del support@example.com
sudo chmod 644 /etc/apt/keyrings/EXAMPLE.gpg
sudo chmod 644 /etc/apt/sources.list.d/EXAMPLE.list
```

### Migrating to DEB822

If you can, you should migrate to DEB822, because that will be the default format in Debian and Ubuntu. That said, as of February 2025, there is no urgency to migrate, because the old one-line format is not deprecated.

Since apt 2.9.24 (released January 2025), you can run `apt modernize-sources` to automatically migrate all your `.list` files to DEB822. Note, however, that some fields, such as `arch=...` are not migrated correctly, so you must add `Architectures: ...` to your `.sources` file manually. Fields with multiple values are space-separated, so multiple architectures would be written as `Architectures: amd64 i386`.