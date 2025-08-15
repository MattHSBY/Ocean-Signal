# Setting up SSH keys for use with BitBucket

This document describes one process for setting up SSH keys for use with Bitbucket/Git.

## Assumptions

- You have installed Powershell Core 7.0 or later
- You have installed Git for Windows.

To install Powershell Core, [download][pwsh] and run the installer.

To install `Git for Windows` open a powershell prompt and run:

```pwsh
winget install Git.Git
```

## Creating your private and public key pair

```pwsh
cd ~
mkdir .ssh
cd .ssh/
mkdir keys
cd keys
ssh-keygen -t ed25519 -b 4096 -C "my.user@my.com" -f bitbucket-ssh
```

The `-f bitbucket-ssh` option specifies the output filename.
You can change this to suit your needs - don't specify an extension.

When asked for passphrase, you can either provide a pass phrase or leave it blank.
Providing a passphrase is better for security but it'll mean you have to enter it everything you use a Git command.

You should now have a _private key_ in `bitbucket-ssh` and a _public key_ in `bitbucket-ssh.pub`.

## Add the _private key_ to ssh-agent

```pwsh
ssh-add ~/.ssh/keys/bitbucket-ssh
```

## Configure OpenSSH to Use the Correct Key for Bitbucket

It is necessary to configure OpenSSH to use the correct _private key_ when authenticating with Bitbucket.

Create or edit the file `~/.ssh/config` (no extension) in your favourite editor.
Add the following:

```
Host bitbucket.org
  AddKeysToAgent yes
  IdentityFile ~/.ssh/keys/bibucket-ssh
```

## Adding the _Public Key_ to Bitbucket

The final piece fo the puzzle is to add the _public key_ to Bitbucket.
First, print out the contents of your public key.

```pwsh
cd ~/.ssh/keys
cat bitbucket-ssh.pub
```

This should produce a single line that looks similar to:

> ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBOQLX6YoCik3Q6/YfPrHqMLX7IMGSKFantZfc+hfZpR Tim.Long@oceansignal.com

Copy that line of text to the clipboard.

Now in your browser, go to BitBucket and add the SSH public key.
This can be done in your Personal Settings, under Security, [SSH Keys][ssh-keys].

Select the option to _Add Key_, give it a meaningful label and paste the clipboard contents into the Key box.

## Testing SSH Authentication

If everything has worked, issue the following command to test SSH authentication:

```pwsh
ssh -T git@bitbucket.org
```

This should produce a response like:

> authenticated via ssh key.
>
> You can use git to connect to Bitbucket. Shell access is disabled

You should now be able to use Bitbucket from the Git command line, Visual Studio Code, Visual Studio, or any other program with Git integration.
Don't forget to set your Git username and email if you haven't already done so (these are used to tag commits):

```pwsh
git config --global user.email "Tim.Long@oceansignal.com"
git config --global user.name "Tim Long"
```

[pwsh]: https://github.com/PowerShell/PowerShell/releases/latest "Powershell Core latest release"
[ssh-keys]: https://bitbucket.org/account/settings/ssh-keys/ "Bitbucket personal SSH key management"
