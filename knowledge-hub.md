# Kowledge Hub

This is my guide collection of how my Fedora system is set up, what programs I use and how should they be used. It is basically a pocket tldr guide book.

## GPG guide

### Generating a gpg key

    gpg --gen-key
    Real Name: {write your name here}
    Email address: {your email here, used at extract step}
    > (O)kay
    Passphrase: {This will be your master password, make it memorable}

Note down your gpg-id. You gonna need it in pass setup.
If you ever need your gpg-id:

    gpg -K

If you wanna change your gpg key to never expire

    gpg --edit-key {gpg-id}
    > expire
    > 0
    > y
    > save

### Exporting a gpg key

It is recommended that you export your gpg keys and store it somewhere secure so you dont lose them. Use a usb stick maybe that only you have physical access to for example.

    mkdir exported-keys
    cd exported-keys
    gpg --output public.pgp --armor --export {your email}
    gpg --output private.pgp --armor --export-secret-key {your email}

### Importing a gpg key

    gpg --import private.pgp
    gpg --import public.pgp

    gpg --edit-key {your email}
    > trust
    > 5
    > y
    > save

## Pass guide

### Installation

    sudo yum install pass

### Setup

It is recommended to have your passwords backed up in cloud. You can use git (Github, GitLab etc.) to do this, just make the repo private.

    pass init {gpg-id}
    pass git init (You gonna need git configured)
    pass git remote add origin git@github.com:repo.git
    pass git push origin main

### Managing passwords

Dont use username or email as a name. Use something like github/personal as name instead

Adding existing passwords

    pass insert path/to/name
    > {pw}
    > {re-pw}

Generating a new password

    pass generate path/to/name

Copy password to clipboard without displaying it

    pass -c path/to/name

Remove password

    pass rm path-to-name

Showing stored names

    pass

Finding a specific name

    pass find name
    pass find *name*

Adding metadata.

    pass edit path/to/name

Your first line is your password. User other lines to store additional info.

    1208t01ghf01t01tg04t
    username: my_awesome_duck
    email: awesome_duck_jr@duckplanet.com
    url: shop.duckplanet.com

If you wanna search for metadata

    pass grep "{something}"
    pass grep "email:"
    pass grep "awesome_duck_jr@duckplanet.com"

Anything git related can be done with

    pass git {options}

Restoring last removed password scenario

    pass rm github/personal
    pass git revert HEAD

I don't know if it is required to call but I do so...

    pass git push

## Github-cli

    gh repo create {name} --private
