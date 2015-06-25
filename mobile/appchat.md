### Intro

[AppChat](http://appchat.co) is an Android app that makes a chat room for every app on your phone. It experienced a large spike of users early in it's life, roughly [10,000 in the first two days](https://medium.com/@matthall2000/appchat-week-1-freak-outs-flaming-servers-and-6-releases-14f81552f186).

### Traffic and Loads

Between 10-100 simultaneous users, fairly high write to read ratio.

### Framework

Our backend is Java using [Play Frameowrk 1](https://www.playframework.com/documentation/1.3.x/home). We've written a bunch of things using Play 1 so we stuck with it for this too. Play 2 has been out for a while now, but it seems like they lost the productivity focus of the first version and they got excited about Scala, which we are not. Java is fast which is good, but needs a fair amount of memory which is where we see the limits of our hosting setup (see below).

### Hosting ($25/month)

We use [Heroku](https://www.heroku.com/) usually running a single 1x dyno. Heroku is an awesome software platform with a weak hardware offering. Dyno performance is too variable, there's not enough options to scale vertically to bigger dynos, and they somehow ignore the existance of Moore's law by never improving the dynos themselves. That being said, the deployment and rollback options alone keep us using it. Nothing is fater to set up and has less management time. We deploy anywhere from 1-10 times a day and nothing is faster and simpler than Heroku. We just wish the dynos were better.

**Aside:** Play support for worker dynos is basically non-existant. Howver, we finally figured out how to run Play jobs on a temporary scheduler instance started just for that job. We use a module called [Jobberwocky](https://github.com/bubblecrisis/jobberwocky) (awesome name). This helps us stick to 1 dyno serving app traffic and not have it slow down when the larger jobs run.

### Database ($49/month)

We're old school and still use MySql. MySql has been bumming us out lately though, especially with slow or failing large table alters and a fair number of transaction timeouts when we're under load. Next project we'll try Postgres I think. We run it on a t2.medium [RDS](http://aws.amazon.com/rds/) instance. There's cheaper ways to do it, but then you're in charge of your own backups, etc. **Impportant Note**: On RDS make sure you provision enough storage as it also determines your IO quota. We don't use much storage, but still have 30GB provisioned to get enough throughput. Storage is cheap anyway.

**Avoid:** We first tried ClearDB which billed itself as a hosted clustered MySQL as a service type thing, but we hit an undisclosed monthly query limit and they just shut us off. No overage charge, no warning, just shut us off. Their support was unsympathetic too. Not recommended.

### Services ($51/month)

* **[ZeroPush](https://zeropush.com/)** - $32/month - Easy Android push messaging for too much money. I haven't switched off of this yet but should.
* **[Pusher](http://pusher.com)** - $19/month - A really good and resonably priced websockets service, recommended.
* **[Logentries](https://logentries.com/)** - Free - A really solid log search and tagging service, recommended.
* **[Redis Cloud](https://redislabs.com/redis-cloud)** - Free for 25MB - For caching and some realtime user presence tracking.
* **[Mailgun](http://mailgun.com)** - Free for 300 emails/day - Really simple email service. Can turn incoming emails into POSTs which is great too. We don't send many emails in this project but have used them for solid volume on other projects.

## Total Cost: $125/month.

Could be cheaper, but probably would be less convenient. We have a small team and try to release a new version of the app twice a week. Any extra time spent on infrastructure has a significant cost in lost time.
