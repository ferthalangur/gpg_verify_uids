gpg_verify\_uids
===============

This is a Bash script to use after a GPG keysigning party. The script
generates an email message addressed to each distinct email address in
a UID of the GPG key entered on the command line. A random nonce is
generated (defaults to 20 random hex digits) and included in the
message. The messages are then emailed (optional), with a copy
retained.

Usage
-----

gpg\_verify\_keys [ -h | --help ] [ -v | --verbose ] [ -d | --debug
debuglvl ] [ -s | --send-email ] keyid


>-h or --help : display a help message and exit.

>-v or --verbose : increase the verbosity of the GPG program.
    
>-d or --debug __debuglvl__ : Set the debugging level to
>__debuglvl__. Generates debugging messages. 3 is the most
>extreme level to use. Debugging messages go to standard error. 

>-s or --send-mail : Tells the program to try to send out the
>nonce messages created. The default is to create the mail messages
>in the working directory (defaults to
>$HOME/gpg\_keysigning\_work/$keyid) 

>keyid : This is the 16 hex-digit key ID, optionally prefixed
>by __0x__

Side Effects
------------

The program will create several files in the working directory

* $keyid_fromkeyserver.asc -- ASCII-armored copy of the key, as
  pulled from a keyserver

* $keyid_cleaned.asc -- Left over from when this script was part of
  my personal process for keysigning. An ASCII-armored copy of the key
  with all signatures other than my own removed.

* $keyid_manifest -- Generated/appended by this script. A text file,
  fields separated by pipe (|) characters, with information about the
  key including nonce that was sent.

* $keyid_uids -- A list of UIDs parsed from the key. Used internally
  by the program.

* nonce\_to\_send\_to\_*.txt    -- Plaintext version of the message to be
  emailed (or was attempted to email) to the key recipient. Should be
  one per unique email message on $keyid

* nonce\_to\_send\_to\_*.txt.asc -- Signed (by your key) and encrypted
  (with $keyid) version of the message to be sent (or 

System Requirements
-------------------

* Bash

* GnuPG

Motivation
----------

So before signing a UID on someone's GPG public key, you should make a
best effort to verify the following:

* The identity of the person claiming ownership of the GPG key that
you will sign. You do this by examining their identifying documents,
in person.

* The fingerprint of the GPG public key that you will sign (usually
downloaded from a public keyserver) matches the fingerprint that the
purported key owner recited or otherwise presented to you, in person.

* That the purported key owner actually has control of the secret key
corresponding to the GPG public key that you will sign ... and
hopefully knows how to use the key.

* That the purported key owner has control of or access to each email
address in each UID on the key that you will sign.

The first two requirements are satisfied at the meeting between
keysigners. The third requirement can be satisfied by sending the
recipient a unique randomly-generated message (the nonce string)
encrypted with the public key and asking them to tell you what was in
that message, and the fourth requirement can be satisfied by sending
that encrypted message to the email address(es) of the key.

There are several tools available that automate the generation of
signatures and then send out the signed keys to the recipients, such
as [caff](https://wiki.debian.org/caff),
[Monkeysign](http://web.monkeysphere.info/monkeysign/),
and
[PIUS](http://www.phildev.net/pius/). These programs are all fine, but
they all sign the key and send it out __before__ verifying that the
keyowner controls the email address and the key. An argument can be
made that because the signed key UID is sent encrypted with the
purported owner's key that the latter two criteria are met because the
recipient can't use the signed key unless they control the email
address and the key. Whether this satisfies your own keysigning policy
or not is a personal decision. I'd rather do the verification before
signing the key.

Signing the key before verification also leaves you in an uncertain
state about whether to add the signed UIDs to your normal keyring and
trust the key and keys signed by it. Part of the purpose of signing
keys is so that you know whether to trust a key. Without the feedback
of whether they successfully decrypted the key, you don't know whether
you should trust the key you signed for that UID. I would rather
verify all four requirements before signing the key. That is what this
script is for ... generating and sending out the messages with a
random nonce and waiting for the expected response before any signing
takes place.

Bugs, Comments, Feedback
------------------------

See the file TODO.md for a list of known bugs and/or features needed.

I welcome bug reports, comments, feedback and discussion. You may
email me ... rbj_gpg {at} spotch.com . My gpg key fingerprint:

     pub   4096R/0x1D3AA60C86EF0FF0 2013-10-23 [expires: 2017-10-23]
     Key fingerprint = 9152 5B82 35DD 718F 3D52  CD4C 1D3A A60C 86EF 0FF0

I will also be blogging about GPG at http://rbjgpg.wordpress.com

License
-------
Copyright (C) 2013  Robert B. Jenson : rbj_gpg {at} spotch.com 

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation, Inc.,
51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
