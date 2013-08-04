---
layout: post
title: "PGP for Mac Mail"
alias: /2005/08/pgp-for-mac-mail.html
categories:
---
If you've ever needed (perhaps need is too strong a word, how about wanted) to digitally sign -- or encrypt for that matter -- your emails from within the Mac Mail client, it's pretty simple. Even though there are [plenty](http://www.bretschneidernet.de/tips/secmua.html) of mail applications that support PGP, I've grown fond of Mail.app so this morning -- for no real good reason -- I installed a few plugins, etc. and was up and running in literally 5 mins (ok 10 mins -- I didn't have a clue what kind of signature I should use).

First go and get [GPGMail](http://www.sente.ch/software/GPGMail/English.lproj/GPGMail.html). The download is a `.DMG` file and the instructions on the web site are easy to follow. In addition to GPGMail, you'll also need [GNU PG for the Mac](http://macgpg.sourceforge.net/) (GPGMac and GPG Keychain Access as a minimum).

I also found I needed to go into the Preferences&gt;PGP&gt;Composing and switch on "By default, use OpenPGP/MIME". This allows signed messages to be sent using MIME rather than the old-school format which surrounds the text in the signature and makes it look as though your email was sent through some kind of full-on nerdifier -- a bit scary for mum.

Now that I have the ability to sign emails I'll just, well, probably never use it really -- quite frankly, I can't imagine anyone being bothered to impersonate me in an email -- but at least I _feel_ "safer" LOL.

So anyway, here is my PGP key (valid until August 22, 2006) for anyone who cares:

Key ID: 0xD56C95B07EA87B26<br />Key Type: RSA<br />Expires: 2006-08-23<br />Key Size: 2048<br />Fingerprint: AF94 4D3E F229 1A40 4F79  17DD D56C 95B0 7EA8 7B26<br />UserID: Simon Harris &lt;haruki_zaemon@mac.com&gt;

``` text
- -- --BEGIN PGP PUBLIC KEY BLOCK -- ---Version: GnuPG v1.4.1 (Darwin)mQELBEMLpCoBCAD1qKqEmXhnbg8unu7n3wL8d8qyqu9M7iIhLMn6ZIxHPe91vXCi3NxCu9J5p+nenKcwyPV4i/TA4G6lr6hGAv6yOkCKXg/kMX/oPoqVALBnzz+NNXXOv9xZ1/DnQuyMXj/JP3u/rMrGErO5BEq+RtxQ3BwdbF8TsEktaaIrPPG/ZRWlSSCnFhTbS4F+J8bNNgmB2M8AYbk5F2FJc68ikSDDKz28F/pF1Xal+O/s3nVtXgQkf6o5sV/YnIGogDP419XQ8C9tb4dqoe8oR8v25g6umNEDcUMtOS3Utyds2q7mfdlczjAuYpixCLk8QMbEfsaQqGJU7b7YjK9iaVJqS/Z3AAYptC1TaW1vbiBIYXJyaXMgPHNpbW9uQHJlZGhpbGxjb25zdWx0aW5nLmNvbS5hdT6JATcEEwECACEFAkMLpCoFCQHf4gAGCwkIBwMCAxUCAwMWAgECHgECF4AACgkQ1WyVsH6oeyZa4gf9Fus1SDOwBYG6RLiQomXWhfHibGZnrssw9ECemI6I81kgKC6rd+srxbiKit09TIMIUzZ/oecNVtxg80rgbYsOT4EGniq/As5c6xfYNcxwgGW00Xf6txvMGCRzkierHWlE0KajOW94AnuAtzHC9vsPVxTjt4dM08IHWS9VeuqGq8ULokcHh9uF4e24s/maJFUGikYm2dACKS8vNPImFHUcV17pTm4gptag4bm1+KmFWS1wnUS/I1jfmUc+xJOrXFRadkEbiYJEi1aaPyvgyvXzupia6RDyMvPfUILZLO1L4be/KGf6R6jdhE4T+9U4dNHbGnukIqkIQLRxgNqhtklgkA===2DOr -- ---END PGP PUBLIC KEY BLOCK -- ---
```

Of course it occurs to me that someone could also spoof my blog and change the public key here but again, I'm thinking people have better things to waste their valuable time on like say, [RAD](http://www-306.ibm.com/software/awdtools/developer/application/index.html) for example ;-).
