## Intro

[AppChat](http://appchat.co) is an Android app that makes a chat room for every app on your phone. It experienced a large spike of users early in it's life, roughly [10,000 in the first two days](https://medium.com/@matthall2000/appchat-week-1-freak-outs-flaming-servers-and-6-releases-14f81552f186).

### Traffic and Loads

Between 10-100 simultaneous users, fairly high write to read ratio.

### Framework

Our backend is Java using [Play Frameowrk 1](https://www.playframework.com/documentation/1.3.x/home). We've written a bunch of things using Play 1 so we stuck with it for this too. Play 2 has been out for a while now, but it seems like they lost the productivity focus of the first version and they got excited about Scala, which we are not.

### Hosting ($25/month)

We use [Heroku](https://www.heroku.com/) usually running a single 1x dyno. Heroku is an awesome software platform with a weak hardware offering. Dyno performance is too variable, there's not enough options to scale vertically to bigger dynos, and they somehow ignore the existance of Moore's law by never improving the dynos themselves. That being said, the deployment and rollback options alone keep us using it. Nothing is fater to set up and has less management time. We deploy anywhere from 1-10 times a day and nothing is faster and simpler than Heroku. We just wish the dynos were better.

### Services ($51/month)

* **[ZeroPush](https://zeropush.com/)** - $32/month - Easy Android push messaging for too much money. I haven't switched off of this yet but should.
* **[Pusher](http://pusher.com)** - $19/month - A really good and resonably priced websockets service, recommended.
* **[Logentries](https://logentries.com/)** - Free - A really solid log search and tagging service, recommended.
* **[Redis Cloud](https://redislabs.com/redis-cloud)** - Free for 25MB - For caching and some realtime user presence tracking.
