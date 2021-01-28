# GPG Keys

## Generating a new key

### Generating a new key with Ubuntu

```text
# on 18.04
gpg --full-gen-key;

# on 16.04/macOS
gpg --gen-key;
```

You will be asked for your name and email address. Note that the email address **will be public**, so use a masked email which both Github \([can be found here](https://github.com/settings/emails)\) and Gitlab \([can be found in the **Commit email** sub-section here](https://gitlab.com/profile)\) provides.

### Listing existing keys

```text
gpg --list-secret-keys --keyid-format LONG
```

## Exporting the GPG key

Use cases: 1. [Adding GPG keys to platforms](https://wiki.joeir.net/gpg/#adding-gpg-keys-to-platforms)

### List the keys and get the key id

```text
gpg --list-secret-keys --keyid-format LONG
# example output:
#   ...
#   sec   4096R/C4CC3911D49AB2D8 2020-01-07 [expires: 2021-01-07]
#               ^^^^^^^^^^^^^^^^
#               \-> the key id
```

### Export the pbulic key

```text
# copy and paste the key id from above to generate the public key
gpg --armor --export C4CC3911D49AB2D8
# example output:
# -----BEGIN PGP PUBLIC KEY BLOCK-----
# ...
# -----END PGP PUBLIC KEY BLOCK-----
```

## Adding GPG keys to platforms

### Adding GPG keys to Gitlab

Login to Gitlab and visit [https://gitlab.com/profile/gpg\_keys](https://gitlab.com/profile/gpg_keys). Run the steps in [Exporting public key](https://wiki.joeir.net/gpg/#exporting-public-key) and paste the exported public key block in.

### Adding GPG keys to Github

Login to Github and visit [https://github.com/settings/keys](https://github.com/settings/keys). Run the steps in [Exporting public key](https://wiki.joeir.net/gpg/#exporting-public-key) and paste the exported public key block in.

## Using GPG keys in Git

### Using a key on a per-repo basis

> This method assumes that you would like to sign commits for only one repository

**Step 1.** The local Git configuration for a repository should be at `.git/config`, open it up and add:

```text
[user]
  name = your name
  email = yourpublicemail@domain.com
  signingkey = XXXXXXXXXXXXXXXX
```

> Replace the `signingkey` value with the desired key ID from the output of the [**List the keys and get the key id** section](https://wiki.joeir.net/gpg/#list-the-keys-and-get-the-key-id).

You're done.

### Using one key to rule them all

> This method assumes that you'd only like to generate one key and use them to sign ALL commits.

**Step 1.** Find the root `.gitconfig` by running:

```text
git config --global --list --show-origin
```

> The root `.gitconfig` is typically at `${HOME}/.gitconfig`

**Step 2.** In the `.gitconfig`, add the following:

```text
[user]
  name = your name
  email = yourpublicemail@domain.com
  signingkey = XXXXXXXXXXXXXXXX
```

> Replace the `signingkey` value with the desired key ID from the output of the [**List the keys and get the key id** section](https://wiki.joeir.net/gpg/#list-the-keys-and-get-the-key-id).

### Using the `includeIf` directive

> This method assumes you have a local directory structure that mimics the SCM platform you're using. For example, a repository at `https://github.com/zephinzer/wiki.joeir.net` should be located at `${HOME}/github.com/zephinzer/wiki.joeir.net` or `${HOME}/code/github.com/zephinzer/wiki.joeir.net`

**Step 1.** Find the root `.gitconfig` by running:

```text
git config --global --list --show-origin
```

> The root `.gitconfig` is typically at `${HOME}/.gitconfig`

**Step 2.** In the root `.gitconfig`, add the directive:

```text
[includeIf "gitdir:**/github.com/"]
  path = ~/.github.com.gitconfig
```

**Step 3.** Then create the file at `~/.github.com.gitconfig` with the following content:

```text
[user]
  name = your name
  email = yourpublicemail@domain.com
  signingkey = XXXXXXXXXXXXXXXX
```

> Replace the `signingkey` value with the desired key ID from the output of the [**List the keys and get the key id** section](https://wiki.joeir.net/gpg/#list-the-keys-and-get-the-key-id).

**Step 4.** Repeat steps 2-3 for as many SCM platforms as you are using

Note that if you, like me commit your dotfiles to a public repository for others to copy/paste as needed, it might be useful to use the `include` directive to include a path that isn't committed to your dotfiles such as:

```text
[include]
  path = ~/.gitconfig.d
```

You can chain other `includeIf` directives from your `~/.gitconfig.d` file as needed to SCM platforms whose URLs are better left out of the public eye.

## Other Resources

* [Signing commits with GPG \(by Gitlab\)](https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/)
* [About commit signature verification \(by Github\)](https://help.github.com/en/github/authenticating-to-github/about-commit-signature-verification)
* [Using GPG Keys \(by BitBucket\)](https://confluence.atlassian.com/bitbucketserver/using-gpg-keys-913477014.html)
* [A Git Horror Story: Repository Integrity With Signed Commits \(independent\)](https://mikegerwitz.com/2012/05/a-git-horror-story-repository-integrity-with-signed-commits)

