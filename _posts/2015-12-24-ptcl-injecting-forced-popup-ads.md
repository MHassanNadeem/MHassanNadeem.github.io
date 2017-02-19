---
layout: post
title: Analysing PTCL's forced popup advertisements
description: 'PTCL the leading broadband provider in Pakistan is injecting pop-up ads into websites for users using its broadband services. I have analysed the ad and devised ways to block them.'
tags: ['ptcl', 'adblock']
---

PTCL the leading broadband provider in Pakistan is injecting pop-up ads into websites for users using its DSL and EVO services.

![Full PTCL Smart TV Ad]({{ site.baseurl }}/assets/images/ptcl-ad-full.png)

I encountered this atrocity for the first time yesterday, on a Pakistani website. I use adblock so seeing a gigantic image advertising PTCL's Smart TV app plastered over a website was unusual. But I did not think much of it, it is not unusual for Pakistani websites to skip advertising networks and deal directly with the advertisers, so some of the 'Desi' ads go undetected by adblockers. However I grew suspicious of foul play when I encountered the same ad on an internationally popular website.

Apparently the popups are timed, they only appear for a couple of minutes before vanishing into thin air. I was in the process of inspecting the website source with the intention of creating a rule for my adblock extension in chrome to filter these unpleasant (possibly illegal) ads that cover more than 1/3rd of the website, but the advert disappeared in a minute or two it took me to log into into my blogger account.

For now all that I have is the perhamalink for the image that appears in the ad:

`https://my.ptcl.net.pk/images/smart_tv_app.jpg`

Lucky these advertisements are not very frequent and an average internet user does not have to worry about them. I am an avid internet user especially during the holidays, I visited exactly 2628 unique websites in the last 48 hours and encountered the ad only 3 times. That is a meagre probability 0.00114. Any website you open there is only a chance of 0.1% that you will be greeted with this unpleasantness.

<div class="yellow-box">
<strong>Note: </strong>I have realized that approximately 70% of my internet traffic is over https. For the uninitiated https is the secure version of http, meaning https can not be tempered with. This means that PTCL can not inject their advertisements into websites that operate over https protocol. This also implies that the calculation I did above regarding the frequency of ad injection can vary greatly depending on your browsing habits.
</div>

Next time I see this pop-up I will make sure to copy the website source and hopefully also post a rule for adblock users to get rid of them.

### Update:
As promised, I dug a little deeper when I saw that ad again, this time on one my favourite website [lifehacker](http://lifehacker.com).

![PTCL Ad on lifehacker with source]({{ site.baseurl }}/assets/images/ptcl-ad-source.png)

I was able to get the minified version of javascript used by PTCL to inject ads. After removing the jquery library from the source code and running the minified script through [jsbeautifier](http://jsbeautifier.org) (for reindentation purposes), I have uploaded the source code on github for those interested. It is very hard to make sense of the code since it is minified to remove comments and variable names. Nevertheless you can get some important insights from it, like the url of ad and the duration of time for which the ad appears (100 seconds) etc.

Javascript that PTCL uses to inject ads:
<script src="https://gist.github.com/MHassanNadeem/6e2bcfec78206f3fddc6.js"></script>



### How to Block these advertisements
There are several ways you could block these ads.

1. Use https instead of http. You could use the popular extension [HTTPS everywhere](https://www.eff.org/Https-everywhere) to redirect you to https protocol where available. You will still see some ads but they will be limited to http websites only. This option is for people like my father, who like seeing non-intrusive ads.

2. Block the permalink to the ad image using adblocker extension.
`https://my.ptcl.net.pk/images/smart_tv_app.jpg`

3. Block scripts from the PTCL's static IP that serves the javascript that injects the ads. You will have to use an extension like *NoScript* or *ScriptSafe* for this.
`182.190.0.41`

