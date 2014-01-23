[58: A Team Groupon Show](http://nodeup.com/fiftyeight)
===

Panel:

* [Daniel Shaw](https://twitter.com/dshaw)
* [Sean Massa](http://twitter.com/endangeredmassa)
* [Sean McCullough](http://twitter.com/mcculloughsean)
* [Adam Geitgey](https://twitter.com/ageitgey)
* [Laura Roman](https://engineering.groupon.com/)

Table of Contents:

1. [Introduction](#introduction)
2. [Placeholder](#placeholder)
3. [Placeholder](#placeholder)
4. [Placeholder](#placeholder)
5. [Placeholder](#placeholder)
6. [Placeholder](#placeholder)
7. [Placeholder](#placeholder)
8. [Placeholder](#placeholder)
9. [Placeholder](#placeholder)
10. [Plugs](#plugs)

## Introduction

00:17 - **Daniel Shaw**: Hello and welcome to NodeUp! This is NodeUp 58 and today we're gonna do another Node Teams show. I'm joined today by Team Groupon. A great story where Groupon has transformed and been one of the great success stories or moving from Rails to node. I'm joined today by [Sean Massa](http://twitter.com/endangeredmassa), [Sean McCullough](http://twitter.com/mcculloughsean), [Adam Geitgey](https://twitter.com/ageitgey), [Laura Roman](https://engineering.groupon.com/). Today, our sponsors are Joyent, Clock, and &yet. New sponsor: welcome Joyent! Thanks for sponsoring NodeUp. So, I'm @dshaw I run The Node Firm. We help business be successful with node through training, consulting, and support. Let's go through and have everyone introduce themselves. Sean, you want to kick it off?

01:27 - **Sean Massa**: I'm a node developer at Groupon. I really love testing and community. I work on testing tools and we'll talk some about that. I also run the Chicago NodeJS and Geekfest meetups in Chicago. 

01:40 - **Sean McCullough**: I've been working at Groupon for a couple years. I was the one who started playing around with node at Groupon and helped make it a reality here and I've been starting to evangelize it for other teams who want to make the big switch.

02:00 - **Daniel Shaw**: Fantastic. What's a classicist?

02:02 - **Sean McCullough**: I studied Latin and Philosophy in college. So, yeah, really marketable skills there.

02:10 - **Daniel Shaw**: Like all the great JavaScript developers come with a liberal arts degree. That's awesome. Adam?

02:18 - **Adam Geitgey**: Yeah, so I'm an engineering manager at Groupon. I've been working with these guys for about a year on this node buildout. Before that, I worked with any number of other languages and this is just the latest one.

02:28 - **Daniel Shaw**: Excellent. Laura?

02:31 - **Laura Roman**: So I have a humanities background like Sean McCullough. I have an phd in English Literature from Oxford. And my role at Groupon is I head our technology public relations. And my general passion is technology and story-telling.

02:42 - **Daniel Shaw**: Great! And you're responsible for bringing the team to NodeUp, so thank you for that. 

02:46 - **Laura Roman**: Thank you!

02:48 - And Lauren makes us talk good in public.

[Laughter]

02:53 - **Daniel Shaw**: I will try to ruin that!

02:55 - **Laura Roman**: We have a lot of fun!

02:57 - **Daniel Shaw**: I'm gonna do my best to try and derail that in the nest hour. So that's good. So, let me kick it off real quick with a word from our sponsor. Welcome, Joyent, to sponsoring NodeUp. It's great to have them onboard as a sponsor. So, Joyent is a high-performance cloud provider and a big data analytics company. They deliver node.js as the best runtime for realtime applications. They are the corporate sponsor and steward of node. And Joyent has a cool new offering to support the debugging of node, now not only on SmartOS but on Linux and takes advantage of the power of their Manta smart data platform -- Manta's really cool -- and, anecdotally, all built on node. So, on top of that if you want the in-depth tooling that has only been available in SmartOS, you can get that now from Joyent. So for their cloud services, go to Joyent.com and you can sign up for their compute service, their Manta data storage service. If you want to learn development and production best practices, head over to [joyent.com/developer/node](https://joyent.com/developer/node). Be sure to follow Joyent on the Twitters [@Joyent](https://twitter.com/joyent). Welcome, Joyent, it's good to have you guys on board.

Alright! So Groupon's been, historically, a [Ruby on Rails](http://rubyonrails.org/) shop and I started doing what I do for The Node Firm almost exclusively because of this challenge. I was working at a startup that was trying to scale Rails and I was responsible for the realtime services. But the core application when it got up to a hundred users in our initial private beta and Rails fell over. It was like "that's not scale! Come on, Rails!" So Sean, why don't you fill in the context and kind of tell us the story about why node came to Groupon. 

05:17 - **Sean McCullough**: Sure. So Groupon has a pretty widely publicized history where we went through this period of hyper growth through 2008 until it probably leveled off around 2012. And we started off as a daily deal company. So we'd send out an email to you early in the morning. People would come in and see one deal for their city and that was a very simple model. We didn't stick around with that model for very long. Ever since we nailed that one thing Groupon wanted to expand and diversify. So we don't just do daily deals, we do getaways, we ship you physical things, we sell concert tickets, we sell experiences and things like that.

You made a point about the "Rails not scaling" thing. I think we were actually able to get a lot of scalability out of Rails; where we started hitting major issues with Rails was our architecture. We had this one huge Ruby on Rails app that was actually from our first incarnation as The Point -- Groupon started as The Point, which was a charitable giving site. It was kind of like kickstarter for charitable giving. So you and five of your friends would get together and be like "we'll all donate twenty bucks to a charity if all of us go in and do it together." So our old codebase had references to things from The Point. And it still does to this day.

And we kind of kept building upon this monolith over the course of five years. And that was really slowing us down. We were having a hard time adding new features. We couldn't go through and make the changes to Groupon's brand like we wanted to. So right when I joined, I was on what we called the Scottsdale team. That was an effort to do a visual refresh for Groupon's web site. We tried to do this all in our monolithic Ruby on Rails app and it was a failure for a few reasons. The biggest of which being: the way our Ruby on Rails app was set up, by adding the new look and feel to the site we broke our caching. We fragmented our cache to the point where the site got significantly slower. And up until we cut over completely to the new design, the site was gonna suffer for it.

And, you know, if you work for an e-commerce company you're probably used to the idea that your A/B testing different experiences. And we weren't able to get solid signal on whether our A/B test was doing well because the site was getting slow. So this is where I started getting interested in "let's figure out how to break this up." I think for all of us, we kind of knew that we needed to move away from the monolith. Adam was on one of the first teams to do this, where we moved some of our merchant stuff out. And that worked really well. But you guys use Rails, right?

08:31 - **Adam Geitgey**: Yeah, we use Rails. The biggest advantage we got there was strictly because we were building just a separate small app, not a monolith. But I think one of the things people don't realize is just how much of a startup Groupon is and was. Groupon's barely five years old. So even though today we're in 49 countries and we have over ten thousand people around the world, Groupon since day one -- until maybe a year ago -- has been growing so fast that we've been just kind of hanging on. So the same Rails app that was written in 2009 was still serving 1,000x traffic, or whatever it was.

The other challenge we had was: Groupon grew, not just in the U.S., but we acquired a lot of companies in other countries -- that's part of how we got into 49 countries -- and along the way we picked up a lot of different software platforms. A lot of the companies we acquired, we picked up their technology stacks, too. So part of our goal here was not just to replace the U.S. platform but to replace this with a new node platform that could be a single platform around the whole world, around all these different countries. And have a real service-oriented architecture here. 

09:41 - **Daniel Shaw**: That's fantastic. So, the initial starvation pattern -- or picking off the services that can get pulled out of a monolithic architecture -- actually started out with smaller Rails apps.

09:55 - **Adam Geitgey**: Yeah, that's exactly right. We stared with rails apps, just to prove the idea that building out of this monolith would work. And then Sean took on a project to actually build some prototypes in different languages. Sean, maybe you can talk about that. 

10:07 - **Sean McCullough**: So we actually had started with a crazy idea that me and one of my coworkers, Keith Norman, he kinda had this idea where, because we had this huge monolith, and our server side rendering times were getting worse and worse, we started building a lot of client-side applications in our monolith. It was easier for the teams to develop in backbone and stuff like that and use our RESTful API to build good consumer experiences. But the problem with that is that there's some serious penalties to having single page applications, especially with the users that we have. I think most people come to Groupon, they'll browse around for a minute and they'll kind of bounce off the site. Or they buy. They're not engaged with the web site for ten, twenty, thirty minutes. So, Keith had this idea where we would take our backbone apps and figure out how to render them on the server. And, actually, we had talked with some people at AirBNB and they have a platform to do something very similar to that.

11:11 - **Daniel Shaw**: Right, right. What's that thing called? I forget, off the top of my head. Spike's thing!



