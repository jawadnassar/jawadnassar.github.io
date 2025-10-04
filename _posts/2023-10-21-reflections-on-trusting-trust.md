---
layout: post
title: "Reflections on Trusting Trust"
date: 2023-10-21
categories: Blog
---

I want to start by saying that I personally use [Signal](https://www.signal.org/){:target="_blank"}, [ProtonMail](https://proton.me/){:target="_blank"}, [GrapheneOS](https://grapheneos.org/){:target="_blank"}, etc., and I find them to be very important projects to have in our modern society, where every tech company seems to invade our privacy in ways we can't fully comprehend.

However, I notice many people who recommend these solutions blindly, without giving it a second thought. I always think that if I were a shady government department, I would do the exact same thing: promote a privacy-focused, open-source solution [that many content creators endorse repeatedly](https://www.vice.com/en/article/n7b4gg/anom-phone-arcaneos-fbi-backdoor){:target="_blank"}, displaying open-source audits, reverse-engineering the apps, and sniffing network requests to prove their trustworthiness.

Here are some ways to challenge their security and privacy claims:

- Are they a non-profit organization? Or could they be bought tomorrow and legally change their algorithms?
- Is their code open source? Has it been audited by a trusted third party?
- Are they using [reproducible builds](https://reproducible-builds.org/){:target="_blank"}? Have you compiled the code locally and compared the hashed value with the online ones?
- Do you [trust the compilers in use](https://www.schneier.com/blog/archives/2015/03/how_the_cia_mig.html){:target="_blank"} and believe that apps developed with them won't be compromised?
- Do you trust the DNS serving you the open-source packages to build the app?
- Do you trust the website displaying the checksums and hash values, ensuring it hasn't been intercepted?
- Do you trust that there are no [backdoors in the cryptographic standards and toolkits in use](https://blog.cloudflare.com/how-the-nsa-may-have-put-a-backdoor-in-rsas-cryptography-a-technical-primer/){:target="_blank"}?
- Do you trust [the hardware](https://www.techrepublic.com/article/is-the-intel-management-engine-a-backdoor/){:target="_blank"} running those apps and believe it [hasn't been compromised itself](https://en.wikipedia.org/wiki/Evil_maid_attack){:target="_blank"}?

The list goes on, which brings me to a lecture published by Ken Thompson in August 1984, "[Reflections on Trusting Trust](https://www.cs.cmu.edu/~rdriley/487/papers/Thompson_1984_ReflectionsonTrustingTrust.pdf){:target="_blank"}," which highlights a crucial point: you can't trust code you didn't create entirely yourself. No amount of source-level verification or scrutiny can protect you from using untrusted code, especially if you don't trust the compiler itself.

I was recently watching Jack Ryan on Amazon Prime, and in one scene, it showed him contacting his assets via a random car dealership website, [obfuscated in plain sight](https://citizenlab.ca/2022/09/statement-on-the-fatal-flaws-found-in-a-defunct-cia-covert-communications-system/){:target="_blank"}. I believe this is how actual malicious users operate on the internet, hiding in plain sight but obfuscated enough that you won't second-guess it if you see it. We've witnessed many crimes happening over game chats and [Discord channels](https://www.cbsnews.com/news/cybercriminals-are-doing-big-business-in-the-gaming-chat-app-discord/){:target="_blank"} over the years.

I encourage you to continue using these technologies but with an awareness of the challenges and potential risks they may pose.

Let's wrap up with a couple of [xkcd's comics](https://xkcd.com){:target="_blank"} that sums it up

<img src="https://imgs.xkcd.com/comics/privacy_opinions.png">


<img src="https://imgs.xkcd.com/comics/security.png">


