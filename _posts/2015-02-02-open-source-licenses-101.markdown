---
layout: post
title:  "Open Source Licenses 101"
date:   2015-02-02 20:11:00
category: taft
author: gotoplanb
---

## What is an open source license?

Simply an open-source license must comply with the criteria of the [Open Source Initative](http://opensource.org/osd), which is a [non-profit 501(c)3 registered in California](http://opensource.org/about). The critera (and short definitions) are:

1. No fees for redistribution
2. Must provide source code
3. Allows for derived works
4. Integrity of the author's source instead of so much modification original is unrecognizable
5. No discrimation
6. No restrictions on why the source is used
7. License details are included with the source
8. Source must me distributable by itself
9. License cannot restrict other software
10. Licenses cannot dictate specific technologies

## Dislaimer: I'm not your lawyer

There are a ton of details and nuance, so please do not assume anything I say here to be in the galaxy of legal advice. If you have lots of time or money at stake, you should probably talk with an attorney. I'm going to try and give enough detail to help you narrow your reading to a couple licenses and go from there to determine which is best for you.

## Which license should I use?

### Apache v2.0 

This is a pretty flexible license that requires you to keep all notices of contribution and attribution but very few other restrictions.

The **licensor** owns the copyright and grants the license.

A **contribution** is any work submitted to the licensor that hasn't been marked as _not a contribution_ in some way. This means a pull request is a contribution. A closed pull request has been conspiculously marked as _not a contribution_. If the pull request is merged by the licensor, then you are officially a **contributor**.

A fork of a GitHub repository is an example of a **derivative work** with the original repository being the **work**.

The licensor and each contributor, "hereby grants to You a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable copyright license to reproduce, prepare Derivative Works of, publicly display, publicly perform, sublicense, and distribute the Work and such Derivative Works in Source or Object form" ([OSI](http://opensource.org/licenses/Apache-2.0), 2015). You can even submit a patent for derivative works you create. However, if you file some lawsuit against the licensor of the original work, then you lose the right to use the original work. Plus you'd be a huge jerk. 

You can redistribute the source almost anyway you want as long as you:

1. Include the license
2. Make obvious notice of any modified files
3. Retain all notices of attribution and contribution
4. Keep the content of a NOTICE text file

You are assuming all liability and expecting zero warranty from the licensor. 

### MIT

This is the most open of the major open source licenses. The only requirement is to include the following notice wherever the full original work or any substantial parts of the original work might have be extracted:

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

There are no warranties. This is the #yolo of open source licenses.

### GPLv3

This one is [really long](http://opensource.org/licenses/GPL-3.0). 

You can run the code as much as you want and for whatever reasons you want. You can distribute the unmodified source and even charge for it. You can also charge for warranties and services for running and maintaining the unmodified source. 

You can extend the functionality as long as you aren't modifying the original work. This is one reason you see a common WordPress core and then a robust plguin community to extend core. 

If you modify the original work, then you have to license all of your additional changes under this GPLv3 license as well. The key definiton is regarding a the **aggregate**:

> A compilation of a covered work with other separate and independent works, which are not by their nature extensions of the covered work, and which are not combined with it such as to form a larger program, in or on a volume of a storage or distribution medium, is called an “aggregate” if the compilation and its resulting copyright are not used to limit the access or legal rights of the compilation's users beyond what the individual works permit. Inclusion of a covered work in an aggregate does not cause this License to apply to the other parts of the aggregate. ([OSI](http://opensource.org/licenses/GPL-3.0), 2015)

You can include GPLv3 libraries in aggregate work without automatically triggers the GPLv3 license to cover your entire work _is you have not modified the original work in any way at all_. This is gray area and easy to trample. You should either have very strict tracking and auditing of how you use GPLv3 libraries in other projects or avoid these licenses altogether. 

At Mobiquity, we prefer to avoid GPLv3 for client project because we don't want to accidentally modified the original work and as a side effect trigger a client project to forcefully assume a GPLv3 license as well.

### MPLv2

The Mozilla Public License is somewhere between MIT and GPLv3. Here's the crux regarding distribution in a larger project:

> You may create and distribute a Larger Work under terms of Your choice, provided that You also comply with the requirements of this License for the Covered Software. If the Larger Work is a combination of Covered Software with a work governed by one or more Secondary Licenses, and the Covered Software is not Incompatible With Secondary Licenses, this License permits You to additionally distribute such Covered Software under the terms of such Secondary License(s), so that the recipient of the Larger Work may, at their option, further distribute the Covered Software under the terms of either this License or such Secondary License(s).

You can modify MPL works in your larger work without the MPL license having to bubble up to the rest of the larger work like in GPLv3.

Also worth noting that if you break some rule of this license, you can fix your violations and then be back in the good graces of the licensor. 

### More licenses

There are many more open source licenses. Here's a [list of other popular licenses](http://opensource.org/licenses/).

### Contribute

I'd love some help writing up summaries of other licenses as well as providing examples of software we use that falls under each license type along with pros and cons of that license type.