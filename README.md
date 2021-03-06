# proxypush
shell script for tellling another computer to retrieve your code &amp; push it

## terminology & background
**master server** is the SSH server you're writing your code on. **pusher server** is the SSH server you want to tell to retrieve your code & issue the version control push command on.

bother servers should have SSH daemons running, configured to accept passwordless public key authentication, with each other's public key copied to one another. this script is recommended for use over a LAN.

your code should have its version control remote destination configured (i.e. a git remote should be set).

## installation

on all machines, master or pusher:
- make sure you have the **scp** binary installed
- install the ppush script to ~/bin
- add a config file in ~/.ppush/ according to the machine role:
  - `~/.ppush/master`: master server config file defines pushers which specify pusher name, SSH hostname/IP, push command (defaults to `git push`), pull command (defaults to `git pull`), script parent directory (defaults to `$HOME/bin`)
  - `~/.ppush/pusher`: pusher server config file only defines master server's username and SSH hostname/IP, nothing else

## usage

From master server, cd to your repo root and run `$ ppush $PUSHER_NAME [$OPERATION $OPTIONS]`
- `$PUSHER_NAME` is the name of a pusher set in the master server's config file.
- `$OPERATION` is either "push" or "pull", defaults to "push"
- `$OPTIONS` is appended to the push command defined in the master config file that is executed on the pusher server.

## examples
`$ ppush enterprisegit`

`$ ppush enterprisegit push`

`$ ppush enterprisegit push "--set-upstream origin my-branch"`

`$ ppush enterprisegit pull`

`$ ppush enterprisegit pull "--rebase"`

## to-do/contribution suggestions

- error handling
- handle the config file text processing and conditional logic in a more robust scripting language instead of bash+awk
- use rsync instead of scp
- interactive features to kickstart pusher configuration & choosing a pusher
