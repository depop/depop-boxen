# Depop Boxen

## Prerequisites

* Make sure that you are running El Capitan (Yosemite *should* work, but not
  actively tested)
* Install the full Xcode from the Mac App Store, and then explicitly install
  the command line tools by running `xcode-select --install` from the terminal
* Ensure you your SSH keypair is installed on the laptop and that your user
  account has been bootstrapped with the correct UID and group membership
* Ensure that you have a github account, with 2FA enabled, and your public key
  installed into your account
* Ensure that your github account is added as a member of our organisation

## Running for the First Time

```bash
sudo mkdir -p /opt/boxen
sudo chown ${USER}:admin /opt/boxen
git clone git@github.com:depop/depop-boxen.git /opt/boxen/repo
cd /opt/boxen/repo
./script/boxen
```

For users without a bash or zsh config or a `~/.profile` file,
Boxen will create a shim for you that will work correctly.
If you do have a `~/.bashrc` or `~/.zshrc`, your shell will not use
`~/.profile` so you'll need to add a line like so at _the end of your config_:

``` sh
[ -f /opt/boxen/env.sh ] && source /opt/boxen/env.sh
```

Once your shell is ready, open a new tab/window in your Terminal and you should
be able to successfully run `boxen --env`.  If that runs cleanly, you're in
good shape.

## Removing install of Homebrew in /usr/local/bin for an already set-up Mac.

- Boxen installs its own version of Homebrew in `/opt/boxen/homebrew` which may
  conflict with a previous version installed in `/usr/local/bin`.
- You can list the brews installed in your previous version with:

        /usr/local/bin/brew list

- To uninstall your homebrew you can run the following uninstall script:

        ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"

- You can list the brews installed by Boxen with:

        /opt/boxen/homebrew/bin/brew list

- To generate a list of previous homebrew builds (to add to
  `modules/people/manifests/yourgithubuser.pp`) you can do:

        /usr/local/bin/brew list | (
            echo "package { ["
            while read line; do
                echo "           '$line',"
            done
            echo "          ]:"
            echo "          ensure => 'present',"
            echo "}"
        )

- One you have installed your previous brews with Boxen (run the `boxen` command
  after adding the resource generated above) you can [delete the previous
  homebrew](https://gist.github.com/mxcl/1173223)

## What you get by default

The following are provided by default:

* Homebrew
* Git
* Hub
* rbenv
* Nodejs 0.10
* Nodejs 0.12
* Full Disk Encryption requirement
* Ruby 2.0.0
* Ruby 2.1.8
* Ruby 2.2.4
* ack
* Findutils
* GNU tar

## Further information about boxen

See [the rest of README.boxen](README.boxen.md)

