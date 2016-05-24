---
title: Making Remote Workplaces Work 
layout: post
---

In 1998, I worked for a company that made, yes, "made," small office/home office all-in-one routers. The world was just getting connected enough that people were considering operating out of satellite offices, instead of sinking dollars into commercial offices. The hardware supported dial-up, xDSL and ISDN. I lived on a boat at the time (another story). So, I called AT&T and asked if they could install a DSL line to the dock. It's just another phone line, really. Nothing terribly special, but you do have to be close enough to the "demarc" so that the signal was strong enough. I told them my address and said, "I live on a boat. Please write that down." 

The agent said, "Sure, you're close enough."
  
"Are you sure? Remember, I'm on a *boat* in a marina. You know, on the w-a-t-e-r."

"The provisioning software says that it's fine."

Several weeks later, a technician arrives and phones my cell phone. "I can't find your house, anywhere."

Sigh. "I'm on a boat. Come to gate G and I'll let you in."

"I'm installing DSL on a boat?"

"Apparently so."

"We can do this?!?"

""

Ironically, I got a new boss who said that I could not work from home at all, ever. He needed my presence every single day. I quit shortly thereafter.

I've been working remotely for long enough that I won't feel like an idiot sharing what I've learned. There's a lot of overlapping concerns here, but most importantly, working remote is a *skill*. It makes me cringe when people claim that distributed work forces "don't work." (Just like Rails doesn't scale. [trollface]) Of course they do, in particular environment, with specific team members who have the appropriate skills. As a found, manager, leader or individual, you still have to choose your tools carefully. And, like every valuable skill, it is a *learned* skill. It takes *practice* to improve (or just maintain). It just doesn't "happen" when you sprinkle magic process dust over it. On the other hand, none of this is rocket science, either. Every one of these skills is an incremental refinement that you can build on to make better.

## Why

Let's answer the existential question first. Why would you want to work this way, or why would you want your work force to work this way? The main categories are lifestyle and economics.

### Lifestyle

I don't want to spend my "life" stuck in transit. My last commute was 90 minutes each way. That's 3 hours of my life, trapped. I learned to make the best of it. Here's the case for a sane IT department: I was a manager. I asked my IT people if I could get something to write reports, reviews, etc. on the train. They did they very easy math. If I could do the administrative work on BART so I could bill our contract clients an extra 3 hours, it would pay for the iPad with the tricked out data plan _for a whole year_. Moral: Don't go cheap on your staff, they are worth $4 per minute.

PacerPro, the company I am founding is bootstrapped with private money. We can't afford useless extras like a fancy office with rented SOMA art on the walls. I also can't afford San Francisco salaries. Call me cheap, but I'd rather pay well in a place with a lower cost of living. It's a "science experiment". [Insert Back to the Future Part 3 image, here] I can build an engineering organization that is every bit as good as any other, but with lower costs, lower attrition, and better product. Here's another economic reality that justifies remote workers: Jane, your key engineer has to move out of town for personal reasons. You don't want to give her up. Moral: You can find and keep key staff but thinking globally.

## Values

You need to instill some values into your organization. It provides the foundation that protects your company culture. We are all peers. There is no remote versus local. That's bullshit. Anyone who thinks that remote workers are second class needs a serious attitude adjustment [Hammer]. Emphasize collaboration over individuals (rock stars). We learn from each other. That's valuable. Interestingly enough, and perhaps counter to this entire rant, This is not for everyone. That's fine, just don't sign up for it if it isn't. I make this very clear during the early interview process. I will fire you for not showing up.

## Hardware

Okay, let's get into the fun stuff; the toys, I mean, the tools: Get the highest end processor, with lots of screen real estate and as much memory as you can afford. Note: If memory is upgradeable, you can let Moore's Law pay for memory upgrades in future years to keep the dev platform running even longer. I'm torn about laptops (and it seems that I'm in a diminishing minority), especially if you've got a high-end MacBook Pro. They seem powerful enough to drive two 27" monitors. I have both, btw. At *least* two 27" monitors. Human beings have evolved to be excellent at spatial reasoning, but we're not so good at temporal reasoning. Switching from screen to screen causes a cognitive context shift that can slow down your work. 

You want to establish a "presence" for both yourself and your remote environment. You'll need a decent camera, microphone and screen for rendering a face. Apple products come with excellent defaults. They are inexpensive, and the products improve every year. Some tried and true: Snowballs, Snowflakes, boom mics, tablets. Comfortable, *really* comfortable head phones. You will be wearing these things all day, don't go cheap. You may have to experiment. Have a sample "wall" at the home office for people to borrow and try out before they settle on something. Tastes and environments change, so allow for change.

- High-end processors, lots of memory
- I'm torn about laptops, especially if you've got a high-end MBP. I have both, btw.
- At *least* two monitors
- *Wired,* gigabit ethernet (Ethernet over A/C line, e.g. TP-Link)
- Extended warranty
- Camera, a screen for the "face", iPad, Android Tablet
- Headphones, directional mic.
- Network monitoring tools
- Configure scripts for the dev machine, so everyone has the same environment
- Shoe organizer on the door [pic]

## Infrastructure

- There's no IT department, so spend
- There's no office manager, so spend on snacks, coffee
- Backup channels, out-of-band control channels, redundancy in depth
- Be cognizant of asymmetric bandwidth: up is often much smaller than down. Screenhero host is mostly up, client is mostly down.

Things are going to go wrong, on a daily basis. By having fallbacks in place and procedures for what to do next, it is easier to keep the rhythm going. My CEO doesn't listen to me and complains when things don't go well. [sigh] Just keep going. Sometimes you just have to split the pair. Be ready for that, too.

- Voice: SoCoCo, Screenhero, Cell, Google Hangout, join.me appear.in
- Video: SoCoCo, Hangout, join.me, Facetime

## Space

- Desk
- Chair
- Good lighting
- Good ventilation

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