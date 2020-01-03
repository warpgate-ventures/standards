# Signing Commits with GPG

## About signed commits
GitHub allows its users to sign and verify commits using GPG. When you sign your commits, you provide a guarantee to other people that you actually created that particular commit. This lessens the risk of identity spoofing and provides a security mechanism to certify that the author of a commit is the legitimate person associated with that GitHub account.

## Prerequisites
For any of these processes, `gpg` must be available to use on the command line.
- GPG command line tools must be installed (OS X & Windows doesn't come installed with GPG)
- Download GPG command line tools with GnuPG: [GnuPG](https://www.gnupg.org/download/)
- You can also use GPG Suite for OS X: [GPG Suite](https://gpgtools.org/)

## Generating a new GPG key
If you don't have an existing GPG key, you can generate a new one to use for signing commits.

1. Open Terminal.
2. Run the command below to generate a GPG key pair.
```bash
$ gpg --full-generate-key
```
3. At the prompt, press `Enter` to accept the default `RSA and RSA`.
4. Enter the maximum of `4096` for the desired key size.
5. Enter the length of time the key should be valid. Press `Enter` to specify the default selection, indicating that the key doesn't expire.
6. Verify that your selections are correct.
7. Enter your user ID information. When asked to enter your email address, make sure to enter the email address associated with your GitHub account.
8. Type a secure passphrase.

## Adding a GPG key to your GitHub account
After creating a GPG key, follow the instructions below to add your GPG key to your GitHub account.

1. Open Terminal.
2. Use the following command to list GPG keys for which you have both a public key and a private key. A private key is required for signing commits or tags.
```bash
$ gpg --list-secret-keys --keyid-format LONG
```
3. From the list of GPG keys, copy the GPG key ID you'd like to use. The ID is the series of characters **after** the slash (`/`) character on the `sec` row.
4. Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`
5. Go to GitHub settings, and click on `SSH and GPG keys`.
6. Click on `New GPG key`.
7. In the `Key` field, paste the GPG key you copied from step 3.
8. Click `Add GPG key`.
9. Enter your GitHub password to confirm the action.

## Telling Git about your GPG key
After setting up your GPG key and added it to your GitHub account, you need to tell Git that there's a GPG key you'd like to use.
1. Open Terminal.
2. Use the following command to list GPG keys for which you have both a public key and a private key. A private key is required for signing commits or tags.
```bash
$ gpg --list-secret-keys --keyid-format LONG
```
3. From the list of GPG keys, copy the GPG key ID you'd like to use. The ID is the series of characters **after** the slash (`/`) character on the `sec` row.
4. Set your GPG signing key in Git using the following command, replacing the signing key with the key you have copied from step 3.
```bash
$ git config --global user.signingkey <signing_key>
```
5. If you aren't using GPG suite, run the following command to add the GPG key to your bash profile.
```bash
echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
```

## Signing commits using GPG
Once you've set up your GPG key and associated it with your GitHub account and Git, you can sign commits locally. Your commits will show as verified within a pull request on GitHub.
1. When committing, add the `-S` flag to tell Git that you want to sign your commit. For example:
```bash
$ git commit -S -m your commit message
```
2. Provide the passphrase you have setup for your GPG key when prompted.
3. If you want to enable auto-signing commits without specifying the `-S` flag on every commit, run the following command:
```bash
git config --global commit.gpgSign true
```
4. If you are using VSCode, you can add the following line to your settings to enable commit signing when using VSCode to add commits.
```json
"git.enableCommitSigning": true
```

Read more: [Signing commits with GPG - User Documentation](https://help.github.com/articles/signing-commits-with-gpg/)
