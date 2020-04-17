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
  - `~/.ppush/master`: master server config file defines pushers which specify pusher name, SSH hostname/IP, push command (defaults to `git push`), script parent directory (defaults to `$HOME/bin`)
  - `~/.ppush/pusher`: pusher server config file only defines master server's username and SSH hostname/IP, nothing else

## usage

From master server, run `$ ppush $PUSHER_NAME [$REPO_ROOT $OPTIONS]`
- `$PUSHER_NAME` is the name of a pusher set in the master server's config file.
- `$REPO_ROOT` is the absolute path on the master server to the root directory of the repo you want to push. Falls back to $PWD's git root if not set.
- `$OPTIONS` is appended to the push command defined in the master config file that is executed on the pusher server.

## to-do/contribution suggestions

i'll be working on this myself but feel free to open a PR (: 
- handle the text processing and conditional logic in a more robust scripting language instead of bash+awk
- use rsync instead of scp
- interactive features to kickstart pusher configuration & choosing a pusher
