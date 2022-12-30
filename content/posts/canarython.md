---
title: "Canarython - A tool for creating Canaries"
date: 2022-12-29T14:11:52-06:00
draft: true
categories:
  - Tools
tags:
  - tools
  - python
---

## Premis
While driving to a friends house, listening to a podcast, I got to thinking about warrant canaries. Warrant canaries, for the unfamiliar, are a proposed tool for notifying users that a site has been served a NSL or similar government order by negative action. They are often cryptographically signed to ensure that only an authorized person can update them, include a number of claims about the security of the site, and have an expiry date. For example, DEFCON currently has the following as their canary.

```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

Transparency Report
2022 November 26

As of the date above, DEF CON has not received any National
Security Letters or any orders under the Foreign Intelligence
Surveillance Act. DEF CON does not have any knowledge of any
search orders that have been issued or carried out. No search
warrents, discovery motions or gag orders either.

DEF CON will update this statement bi-monthly with any changes.
The next one should be on or before 2023 February 01

For more information around what inspired this statement check
out https://www.canarywatch.org/ (It's no longer active, but
still interesting)

The Dark Tangent

This is signed with my GPG key, please verify it here:
https://www.defcon.org/html/links/dtangent.html
https://keys.openpgp.org/search?q=dtangent%40defcon.org
-----BEGIN PGP SIGNATURE-----

iLgEARMKAB0WIQRVWpTk4m5wVSL0dwH0NMTF8gVVZAUCY4GHQwAKCRD0NMTF8gVV
ZHEdAgQLy2HiGQDCE8F4u6Mn/mstHdOQUkCRTO9Maa0fulGUK/htPsMnShRBCjW5
rVdkvV6f8VqacNyPRi8CB6AllDeoWgIJAe5hzgt4SbI+A8Sd56KnzOh+e2l9aqwB
+CQrOzxtP8CdvmzlU9wzh6bBz54x4pH11pN8a81faFccPtzgNa7u4Z89
=ir2B
-----END PGP SIGNATURE-----
```
Now, anyone can go to [DEFCON's website](https://defcon.org/html/links/dc-canary.html), download this message, and verify that it's authentic.
```
$ gpg --verify dc.txt
gpg: Signature made Fri 25 Nov 2022 09:25:55 PM CST
gpg:                using ECDSA key 555A94E4E26E705522F47701F434C4C5F2055564
gpg: Good signature from "The Dark Tangent (ECC p521 2021-2024) <dtangent@defcon.org>" [unknown]
```
Now, this tells us that DT (or someone with his private key and password) signed this message, and that the _system clock_ on the machine generating the signature said it was 9:25 PM on a Friday when he did so. This leads us to the first major concern with such canaries, and the one this tool attempts to simplify: timing.

## So.... What time is it again?
One potential attack against the canary system is pre-creating the messages with dates in the future. For example, telling the system clock that it's actually next month, generating a canary, and saving it to be uploaded later. A common way to prevent this attack is to include the current headlines from various news sources into the signature. The reasoning is that no attacker could know the headlines on CNN/BBC/Fox/Las Vegas News 3 months ahead of time. Take these two messages for example:

```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

It is currently December 29th, 2022
-----BEGIN PGP SIGNATURE-----

iQEzBAEBCgAdFiEE4m1IswjHwcOc08PmhrNdl4nr5KUFAmOt+9gACgkQhrNdl4nr
5KWi7ggAnuMEH4TucL6TwkqCMJZm/NKfCdbhOCY06JE7wks14+cqr7dtej35FY46
8XHjft0jjyF5bEHRZfdva2QcEQcnsvRDaA6HTrWXF+dFvirSK9P9A5/CYaH1fX6y
iL1bQ+VMf6izCAwqIsz2N4hus0jpeJuyIq4vMEP970LoKC6Us+9XYyTvbjsRRR0R
N8JlIBCX+CF0WTVByCQCMwXULlK/22j/PTrIKKteriY/xagSsV/w37tzQI7ioriY
erMTE377tIJnPMMtYAtda4p7pk6vcWdVTmmwrCWSL9u8tc25E8DyUNm9T2NbWLbL
oybWn+flSNbgeNnytiq++BtS0A0Y3w==
=Uy3G
-----END PGP SIGNATURE-----
```
VS
```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

It is currently December 29th, 2022.

The current headline on https://news3lv.com is
Ice Cube, Too $hort, Joe Satriani set to headline Raiders game day entertainment Sunday
-----BEGIN PGP SIGNATURE-----

iQEzBAEBCgAdFiEE4m1IswjHwcOc08PmhrNdl4nr5KUFAmOt/B8ACgkQhrNdl4nr
5KWEHQgArVe/JZs5cmCQ2cBPQJjhD1qSkR0lL+kRsnYIA1Iie/hIjBCpjCmR1LdB
ZVGADDj/OIMZwMt1G5fEym74Zcdg0V1wXtSFiudBz+//cth6z/f4uWR2UurtVidW
PqHkHT0tyG9Twrvg95IQkq/3hSsdpNHKXFtCHJGhPgVFx3nOCj5zvtEyEmE2KJ/p
eDkJ8m2o9unUicUqJLVTmwolpL1HNLBLLn8aM4sAwTLkhNCAg/WTkgPArN7FWn5H
KF7S3WyffwxZAum84gakabMs73M/pmCuwQ8C5Q8EaNcJ6+MRp7Tco0xlHbAj1bkK
V04xx9XZ3ed0ei+micHDljOZF7stSg==
=nm/H
-----END PGP SIGNATURE-----
```

Both messages are cryptographically signed with a valid key. `gpg --verify` will give you the OK on both of them, but the first one could (theoretically) be lying about the date. The second one would require either [future sight](https://scryfall.com/card/mh1/53/future-sight), or Las Vegas News 3 to collaborate with the attacker. Because the local medium I gave $20 to to pick a lottery number for me ended up losing me $10 in powerball tickets, I'm going to assume that [clairvoyance](https://scryfall.com/card/ice/63/clairvoyance) is an attack vector we can ignore. That leaves news sites cooperating with the attacker. This isn't unreasonable if you're a DEFCON, Signal, Tor, ect., and nation-states are part of your threat model. The solution? Include multiple news sources from different jurisdictions. That's where my tool comes in.

## Canarython
Canarython is a portmanteau of Canary and Python, because it's a Python tool for generating canaries. It pulls the latest RSS feeds from four sources by default, with the ability to add more. It uses the GpgME library to securely generate GPG signed messages, which means that it never directly sees your GPG private key or password. It also means that I'm not rolling my own crypto, because I was never good enough at math for that. You can install it right now, simply by running `pip install canarython`. Running it will give you something that looks a little like this:


```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

The current date is Dec 29, 2022.
This is a sample proof for https://thetoddluc.io

====================================
Current headlines as of 2022-12-29T14:58:22.247256:

Latest from 'CNN.com - RSS Channel - HP Hero' (http://rss.cnn.com/rss/cnn_topstories.rss)
Last Updated Thu, 29 Dec 2022 20:41:11 GMT
He was the sport's first global icon and the only three-time World Cup winner
https://www.cnn.com/2022/12/29/football/brazil-pele-soccer-died-intl-latam-spt/index.html

Pelé's life in pictures
https://www.cnn.com/2022/12/29/football/gallery/pele-life-in-pictures/index.html

Look back on Brazilian soccer legend Pelé's life and career
https://www.cnn.com/videos/sports/2022/12/29/pele-brazil-soccer-icon-dead-riddell-obit-pkg-vpx.cnn

Latest from 'Latest & Breaking News on Fox News' (https://moxie.foxnews.com/google-publisher/latest.xml)
Last Updated Thu, 29 Dec 2022 20:48:23 GMT
Neymar Jr., Cristiano Ronaldo among those grieving loss of Brazilian soccer star Pelé
https://www.foxnews.com/sports/neymar-jr-cristiano-ronaldo-among-those-grieving-loss-brazilian-star-pele

New Orleans sees nine shootings in one day as homicide rate climbs to levels not seen in decades
https://www.foxnews.com/us/new-orleans-sees-nine-shootings-one-day-homicide-rate-climbs-levels-not-seen-decades

In WV, Harpers Ferry National Historical Park requires masks again due to rise in COVID cases
https://www.foxnews.com/us/wv-harpers-ferry-national-historical-park-requires-masks-again-due-rise-covid-cases

Latest from 'BBC News - World' (http://feeds.bbci.co.uk/news/world/rss.xml)
Last Updated Thu, 29 Dec 2022 20:22:19 GMT
Pele: Brazil football legend dies aged 82
https://www.bbc.co.uk/sport/football/42751517?at_medium=RSS&at_campaign=KARANGA

Russia fires dozens of missiles at Ukrainian cities
https://www.bbc.co.uk/news/world-europe-64114784?at_medium=RSS&at_campaign=KARANGA

EU calls screening of travellers from China unjustified
https://www.bbc.co.uk/news/world-europe-64119080?at_medium=RSS&at_campaign=KARANGA

Latest from 'The Proton Blog' (https://proton.me/blog/feed)
Last Updated Thu, 29 Dec 2022 12:24:01 GMT
Is Proton Mail safe?
https://proton.me/blog/is-proton-mail-safe

8 tips for secure file sharing
https://proton.me/blog/tips-for-secure-file-sharing

Fighting abuse while preserving privacy
https://proton.me/blog/proton-mail-abuse

====================================
Generated with canarython version 0.0.1
====================================
-----BEGIN PGP SIGNATURE-----

iQEzBAEBCgAdFiEE4m1IswjHwcOc08PmhrNdl4nr5KUFAmOt/3AACgkQhrNdl4nr
5KXYPAgAkFf3tDtQEjJ41EKkR8wLFoCtjnRd2BFt4LQF9St5u/1HzeaQor1eFbNL
njIBuVg79TONjgTxormTFRe1FCEtA2xHmPt4q+P+4s2iAyw3Ox418i8aSX25NeZ7
XWium1Am74fRazn9r7uzDvlam33rXbUnNSWsPOvXi9yv05StYKzm+WgXljj3H/Lg
gjnIcWtTIVLgp8mdFEoRB80pfL0BO+ktEuVkD36TuD7jNomaK6FLKhGjk/eJagyt
DrQ9nvLrXOH+NRnh+FyZyuJjrlqutccptZ35a4FgNjBbYlNJ0fYGF4FD9lA7JQYD
0Rrijhrtr3eb8SZpTOYVNaQM+YpS+w==
=2r0k
-----END PGP SIGNATURE-----
```

## FAQ
#### Is this useful?
No, probably not. It's more of a novel idea I had than something I expect to see any sort of wild adoption.

#### Can't the Gov simply order you to update your canary?
IANAL. Seriously, that will depend on local laws, law enforcement's willingness to follow said laws, and a hundred other things. Ask your lawyer if you find yourself in this situation, not a random guy on the internet.

#### How does this protect against the $5 wrench attack?
![https://xkcd.com/538/](https://imgs.xkcd.com/comics/security.png)
It doesn't. Your best bet is probably a password manager with 2FA, a good password, and hoping Batman comes to save you.

#### Why those four sources?
It's unlikely that anyone, government or otherwise, could get Fox News, CNN, BBC, and Proton to all agree to an op. Proton is included because it's trusted by the community, and not owned by a Five Eyes national. The other three publish frequently, and will almost certainly be available at any point in time.