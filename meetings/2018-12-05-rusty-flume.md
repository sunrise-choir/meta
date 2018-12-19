# sunrise choir - Rusty flume api.

cross-posted on ssb: `%vzNMtiMf6+cK2UqYKbdS5Ugs9FTLMNKi67vl7zAC+L4=.sha256`

## Attendees

- piet
- mikey
- matt
- mix

## Agenda

- [ ] checkins
- [ ] what's the purpose of this meeting?
  - Get Mix and Matt to help guide us toward a rusty flume they would want to use in their apps.
- [ ] build an agenda
  - [ ] banned phrase: "deep butt stuff" :peach: 
  - [ ] matt's experiments over the last few days 
 
Possible talking points that might go in the agenda:
- how do you want to do queries?
- what parts of flume are currently pain points?
- One thing that springs to mind is that you can't query a view until it's up to date with the log. I'd like to make that optional. So you can ask a view for what it's got right now, or wait for it to sync with the log.
- what's the ideal api in your mind?
- indexes? Good or bad how they're done at the moment?
- are streams a must have for you? ( I realise this is probably a stupid question but hey )
- Are there any flumeview-reduce views that people know of that can't be build using a query?

Also to discuss : 
- which parts of sbot api is @matt using alongside ssb-backlinks (e.g. createLogStream)

## Queries

- mix: what queries do we do?
- mix: why do we do queries this way? is it because we're painted in a corner, or because we want to do things this way.
- matt: I'm not really using many flume features
- mix: let's walk through different pages of the apps and all the different things they touch
- mix: for example: i load up a thread
    -  a thread is a linked list of post messages
    -  we need posts involved in thread
    -  we need current names and avatars of all participants in the thread
    -  we need backlinks to any posts in thread
    -  we need likes to all posts in thread
    -  mix: how do we build this up in Patchbay?
        -  get backlinks for particular root (leaning on root key in post messages and newer likes)
        -  ask for each name and avatar async-ly at render time
    -  matt: in Patchwork?
        -  wrapped backlinks in small plugin that sorts on server and returns payload of all messages in thread by heads
        -  all nice-to-have things are done async-ly on-demand
        -  if already looked up async-ly, handled by cache
        -  backlinks are same as likes
- mix: interesting edge, live-updating
    - doing live likes on old style likes is onerous. Old likes don't have a root. You need live query open for every post.
    - names and avataars: moving towards not live updating. means name and avatar are rendered at the time, and won't change if user updates
    - the tricky one: live updating of the thread itself. Tricky because of causal sorting. When you get a new message from a live stream, the message might not be in causal order.
- matt: i'm experimenting with a twist on live-updating
    - trying to decouple live-updating from other pieces
    - made some changes to mutant: able to only observe only things that are visible in the dom
    - you might have only 10 open streams instead of 1000
    - finding this method has been working, let's you do all the magical live-updating that you can dream of without the overhead
    - mix: what about the live-sorting problem?
        - matt: initial render is sorted and sent as one, additional updates are appended to the end
        - want to highlight the new posts as they are appended
- mix: interesting to note that Patchwork and Patchbay have different approaches, Bay has tabs, users might keep them open for ages
- matt: backlinks index is the backbone of all the new stuff
    - mix: and ssb-query
    - matt: forward and reverse search
- matt: sometimes backlinks will give you more than you need, trivial to chop out
    - even about mesages, resolving names and avatars, etc
- mix: we recently gutted ssb-about and rebuilt with backlinks
- mix: new pattern: computation on the server, more query and data manipulation on the server, less you send over muxrpc the quicker
- matt: interesting that our work has started to orbit around these two plugins
    - maybe we need to formalize backlinks and query
    - make it awesome and fast and good
- matt: problem is you need to run your own plugin to get data as you want
    - going forward, if we figure out easy ways of adding plugins from an app, you get more power when you write a query in code
    - ssb-query: when filtering by indexes, it makes it faster, but there's no clear way to know when your query is filtering or indexing
        - matt: i know how ssb-query works so i can use it for the power
        - but i prefer to be able to write my own code to choose indexes more intelligently
- mix: magical query language which resolves which indexes to use is good, but has gotchas:
    - where you write a query which is really slow because it doesn't hit an index
    - where you write a query in such a way that it streams out data in a different order than you expect because it orders based on the last index key that you were using
    - i have this hack which i consistently use where i put a dummy query in the timestamp field, timestamp > 0, which means receive timestamp and stimulates it to use the right index and sort in the right order
    - piet: double-checking, the query that you use affects how it is sorted and what results you get

```json=
{ 
  query: [{
    $filter: {
      value: {
        timestamp: { $gt: 0 },   // this makes sure results come out ordered by asserted ts
        content: { type: 'post' }
      }
    }
  }]
}
```

- matt: my understanding of the reason for ssb-query: so we don't need any extra plugins
    - that's the dream: plugin-less clients
    - i think it goes a long way towards that
    - but when you have the ability to run your own plugins, it's so much better
    - i'm not interested in plugin-less clients
- mix: you could probably write something which is a lot narrower, interested to see what indexes are used
    - dominic's new idea: what if we could dynamically add indexes
- mix: there's a big problem where if you bump versions, it rebuilds indexes, is a pain for jumping between apps
- mikey: i'm hearing from matt that he only wants to use backlinks and custom plugins, not ssb-query
- matt: I'm using 10 backlinks queries and 1 flumeview-query.
- piet: can't we solve the rebuild problem by keeping old flume versions around?
- mikey: if we got rid of query what are the low level apis that you need?
- mix: we could look at the indexes and see what ones we are using.
- 
- mix: looping back on Mikey: how much are mix and matt willing to dogfood? 
- mix: If I'm going to dogfood, can I just have a small serving first (ie js and rust.)
- mikey: I'm looking for the lowest hanging fruit. If there's 2 apps we're trying to get it into, if Patchwork needs less work then that's what I'd do.
- mix: I'm annoyed. I want low hanging fruit too and I'm trying to work out what that is too. 
- mikey: 
- piet: one idea of how to serve a slice of dog food without whole dog roll, on initial sync, there are certain indexes be can build on initial sync with rust, then other bits can be handled in js
- matt: i have thoughts
    - i've been going through looking at the all the different interactions with sbot
    - i'm using the original sbot api, with backlinks (a re-write of links)
    - i'm using createFeedStream, createLogStream, createUserStream, and backlinks
    - this has all been recent, since i started the refactor, before that i was using more flume
- mix: how do you show all gatherings
    - matt: i'm using messagesByType to query messages by type
- mix: i've been writing apps with collections, not streams
    - where ssb-query is useful is being able to get a years worth of gatherings
- matt: messagesByType only gets root messages, not metadata about a gathering
    - have to find gatherings, then query for about messages
- mix: i'm querying all about messages, finding date ranges
- mix: i was wondering if i could get by on matt's diet of api, the answer is maybe probably?
- matt: what we need is the core api, but being able to get the union of multiple apis
    - i can query for type, user, received timestamps, asserted timestamps
    - ssb-query without all the junk
- mix: i've noticed something with christian bundy, i'll call it async indexes, trying to move content to a blob, at a low level it hot swaps content with a blob, ssb-search doesn't work for this if you don't have the blob at the time you received the message, maybe a day later you find the blob, you want to be able to add to the index later
    - matt: you can do this in sql, joins!
    - matt: you can do everything we want in sql!
- mikey: it's possible that our version of ssb-query is sql
    - matt: it's basically sql but without the good parts, joins
- mikey: i maintain that flume is a good re-invention, but not necessarily ssb-query
    - matt: i agree
    - matt: i think backlinks is an outlier, it's a reverse search
- piet: what's a reverse search?
    - matt: let me explain the magic of backlinks, it gives you the ability to find all messages that reference another mention ("give me all messages that mention this message")
        - mix: with forward references, a gathering would have to point to all participants
        - mix: with backward references, all participants can point to a gathering
    - matt: different from parentId = X (belongsTo)
        - everything can have multiple parents
        - could have a backlinks table (belongsToMany)
- matt: i'm sold

- piet: one question, do either of you use flume reduce
    - mix: i don't like it because it uses json
    - matt: i wish it used leveldb
    - mix: i like it's the most beginner-friendly
        - generally when you're building an app you want the reduced state
        - but the implementation is slow
    - matt: it's a massive atom
        - it's not the reduced state for a bunch of objects, its the reduced state for a single object
        - i'd be into something that reduced into multiple objects
            - mix: that would be the join basically
        - you'd have to build up a table that understood what root messages were
    - matt: i'm using it to count channels
    - mix: i think i ended up implementing something like that for Tick Tack, it loses data
- piet: quickly want to ask about other views
    - flumeview-level
    - flumeview-search
    - flumeview-bloom
- mix: haven't used bloom, flumeview-search and ssb-query is implemented on top of flumeview-level
- matt: using ssb-search, but have similar limitations on that, doesn't let me do hybrid searches
    - was trying to search just private messages, can't do that
    - interesting thing, but doesn't do all searching i need, have to do hand-rolling already
- piet: if i asked if we could do sql queries, would that be possible?
    - matt: need full-text search
    - matt: good news is that there are tons of full-text libraries that work with sql
- mix: one thing that is nice about the flumeviews is they only store bytewise pointers, not the whole key
    - piet: dominic wrote something about this
- matt: just sql won't be quite as cool as it could be
    - i like flume for having append-only log and indexes point to offsets
- mikey: the proposal is we still use flume logs and views, but the flume views use sql as much as possible
- mix: I'm not sure about how to do the async views. 
- mikey: i'm hearing from mix that sometimes we have messages with incomplete content, so sometimes we want to pass a message back through the views when we have more information
- mix: There are two asycn cases: New info arrives and I want to attach that info to that gathering.
  - second: eg: Someone reveals an unbox key and we need to update the ...indexs.
    - example: someone got a content blob and now knows what is the message
- mix: the good news is that this would still be deterministic (assuming the unbox key is a message I was sent)

- matt: another thing, multiple identities
    - indexes need to respect the identity
    - if i switched to another identity, i'd start seeing private messages from the other identity
    - this is a limitation in flume now that makes it really difficult to do multiple identities
- matt: i'm keen to talk about my experiences with on-boarding
    - more about building indexes than day-to-day use
    - last night, i switched to a new ssb folder, completely blank, fresh slate
    - disabled follow-back with pubs
    - got a pub invite
    - made changes to try and increase the initial sync perf
    - turns out one of the views doesn't work with an empty database
    - turns out what made the most difference is making the indexes available as soon as possible
    - if i query an index, i want to get what you have now, ready or not
        - i don't want to wait until it's done indexing
    - still a problem, indexes choke sbot
    - even if we make indexes fast, they will never be faster than 30 seconds, that's already too long
        - we need to make indexes faster, but also slower
        - we need to make sure sbot can do other things while indexing
        - it doesn't matter if indexing takes a day if we have interesting content to read
        - every type of indexing on our existing systems, once you start using the system the indexes go slow and give priority to the user's activity
    - indexing is too slow, no doubt
    - but we need to de-prioritize indexing, lazy index
    - did lots of experiments, but those were key takeaways
    - in hindsight, ebt and legacy gossip does parallel requests for feeds
        - on initial sync, you find out about feeds, then you request for them
        - sometimes you request the same feed hundreds of times in parallel
        - piet: i did this for the staltz mobile grant, could limit number of simultaneous ebt connections
            - matt: only pulling in a few feeds at a time?
            - piet: yeah, https://github.com/ssbc/ssb-replication-limiter
- mix: there's another topic around writing the flume view to disk on every message
    - keen for benchmarks on different flume views
    - matt: https://github.com/flumedb/atomic-file
- matt: multiple cursors are bad
- matt: one last thing about indexes,
    - i found last night, initially it was painfully slow
    - even more slower than indexing should be
    - from the profiler, found some bad observers, recent channels in a memory cache, was sucking up 20% of indexing time
- mix: one of the cases where when you're doing initial sync, maybe don't show anything
    - matt: that would be fine, but the problem is it's not just one pub, or not just pubs
    - matt: what is slow is the first person you follow
        - then you get brought to its knees
- matt: realized i never tried pub onboards, all before did local onboard testing
    - "why does anyone even bother using ssb?"
    - once i started getting the de-priority working, i could scroll around good content
    - will try and get this into Patchwork before i release next version
- mix: can you do that in a second bite?
    - matt: maybe, but i wanna get "ready or not" feature in
    - matt: just added ebt to Patchwork, which messed up progress bars
- progress bars, spinners, animations, and yum
- matt: i like the idea of the heart-beat thing
- piet: in terms of progress bars
    - maybe having different indications for different things happening
        - indexing
        - gossiping
    - my pet peeve with progress bars, when goes to 0 - 50 - 100 and back to 0 - 50 - 100
- matt: flashing indicators are cute
    - can see the real working of the machine
    - you often know you get information by leaking through
    - we should leak more of the internal machine state


## Resources

Fallback mumble server : blockades.org
