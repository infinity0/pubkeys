How to use
==========

Please read `Terminology`_ if you are not familiar with the *precise* definitions of PKI-related terms. Also, "PGP" refers to the OpenPGP standard rather than the proprietary PGP tool.

This repository contains various public information about myself ("my info"), such as contact details, public key material, certification policy, etc. It is cryptographically signed using a PGP key (hopefully mine); you may interpret this signature as an implicit certificate that "my info is valid i.e. correct", and this key as a CA for my info.

Why not just a signed file? In any PKI (public-key infrastructure), there must be a way to distribute signed certificates and their revocations. In PGP's PKI, public keyservers provide this infrastructure; users poll these for updates on interested keys, including new certificates and revocations ("C" sigs). However, there is no such universal infrastructure for signed files ("S" sigs), and therefore PGP does not attempt to provide revocation capabilities for file signatures.

So, this repository is a custom revocation mechanism for my info. After clone, you should add a cron job to periodically run `git pull`, otherwise you will *miss revocations*. To verify the signature on each commit, use `git log --show-signature`. This is to avoid an attack where someone takes a signed file *out of the context* of this repository, and presents it to someone else as self-contained signed data without the explicit possiblity that it might have been revoked.

You should ensure that the key that signed this commit is *valid* for me - i.e. that you are not seeing a copy signed by a fake key that only *pretends* to be mine. You can do this by checking that there are (a few short, or many many long) trust paths *from* your key, *to* the (potentially fake) key that signed this commit. (The direction is important, exercise for the reader to figure out why.)

The following provides an easy-to-use non-cryptographic UI for this purpose:

http://pgp.cs.uu.nl/mk_path.cgi?FROM=YOUR_KEY&TO=MY_KEY

However, you are strongly encouraged to verify its claims cryptographically, by downloading the relevant keys and verifying the claimed signatures. I will update this part if someone releases an easy-to-use cryptographic tool for this.

Terminology
===========

The definitions here are a guide to understanding this document, and apply to this document only. Usage of the same terminology outside of this document may differ.

Being precise is vital here. Note that in particular, "trust" in PGP is a much more specific concept than "trust" in English; arguably the authors should have used or invented some other word.

sign
	To cryptographically commit to something. PGP actually has two types of signature, "S" (used to non-revocably sign a file) and "C" (used to revocably sign a certificate).
certificate
	A claim that some proposition is true.
validity
	The proposition that a key *is in fact* controlled by a given identity.
trust
	The proposition that the owner of a key signs certificates *truthfully and properly*, i.e. by personally verifying each certificate's proposition.

In colloquial terms,

- "A signs B's key" more precisely means:
- "A signs a validity certificate for B's key" which itself more precisely means:
- "A cryptographically commits that B's key is valid i.e. really is controlled by the given identities"

You may read `Validity and Trust <http://www.pgpi.org/doc/pgpintro/#p17>`_ for a more detailed discussion on these concepts. Note that I define "certificate" more generally.

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

Repository contents
===================

Ximin_Luo.pub
	My PGP public key, plus validity signatures from other people. You may also get this from public keyservers, which may be slightly more up-to-date.
Ximin_Luo.otr
	Fingerprints for my OTR keys, plus selected metadata.
