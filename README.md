# bwfzf

**This is a Proof of Concept, use at own risk!**

**Consider the script in its current stage as an learning exercise.**

My main reasons for this was because I was looking for a nice looking, command
line based, interactive fuzzy finding password manger.

# Dependencies
* [Bitwarden cli](https://github.com/bitwarden/cli)
* [jq](https://github.com/stedolan/jq)
* [fzf](https://github.com/junegunn/fzf)
* keyctl

# What Works
* Fuzzy typing of a site name and displaying its jq output in a fzf preview

# TODO
* Actually  improve the security of the script. (or as much as reasonably
  possible for a bash script)
* bw_session caching is broken so you get asked for the password every time,
  even though your bw_session token is saved in your user keyring
* Improve this readme so it looks a bit more professional
* Implement all bitwardens command line functions into the script

There is more that I want to implement, as in the end I want this to be a
fully fledged command line front end to the bitwarden command line program.

# Notes
Due to the way systemd interacts with the kernel's key management facility you
would be able to add to the keyring, but not read or modify it without running
into `keyctl_read_alloc: Permission denied`.
This [bug](https://github.com/systemd/systemd/issues/5522) was first noticed
in systemd version 233, I have personally verified systemd version 239 is
still affected and systemd version 241 is not. I do not know if systemd
version 240 is affected (but I have included this version number in my
workaround until I can either confirm or deny if version 240 is affected).

To work around this bug, I link `user default session keyring` into `session
keyring` only for when I need to use keyctl functions that are affected by
this bug and then immediately unlink after its not needed, to minimise any
security implications that may arise.
