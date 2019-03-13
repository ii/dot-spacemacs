# ii Install and Configuration of Spac(emacs)

[Spacemacs](http://spacemacs.org/) with a number of [Emacs](https://www.gnu.org/software/emacs/) packages is at the center of our workflow.
The goal is to help future users of this workflow to get setup with the least amount of frustration and feeling productive.

> Spacemacs is a new way to experience Emacs -- a sophisticated and polished set-up focused on ergonomics, mnemonics and consistency.


## Install Emacs Snapshot PPA

This installs Emacs-snapshot on Ubuntu Bionic 18.04, though any process that gives us Emacs 27.0.50 is probably fine.

**Note:** Be sure that no other .elc files are loaded

```
CODENAME=$(lsb_release -c -s)
sudo apt-key adv \
  --recv-keys 0D7BAE435ADBC6C3E4918A74062D648FD62FCE72 \
  && sudo add-apt-repository \
  "deb http://ppa.launchpad.net/ubuntu-elisp/ppa/ubuntu $CODENAME main" \
  && sudo apt-get install -y \
  emacs-snapshot
```

## ELPA Mirror

To save time we will setup a local mirror for all the LISP files that we need as part of spacemacs.
Depending on your machine and internet connection it may take some time to download, configure and compile.

```
sudo git clone --depth 1 -b stable \
    https://github.com/ii/elpa-mirror \
    /usr/local/elpa-mirror
```

## Configuring Emacs

You will need to determine which use case below best fits your needs. It could save you a lot of time debugging your future setup.


### New user accounts

Configuring software repeatedly for a number of users is time consuming and possibly problematic.
To save time setting up Spacemacs for a number of users of the same computer we can use the `/etc/skel` folder.

#### Step 1

Download and link spacemacs inside the `/etc/skel` folder.

```
git clone --depth 1 -b stable --recurse-submodules \
    https://github.com/ii/spacemacs.git \
    /etc/skel/.emacs.d \
  && ln -s .emacs.d/private/local/dot-spacemacs/.spacemacs /etc/skel/.spacemacs
```

#### Step 2

As the `root` user we need to process `init.el` for future user accounts.

```
sudo -s
ln -sf /etc/skel/.emacs.d /root/.emacs.d \
  && ln -sf /root/.emacs.d/private/local/dot-spacemacs/.spacemacs /root/.spacemacs \
  && HOME=/root emacs --batch -l /root/.emacs.d/init.el \
  && rm /root/.emacs.d /root/.spacemacs \
  && rm /etc/skel/.emacs.d/elpa/gnupg/S.gpg-agent*
exit
```

### Setup for an the current user

As the current user has already been created we can't use the setup in the `/etc/skel` folder.
The configuration below will use the user's home folder.

```
git clone --depth 1 -b stable --recurse-submodules \
     https://github.com/ii/spacemacs.git \
     ~/.emacs.d \
   && ln -sf ~/.emacs.d/private/local/dot-spacemacs/.spacemacs ~/.spacemacs \
   && emacs --batch -l ~/.emacs.d/init.el]
```

## References

- [Github: ii/spacemacs (stable)](https://github.com/ii/spacemacs/tree/stable)
- [Magit: Forge](https://magit.vc/manual/forge/)
- [ob-tmate](https://gitlab.ii.coop/ii/tooling/ob-tmate)
