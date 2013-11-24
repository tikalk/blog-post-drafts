Remote work has never been more in-the-news, wherever I go on the internet, I see people talking about doing Remote work and how beneficial it is for people.

37Signals recently wrote a book about it, while I generally dislike their style, I think they hit a nerve here.

## History

When I started my company 4 years ago, I knew that remote work will be a huge part of the work environment, and I was explaining it to my clients.

Back then, remote work was even less popular and wide-spread than it is today, so it was hard at times, but the system we developed in the company proved itself useful to clients and after a week or two every single client that was having doubts was completely sold.

When the company joined the Tikal family, I was really happy that this remote work system caught on here as well and kept developing and providing really excellent service to clients all over the world.

## Present times

Here at Tikal we are working with remote clients and remote teams (which typically come together).

For example, right now we have a client from the US (Seattle), the Tikal team working with the client consists of 3 engineers (2 women b.t.w) working out of Israel. The client also has other remote employees from all over the world and the Tikal team needs to work in synergy inside this situation.

## How it's done?

The way we do it is pretty simple and straightforward, however I have not seen a post that covers everything in detail yet, so let's dive in to the tools and the other nitty gritty details.

## The system

The system we developed and refined here consists of a few categories details below

### Birds eye view

The client and the company (Tikal) both have birds eye view of the project and how it's running at any given moment without asking anyone.

You know the status of the tasks, the builds, the hours spent, everything is just one click away.

### Automation

Every task you do more then once is automated, whether it's through the chat robot (details in the post), CI or anything else, but it has to be automated.

Any task has to be simple enough that any person with an internet connection and access to the project can do it at any time

### Control

Really not much to say here, the client must have control on what's happening to the project at any given time.

### Tooling

Both virtual tooling and physical tooling are crucial for the system to work, as you can see detailed in the post


## Virtual Tools


### Chatroom

Perhaps the most crucial part of the remote work is the chatroom, we use 37Signal's [Basecamp](http://basecampnow.com) in order to communicate with the team.

Every team member (from the client side and from the Tikal side) has an account and is online whenever he/she is actively working.

Meaning, the entire communication between the team happens asynchronously, you can leave a message to someone but you can't rely on them to reply right away, they might be focused on a bug or a feature and not always pay attention.

#### Flint

![Flint](http://f.cl.ly/items/2o2f2c0W3i0W221n3O3f/Image%202013.11.24%2010%3A04%3A45%20AM.png)

We **don't** use the web version of Basecamp, because for one you have to open up a browser, you have to dedicate a tab and if you close it it's gone (and it might take you some time to notice).

Instead we use Flint, which is an awesome Mac app ([Purchase link](https://itunes.apple.com/us/app/flint/id459122418)), and an iPhone app ([Purchase link](https://itunes.apple.com/us/app/flint-for-ios/id698724032))/

The best thing about Flint is that you can define notifications and keywords.

During the day, you don't want to get notified on every line in the chatroom, that will be too distracting, instead you can define keywords (for example you name), and only then you get notified.

This way, someone can "poke" you when they need you specific attention.

![image](http://f.cl.ly/items/3E3t2u2q2A1E092z0A36/Image%202013.11.24%2010%3A07%3A21%20AM.png)

Flint is running in the background, so you are always online when you work, and you are confident that when someone needs you, they will find you and you can resolve the issue they have.

#### CI

Whenever you have lots of people working on a project (especially in different timezones), having a CI will really help you control what's going on in your project.

We probably use every CI solution out there (clients sometimes demand working with an existing CI) so we really have experience in setting it up, notifications, build types, deploy types and more.

The CI has built in triggers, whenever a code is pushed to master (we never push to master more on that coming up), the CI runs all of the relevant tests and triggers a notification to the chatroom (more on that coming up as well).

Our CI tasks usually include

* Deploy to production
* Deploy to staging
* Unit tests
* Integration tests
* Javascript tests
* Branch tests (when you want to test a PR)
* Deploy to dev servers

Automated triggers usually are

* Deploy master to staging twice a day on fixed times
We usually have [uTest](http://www.utest.com/) cycles twice a day, so this way we know they have the latest master to test.
* Run specs on master change
* Run specs on chatroom trigger (using [hubot](http://hubot.github.com/))
* Run deploy on chatroom trigger (using hubot)

#### Hubot

In the above section I have mentioned hubot, we use our own version of hubot on every project.
Hubot is usually idle in the chatroom and can respond to two kinds of triggers

* Listen
* Command

Listen means, then whenever a keyword appears in the chatroom, hubot will do something.
Command means you can tell hubot to do something.

Listen sample code:

```

	images = [
	  "http://s3.amazonaws.com/kym-assets/photos/images/original/000/114/151/14185212UtNF3Va6.gif?1302832919",
	  "http://s3.amazonaws.com/kym-assets/photos/images/newsfeed/000/110/885/boss.jpg",
	  "http://verydemotivational.files.wordpress.com/2011/06/demotivational-posters-like-a-boss.jpg",
	  "http://assets.head-fi.org/b/b3/b3ba6b88_funny-facebook-fails-like-a-boss3.jpg",
	  "http://img.anongallery.org/img/6/0/like-a-boss.jpg",
	  ]
	
	module.exports = (robot) ->
	  robot.hear /like a boss/i, (msg) ->
	    msg.send msg.random images

```

Command sample code:

```

	module.exports = (robot) ->
	  robot.respond /PING$/i, (msg) ->
	    msg.send "PONG"
	
	  robot.respond /ECHO (.*)$/i, (msg) ->
	    msg.send msg.match[1]
	
	  robot.respond /TIME$/i, (msg) ->
	    msg.send "Server time is: #{new Date()}"
	
	  robot.respond /DIE$/i, (msg) ->
	    msg.send "Goodbye, cruel world."
	    process.exit 0

```


Our hubot is constantly getting updates and new tasks, we do that to automate everything we need and to expose engineering tasks to the client.

For example, client can deploy to production, staging, run tests, reports and do anything else they want, and if they don't have it, it's usually just a few lines of code away.

#### Project Monitor

![image](http://f.cl.ly/items/30330w3d331a1W1L0U2P/Image%202013.11.24%2010%3A37%3A44%20AM.png)

We use Pivotal's [project_monitor](https://github.com/pivotal/projectmonitor) In order to expose the status of the project to the client on one hand and also to expose the status of all projects to the managers here in Tikal on the other.

This way, the client always knows the status of the builds, and we know the status of every project running right now.

#### Pivotal Tracker

![Pivotal Tracker](http://f.cl.ly/items/061a0v0d0z3E3B1y2J2A/Image%202013.11.24%2010%3A42%3A17%20AM.png)

Pivotal Tracker is probably the most used tool in the company, we use it to communicate, plan, comment, reject, accept and any other project related task you can think of.

Really, I think a lot of people use Pivotal Tracker, the secret IMHO lies in **HOW** you use it.

For example, if you see code that needs to be refactored, open up a pivotal task and put it in the icebox, when you get around to it, just work on that story.

Also, another secret that a lot of people know but very few actually do, is to be as descriptive and communicative as you possibly can, write great story descriptions and great comments and you'll see that with time, this communication comes natural and when you see a story you can easily jump into the discussion.

#### Google

We use two Google products as part of the flow

* Hangouts
* Talk

I think the usage is pretty obvious but one thing that is important to clarify is that we use Google Talk ONLY if the conversation is not relevant to the other team members, if it is, we use the chatroom.

#### Skype

Facetime and pair programming (for voice only)

#### Remote pair programming

![Tmux session](http://f.cl.ly/items/3z0R3P2O1o1K2j2g341V/Image%202013.11.24%2010%3A50%3A56%20AM.png)

I ABSOLUTELY love pair programming, I think that people really miss out on a great thing if they don't do it. Remote or not, do it and do it NOW.

Pair programming in remote **usually** really sucks, and the solutions people use are really awful.

I hate screen sharing, it's broken by design.

The way we do remote pair programming is using [Tmux](http://tmux.sourceforge.net/) and SSH into the machines.

Tmux is basically a terminal multiplexer, it "sits" on top of your terminal and acts as a window manager on steroids.

SSH into the machine means that you don't have to share screen and everything works really fast and really awesome.

Also, as you can see in the image, every part of the project is exposed to both sides.

* Vim
* Rails Console
* Pipe for running tests
* Spork session
* Rails server
* Guard
* Foreman

You can also open new tabs, for example SSH **together** into staging and see something happening in real time.

Both sides have full control and can *drive* in any given moment, it's just like typing in Vim on your own machine, it works really well.

Tip: having a dotfiles repository really helps here. this way when you ssh into someone else's machine you already have all the shortcuts and the muscle memory to just work without issues.

### Real world tools

So far, I explained about the virtual tools, but real world tools are essential for this all to work.

#### Home office or isolated work area

![home office](http://f.cl.ly/items/2J0f2C2p100s0U0L1k1p/Image%202013.11.24%2011%3A02%3A29%20AM.png)

Working on your dining table is out of the question, it's too distracting.

In order for remote work to really have the benefits is to have a home office or an isolated work environment.

This way, you can sit and work without distractions or noise.

If also helps to get into the zone. I am writing this post from my standing desk at my home office, while fully clothed to work after showering and getting ready to work like any other normal person in the world, I just work 20 meters from my living room.

Working from your bed in your PJ's is not what remote work is about, it should be professional and not a game.

I think that most companies that have remote work forbidden have this in mind, employees working in PJ's while half asleep, or working out of bed, this is not remote work it's slacking off.

#### Internet, headphones, microphone

Decent internet connection, really good microphone and headphones are a must.

If I need to talk to you, I need it in good quality without you breaking off, I need you to hear me the first time I say something and be responsive.

It should be just like I am talking to you in person only we can be 5000 miles away.

For example, I have 2 routers, 2 ISP's and 2 lines, I am never offline for more then 2 minutes.

If one of my routers fail, I just replace it, if one ISP fails, I just have another one already predefined in the router for a fallback.

I also have a backup computer already predefined to work.


### Putting it all together

When you put it all together you can see that this system is bound to work, if you follow the rules, tools (physical and virtual) remote work is the best thing that can happen to you.
