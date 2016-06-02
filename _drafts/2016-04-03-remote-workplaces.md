---
title: Making Remote Workplaces Work 
layout: post
---

In 1998, I worked for a company that made, yes, "made," small office/home office all-in-one routers. The world was just getting connected enough that people were considering operating out of satellite offices, instead of sinking dollars into commercial real estate. The hardware supported dial-up, xDSL and ISDN. I lived on a boat at the time (another story). So, I called AT&T and asked if they could install a DSL line to the dock. It's just another phone line, really. Nothing terribly special, but you do have to be close enough to the "demarc"{% sidenote "demarc" "'demarcation' point: Where the phone company's equipment ends and your house wiring begins." %} so that the signal was strong enough. I told them my address and said, "I live on a boat. Please write that down." 

The agent said, "Sure, you're close enough."
  
"Are you sure? Remember, I'm on a *boat* in a marina. You know, on the w-a-t-e-r."

"The provisioning software says that it's fine."

Several weeks later, a technician arrives and calls me on the boat. "I can't find your house, anywhere."

Sigh. "I'm on a boat. Come to gate G and I'll let you in."

"I'm installing DSL on a boat?"

"Apparently so."

"We can do this?!?"

`o_O`

Ironically, I got a new boss who said that I could not work from home at all, ever. He needed my presence every single day. I quit shortly thereafter.

I've been working remotely for long enough that I won't feel like an idiot sharing what I've learned. There's a lot of overlapping concerns here, but most importantly, working remote is a *skill*. It makes me cringe when people claim that distributed work forces "don't work." (Just like Rails doesn't scale. [trollface]) Of course they do, in particular environment, with specific team members who have the appropriate skills. As a founder, manager, leader or individual, you still have to choose your tools carefully. And, like every valuable skill, it is a *learned* skill. It takes *practice* to improve (and maintain). It just doesn't "happen" when you sprinkle magic process dust over it. On the other hand, none of this is rocket science, either. Every one of these skills is an incremental refinement that you can build on to make better.

## Why

Let's answer the existential question first. Why would you want to work this way, or why would you want your work force to work this way? The main categories are lifestyle and economics.

I don't want to waste my "life" stuck in transit. My last commute was 90 minutes each way. That's 3 hours of my life, trapped. I learned to make the best of it. I listened to a lot of podcasts.{% sidenote "podcasts" "The Moth, Ruby 5, 5 Minutes of Javascript, The Ruby Rogues Podcast, Serial, The Nerdette Podcast, The Changelog, The Shoptalk Podcast" %} Here's the case for a sane IT department: When I was a manager, I asked my IT people if I could get something to write reports, reviews, etc. on the train. They did they very easy math. If I could do the administrative work on BART so I could not miss billing 3 hours to our contract clients, it would pay for an iPad with the tricked out data plan _for a whole year_. Moral: Don't go cheap on your staff, they are worth $4 per minute.

[PacerPro](https://www.pacerpro.com), the company I am founding, is bootstrapped with private money. We can't afford useless extras like a fancy office with rented SOMA art on the walls. I also can't afford San Francisco salaries. Call me cheap, but I'd rather pay well in a place with a lower cost of living. It's a "science experiment." {% marginfigure "its-a-science-experiment" "assets/its-a-science-experiment.jpg" %} I can build an engineering organization that is every bit as good as any other, but with lower costs, lower attrition, and better product. Here's another economic reality that justifies remote workers: Jane, your key engineer has to move out of town for personal reasons. You don't want to give her up. Moral: You can find and keep key staff but thinking globally.

## Values

You need to instill some additional values into your organization. It provides the foundation that protects your company culture even when it is spread out. 

{% newthought "We are all peers." %} There is no remote versus local. That's bullshit. Anyone who thinks that remote workers are second class needs a serious attitude adjustment.{% marginfigure "hammer" "assets/hammer.jpg" %} 

{% newthought "Emphasize collaboration over individuals." %} We learn from each other. That's valuable. No more rock star ninja warrior prima donnas. 

{% newthought "Not for everyone." %} Interestingly enough, and perhaps counter to this entire rant, This is not for everyone. That's fine, just don't sign up for it if it isn't. I make this very clear early in the interview process.

## Hardware

Okay, let's get into the fun stuff; the toys, I mean, the tools: Get the highest end processor, with lots of screen real estate and as much memory as you can afford. Note: If memory is upgradeable, you can let Moore's Law pay for memory upgrades in future years to keep the dev platform running even longer. I'm torn about laptops (and it seems that I'm in a diminishing minority), especially if you've got a high-end MacBook Pro. They seem powerful enough to drive two 27" monitors. I have both, btw. At *least* two 27" monitors. Human beings have evolved to be excellent at spatial reasoning, but we're not so good at temporal reasoning. Switching from screen to screen causes a cognitive context shift that can slow down your work. Get extended warranties or service contracts, especially on laptops. 

You want to establish a "presence" for both yourself and your remote environment. You'll need a decent camera, microphone and screen for rendering a face. Apple products come with excellent defaults. They are inexpensive, and the products improve every year. Some tried and true: Snowballs, Snowflakes, boom mics, tablets. Comfortable, *really* comfortable head phones. You will be wearing these things all day, don't go cheap. You may have to experiment. Have a "sample wall" at the home office for people to borrow and try out before they settle on something. Tastes and environments change, so allow for change.

Wired, gigabit ethernet from the workstation to the ISPs modem. WiFi is great, but it is way slower than wired. When you're driving compilers, tests, browsers, virtual machines, voice, full motion video *and* sharing a screen, you're going to need the bandwidth. I use [speedtest.net](http://speedtest.net) to check my network speed. See the table, below for nominal speeds using different media. If you can't run a wire to the modem/router, us an Ethernet-over-A/C adapter such as the one from [TP-Link -- need link](https://amazon.com/...). It's works great, truly plug and play, and feels like line speed. One more thing to note, *hosting* a screen share uses more upload than download network packets, the *clients* use the opposite, more down than up. It may be in your best interest to switch the host/client roles if your network pipes are asymmetric (or simply flakey that day).

--- Photos of various remote pairing environments ---

## Infrastructure

- There's no IT department, so spend
- There's no office manager, so spend on snacks, coffee
- Backup channels, out-of-band control channels, redundancy in depth
- Be cognizant of asymmetric bandwidth: up is often much smaller than down. Screenhero host is mostly up, client is mostly down.
- Network monitoring tools
- Configure scripts for the dev machine, so everyone has the same environment

Things are going to go wrong, on a daily basis. By having fallbacks in place and procedures for what to do next, it is easier to keep the rhythm going. My CEO doesn't listen to me and complains when things don't go well. [sigh] Just keep going. Sometimes you just have to split the pair. Be ready for that, too.

- Voice: SoCoCo, Screenhero, Cell, Google Hangout, join.me appear.in
- Video: SoCoCo, Hangout, join.me, Facetime

## Space

- Desk
- Chair
- Good lighting
- Good ventilation
- Shoe organizer on the door [pic]

    I was surprised at how much heat an iMac, Thunderbolt Display, MBP, 44" LED and two cats can put out.
    
- A work space separate from home-life space

    Locked door, Red light, Do not disturb sign

## Human Scale

- An individual contributor's time is worth $4 per minute. Don't waste it, ever.
- Exercise, take a longer lunch. No one takes ping pong breaks, so it's a wash.
- Suit up (uniforms make you smarter [http://www.sciencedirect.com/science/article/pii/S0022103112000200] [http://spp.sagepub.com/content/6/6/661])
- We're used to 3-dimensional visual, intangible cues, especially facial cues.
- We suck at text-only. We're a little better at audio.
- It's all about context.
- Human beings are very good at spatial reasoning, and not so good at temporal. Put everything, and everyone in a "concrete" space. You have to be able to point at things.
- Pairing help keeps culture and worth ethic
- Get out of the "office." Meet-ups and other person to person, in person, encounters on a weekly basis.
- My pairing heuristic: Default stance. There has to be a reason *not to pair*. Bugs are often singleable. Reading. Customer support. Spikes.
- Working remote can work, but there's a cost to making it work. It doesn't come for free. The tradeoff, I belive is worth it.
- Pet friendly!
- Child friendly!


## Process

- Habit and ritual keeps things moving at a steady pace.
- There's work time, and everything else isn't
- Sound check every morning
- Leave the mic/camera on; a window into the other's office
- 4-6 weeks indoctrination with entire team. We get to learn each other's foibles, speach inflections, level of sarcasm.
- Quarterly gatherings (nod the DNSimple blog post)
- Annual all hands, with families, off-site
- Assistants for administrivia
- Morning katas (dojo)
- Same key maps, everywhere
- Code linters for style consistency
- Continuous integration, deployment pipelines

## Collaboration tools

- SoCoCo
- ScreenHero
- Slack (information radiator for various channels more than chat)
- Zendesk for internal tasks, too (@ask)
- Pivotal Tracker
- Stickies (Retrium?)
- Collaborative drawing app?

## Financial

- Not paying for office space, office manager, parking; that does not mean they are free!