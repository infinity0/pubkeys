Contents
========

Ximin_Luo.pub
	My PGP public key, plus validity signatures from other people.
Ximin_Luo.otr
	Fingerprints for my OTR keys, plus selected metadata.

Terminology
===========

Being precise is vital here. Read `Validity and Trust <http://www.pgpi.org/doc/pgpintro/#p17>`_ if you haven't already.

In summary:

sign
	To cryptographically commit to something. PGP actually has two types of signature, "S" (used to non-revocably sign a file) and "C" (used to revocably sign a certificate).
certificate
	A claim that some proposition is true.
validity
	The proposition that a key *is in fact* associated with a particular UID.
trust
	The proposition that the owner of a key can (knows how to, and does) sign certificates in a correct way.

In colloquial terms,

- "A signs B's key" more precisely means:
- "A signs a validity certificate for B's key" which itself more precisely means:
- "A cryptographically commits that B's key is valid i.e. really does belong to the associated UIDs"

Certification policy
====================

This is effective as of 2013-06-26. Level-less and level-3 validity certs I signed before this date should be taken to mean the same as a level-2 validity cert indicated below. I signed no other type of certs before this date.

Validity
--------

Level 1
	I accepted an online request to sign this key. The request was authentic and valid (e.g. from a different key that I previously verified), but I wasn't able to verify the key offline.
Level 2
	I accepted an offline request to sign this key. The requestor was able to convince me of their identity. Typically this might mean that I've known them personally for a while, or that I saw some official ID, etc.
Level 3
	I accepted an offline request to sign this key. The requestor was able to convince me of their identity, and that the secret master key is stored offline.

GPG supports offline storage of just the secret master key, as follows::

	$ gpg --export-secret-keys $KEYID > /secure_disk/master_offline_secure
	$ gpg --export-secret-subkeys $KEYID > /secure_disk/daily_online_insecure
	$ gpg --delete-secret-key $KEYID
	$ gpg --import /secure_disk/daily_online_insecure
	$ rm /secure_disk/daily_online_insecure
	$ gpg -K $KEYID

For example::

	$ gpg -K 5FBBDBCE
	sec#  4096R/5FBBDBCE 2010-08-03 [expires: 2016-01-01]
	uid                  Ximin Luo <infinity0@gmx.com>
	ssb   4096R/C7A81D49 2010-08-03 [expires: 2016-01-01]
	ssb   4096R/91B24B90 2012-09-22 [expires: 2016-01-01]
	ssb   4096R/8F650B79 2013-06-14 [expires: 2016-01-01]

The "sec#" means the secret master key is not really there, but the secret subkeys are. Typically, subkeys are for day-to-day activities like encryption, signing files, authentication; whereas the master key lets you sign other people's keys and manage your own subkeys (the ultra-important stuff that must be super-secure).

Trust
-----

One can also sign trust certs for others, publicising the fact that you trust someone else to sign validity certs. This is rarely done in public; I haven't seen a tsign from anyone to anyone, let alone people that I personally know. Not only is the concept quite advanced, but there is some dispute about the usefulness and desireability of exposing this information publicly at all (and therefore to attackers).

I will update this section if the situation, or my (currently undefined) position, changes.
