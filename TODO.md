TODO
====

Bugs
----
* When a key has multiple UIDs with the same email address, we should
  only send a message once. We are validating email addresses and keys,
  not names.

* It seems to behave a little bit differently with GnuPG 1.4.14 and
  1.5.3, which I discovered after accidentally downloading the
  MacPorts version of GPG which is older than what comes with MacGPG
  2.0.22.

Tidy Up
-------
* Parametrize the name of the event and your signature lines. Maybe
  it's time for a configuration file.


Features
--------
* Find a better way to specify the the default message and other
  parameters ... From a template file in the working directory, if available?

* Take multiple key IDs on the command line.

* ?? Allow the use of 8 hex byte key IDs instead of exclusively 16 hex
  byte key IDs? I'm not sure this is actually a good idea, given that
  it is possible to create key ID collisions in the low-order 8 bytes.

* Deal with multiple secret keys in the keyring ... select which
  one(s) to use?

* Command-line switch to skip fetching the key from a keyserver. Error
  out if it is already in the keyring.

* Rewrite the whole thing in Python? It's getting hard to read and debug a 400-line
  Bash script.
