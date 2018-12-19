Patchwork 4.0 consensus free scuttlestack
===

cross-posted on ssb: `%meil3xW/YT4WJ26mAsmLVHKxxh2rbtbM7pyvpA1N4v8=.sha256`

## agenda

- [x] checkins
- [x] context dump
- [x] lessbot
- [x] multi sql dbs (how not to have duplicate data)
- [x] updating the view when the db writes.

## ???

- Mikey: Call inspired by [Why it is so hard to build SSB apps](https://viewer.scuttlebot.io/%251bz0TXDuaM65KMTb8bgtrXuqD7L77baneTdoJ0EwRug%3D.sha256)
    - kind of sounds like leader and follower databases (lessbot)
- Matt: had a conversation with Christian after the fairie ring call, shared to Christian about previously wanting every app to be a separate sbot peer, but struggled to find others who supported that idea
    - scuttleshell isn't still quite right. It doesn't solve the core problem. The core problem is a "tragedy of the commons" in the ssb folder.
    - wanted to write everything down, why separate sbot peers is a bad idea, why scuttle-shell is also a bad idea, why there is a middle ground between the two
    - we are treating the ~/.ssb folder like a commons, but we haven't described how we manage the commons together
    - the new idea: lessbot, with a single writable database. where each app has their own read-only database.
- Matt: i want to find a way to experiment with rusty flume stuff in Patchwork, in a way that is mostly backwards compatible with everything else
    - i want to be able to run Patchbay alongside Patchwork, even at the same time
    - we can all benefit from the commons without agreeing on too much
- Piet: can share about the flumedb in rust project, where it's at, some of my opinions and discoveries
    - what i've built so far: defined traits that describe what a flume Log and View are
    - written an offset log decoder and encoder, iterate through every item, deserialize every item
    - flume View which uses sql database
        - three tables:
            - authors
            - data (key, author, content_type, content_json)
            - links (links_from, links_to)
        - can search by type, messages linked to, etc
    - benchmark: can process entire ssb offset log in 17 seconds, compared to JavaScript implementation which takes 230 seconds
    - super exciting!
    - because we're storing content in json, sqlite supports querying in json as well
    - we're kinda storing everything you want to query
    - includes joins! which we don't have in ssb-query
    - code is up at https://github.com/ssbrs/flumedb-rs
- Matt: how does it relate to the flumelog as it exists now
- Piet: may be you get everything you need from sql database, but doesn't store everything verbatim, only stores content json
    - maybe if you need whole message, you get sequence number and ask from flume log
- Matt: keen to run flume stuff directly in Patchwork process
    - Piet: my plan is to make Node.js bindings so you can treat it like calling any JavaScript
        - ideally will need some async mojo so we don't block the main thread
- Matt: how much work is it to expose Rust library as something that can be bound to non-JavaScript?
    - Piet: there are excellent libraries like rust-bindgen to make c bindings
- Matt: so far you haven't done any writing yet?
    - Piet: in my tests i've appended to an offset log, so i know it works, but i haven't played with it much
    - Matt: have you had thoughts on how you'll expose the api?
        - will it be a wrapper around sqlite?
    - Piet: the quick and dirty way is to ask sql questions and it gives you an answer
        - it would be relatively easy to expose methods like messagesByType
        - convenience wrappers
- Ben: i think being able to query the sql database (especially into the json) would be cool
    - if you create json expressions, you can create ad-hoc indexes on the json data
    - Piet: i haven't played with trying to build indexes, i thought you could only do it in PostgreSQL
    - Ben: i think when they added support for json they added json indexes
    - Ben: would be great to work with a sqlite table and ignore the ssb junk
- Matt: i was thinking of having individual indexes per app
- Matt: lessbot doesn't include this
    - lessbot is just the gritty replication, not anything on how to render the data
    - we don't have to agree that everyone uses rusty flume
        - people can continue to build in whatever interesting query language they want
    - i want to see an ecosystem where we don't have to agree on a framework, just agree on a minimum viable shared stack
- Mikey: ...
- Matt: Mix wanted to continue to use the JavaScript indexes alongside the new Rusty flume indexes
- Matt: i want minsbot to be a minimal subset of sbot
- Ben: running an sbot in rust is a lot of work
    - Piet: can add context, the last time we had our Choir catchup, we talked about how hard a rusty sbot would be. Aljoscha expects the ebt replication part isn't too hard, he's currently studying ebt for a paper at university. we talked about building the friends graph, there's already Rust implementations of dykstra's algorithm, we can follow what the Go implementation does and re-build the graph anytime things change, not as dynamic as the JavaScript implementation. Aljoscha already did muxrpc and most other work.
    - Matt: if we come up with a small sbot subset, no plugins, just the core, it'll be beautiful
    - Piet: Aljsocha would be ecstatic by that argument
    - Matt: it only needs to replicate with other peers, not sure

---

- Matt: points in the message i sent out
    - Matt: keen to solve: initial sync responsiveness
    - Matt: two parts:
        - why is it so hard to build ssb apps
        - holding tank
            - indexes prioritize local updates over remote updates
- Matt: just a guess, most people spend time in initial sync
- Matt: is that feasible?
- Piet: can you go over what was wrong with Dominic's suggestion to keep it in the client?
    - Matt: for every query you had, you would have to store messages locally, you'd have to synchronize everything in a custom way
- Mikey: We can think of this like optimistic updates. Matt wants optimistic updates to be baked into the db.
- Matt: so many ways that bugs can occur, which means people can stop trusting the system. for example, in my work project, we had separate validation in the client and the server, we had a bug where the client accepted a change and the server rejected it, our users got used to refreshing the page to check if their updates went through.
    - we need something like this to have a pleasant experience
- Piet: my temptation is to say: it's a fancy feature, if we're going to prioritize things, it's down the list a bit
    - maybe i can cook something up that makes this easy to do
    - happy to sit on it while i'm working on the system
- Matt: i just want you to be aware that this is a problem i want solved
    - if you could do it the way i suggested, that would be incredible, but i'm not even sure if that would work
- Matt: i just think that it's a failure of flume's design that it doesn't prioritize local messages
    - with Andre's problems he's trying to solve on mobile, the sync queue is too long
- Piet: nice to bring up something: i was planning for the flume View api to be slightly different, the indexing is not coupled one-to-one to inserts in the db. i want to expose a function you call `synchronizeUpTo(sequenceNumber)`, makes it possible to throttle properly, stop indexing the main log and start indexing local changes
    - Matt: could be that it needs to happen deep in the stack
    - Piet: this is in my rust stuff
    - Matt: like chunks, index 10 things, then go back to local changes, then index 10 things, etc
- Matt: i think we've talked about everything i proposed in private message
    - is there anything new we've talked about 

- Mik: how are we going to do lesssbot?
    - ...
- Matt: Everyone including Andre is keen for lessbot.


---

- Piet: a question i have, if i exposed only a thing that gave you sql queries, not much convenience, would that be enough to play with?
    - Matt: yes, but we need real-time queries
    - Piet: what is real-time?
    - Matt: i need to know when new messages have come that match the query 
    - Mikey: ...


---

- Matt: a fun note, i just added a new view to Patchwork, messages you are participating in any way
    - Matt: worried it might be really slow if you haven't participated much
    - Matt: is this type of query we can do in sql?
    - Piet: if you could do this in a query now, you should be able to do it in sql?
    - Matt: but we can't do this with a current query, you have to check all authors and see if you are one of them
    - Piet: i have sqlite running, been playing with queries, keen to have a go
    - Matt: can point you at the code: https://github.com/ssbc/patchwork/blob/master/sbot/participating-feed.js
    - Mikey: ...
    - Piet: can make a new table to put the data there
    - Matt: on this other app, we want to query against final state
        - like "am i attending this gathering?"
        - if the state is more complex and has more layers
    - Piet: this idea of reducers, sqlite has an idea of extensions, can write code in rust or c, can hook into sqlite database
    - Matt: that's the power i want to see exposed, that's why i want to be able to get in there and twist thing
    - Mikey: let's have all the flume Views! a thousand sqlite databases each indexing different data

---

## multiple sql dbs

- Piet: tension between
    - putting everything in one database, can do more complex queries
    - separating everything out, more sources of truth
- Piet: probably just a problem of trade-offs
- Matt: trade-off of duplication vs autonomy
- Matt: biggest problem will be on mobile
- Matt: have no idea how this lessbot stuff will work with what Andre's talking about how to run multiple ssb apps on mobile devices

## updating rust stuff when db writes

- Piet: sanity check: you can subscribe to offset log in JavaScript, you call a native function which tells the Rust bits to update
- Mikey: what if you listen to a file system change?
    - Node.js has an fs.watch function, wonder if there is a rusty cross-platform way to listen to file updates

- Matt: mentioned i had a suggestion on how to do optimistic updating of local changes: you post to the server, the server tells you the offset number, you tell the server to index ahead
    - Mikey: what about if it failed half-way through, wouldn't it break robustness?
- Piet: what are examples where you need optimistic updating?
    - Matt: following is the big one, a lot changes when you follow them, there's no way we could do that everywhere without going deep
    - Matt: from a user experience point of view, it's better to do it real rather than fake it, will always leak out
    - Piet: notice there's edge cases around ordering
- Matt: ssb doesn't care about order of all feeds, cares about order of a single feed
- Mikey: index ordering relates to leaking private messages later

- Matt: want to bring up initial idea from holding tank
    - the bit that's injecting the messages into the log slows down when the indexes are behind
- Mikey: priority could be passed to the lessbot so local messages could be appended before replicated messages ...
- Matt: writes happen immediately, indexes take longer
- Mikey: I was imaging it would take a while to write to offset.log. 
- Matt: we don't have back pressure, offsetlog can get way ahead of views. 
- Piet: missed why the priority queue wouldn't work
- Matt: the offset log is miles ahead of the indexes, by the time you insert a local message into the log, the old messages are already there
    - so unless you had two offset logs, a priority one
- Piet: maybe this isn't true for the rusty implementation
    - maybe the indexes won't be the bottleneck anymore
 
 - Matt: how does sqlite behave when you do a query before indexing has finished?
    - Piet: on the rust side, sqlite queries are atomic, you can only do one thing at a time
    - Piet: starting up the database, you open offset file, you open database, you check if your view is behind or not, you check the database's validity in case it crashed, if you're behind you need to bring your View up to date with the Log.
- Matt: say you're replicating stuff, and i do a query for the public feed, how will it behave if we're up to sequence 100 and the index is only up to 10
    - Piet: you'll do a query with whatever is in the state at the moment
    - Piet: you should have the option to wait until a particular sequence number or just use the data there already
    - Matt: sound cool

- Matt: keen to see what we've gotten working, how it behaves under real queries in Patchwork

- Piet: i could also send you my sqlite database so you could play with it
- Matt: i could also just swap out the backend for sqlite in Patchwork

- Mikey: how big is the sqlite database?
- Piet: 420 MB
- Matt: how big is your offset log?
- Piet: 557 MB
- Matt: for comparison, backlinks index is 160 MB 
