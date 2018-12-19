# retrospective #1

- date: 30/10/2018
- format: https://retrospectivewiki.org/index.php?title=Facebook_Reactions_Retrospective
- attendees
    - mikey
    - aljoscha
    - piet

## checkins

- [x]

## observations

### liked

- mik: the Sunrise Choir is a thing meow!
- mik: the train had no problem leaving the station and is building up steam
- alj: not working alone on stuff
- alj: high-bandwidth thought-sharing
- alj: a few other ssb people seem to actually care about this stuff
    - go peeps: keks, cryptix
    - christian bundy (needs a medal for improving js codebase)
    - moid, alanz
    - dominic
- pie: hitting some sweet milestones with napi + cross and stuff.
- mik: that we share our notes, willing to be open
- mik: seeing other people like our notes, dev diaries, etc
- mik: our meetings start with a checkin
- pie: did yoga yesterday and felt lots of emotions. Felt real nice afterwards.
- mik: seeing the amount of effort being put in by piet and aljoscha
- pie: being really disciplined with my timesheet.
- mik: these regular meetings every week, good rhythm
- alj: some problems with the spec getting promptly fixed in sbot
  - non-canonical base64 (dominc made a module really fast)
- alj: goofing around
- alj: interaction with rust crate devs
- pie: encouragement in dev diary thread from bob
    - %wLK+6isZG6emXAT+K1ZjL8lxVfaFeFzAAIbnpL6Ifkg=.sha256
- alj: "lightweight" meetings
    - unblock and/or give motivation

### loved

- pie: working beside Mikey once or twice. Arvo exercise was great.
- pie: putting in consistent hours at the library. It means I feel happy and tired at the end of the day.
- mik: seeing everyone's dev diary entries, seeing the progress that's happening
    - yay active radio comms (not radio silence)
- pie: writing rust
- pie: learned a _shit load_ in the last few weeks.
- alj: freedom
    - realized i could walk away from the computer and think for my work
- pie: being able to call Mikey and checkin that he's happy with progress. And hear that the problem I'd hit happened to be a known issue was a sweet bonus.
- mik: when we got to see aljoscha's face

### made me laugh

- pie: when my boss turned up to a picnic in a sweet dress. Best boss ever.
- mik: when piet calls me his boss
- mik: aljoscha being pedantic

### surprised me

- pie: prebuilding native modules is crazy complex.
- alj: got numb regarding weird ssb behavior
- alj: effects of regular dev diary writing
  - somewhat stressful. felt like had to reach milestones. learned to be ok with not
  - good to have to regularly communicate outside of the cave
  - gotten easier with time, more okay with sharing whatever
- alj: spec macro-organization is hard
- alj: feel like I'm alone with my level of detail of protocol knowledge, which makes communication difficult
  - maybe talk more about this later.
- alj: ~~time~~ self-mangement
- alj: time spend working vs time spend accomplishing something
- alj: thinking about my output as being paid for, being able to calculate "How much did this cost mik?"
- mik: aljoscha doesn't have a face in meetings.
- pie: it's taking quite a push to bring the native bindings thing to a useful point.


### made me sad

- pie: felt like I couldn't keep up with all of aljosha's work. Want to be able to engage and support but haven't that well.
    - don't want alj to feel alone fighting the demon
- alj: my English...
- alj: one month of work but still no "real" impact on the protocol
- alj: so much programming for programming's sake (appeasing the language, fitting into traits) rather than inherent complexity
- alj: quitting the tutoring job
- alj: no single stable place to work
- pie: I miss alj's face.
- mik: haven't been consistent in doing the things i say i'd do
- alj: (decimal representation of) floats
- pie: doing compilation on my laptop is a bit of a drag.
- alj: merkle trees not working out
  - mik: why does it work for dat and not ssb?
  - alj: ssb has a strong guarantee about past messages having existed in the past.
- mik + pie: hearing that aljoscha is having a tough time outside of work
- alj: having a tough time outside work =P
- alj: being the only angry one

### made me angry

- alj: corner cases everywhere, absolutely *everywhere*
- alj: floats
- alj: Lack of thorough engangement (at first) from Dominic on small but still crucial details

## discussion

### being alone in the details

punt (i.e. delay) until next week, we can spend time focusing on this then.

in the meantime, let's keep a watch on it, mindful in case it gets worse.

## actions

???

## normal meeting stuff

### bindings

- js object equivalent of https://github.com/ssbrs/legacy-msg-data/blob/master/src/value.rs#L138
- run [this](https://github.com/ssbrs/ssb-legacy-msg/blob/master/src/lib.rs#L27), then create a js object with the right keys (with the content type `T` being the js object from above)
- bindings to https://github.com/ssbrs/ssb-legacy-msg/blob/master/src/verify.rs

action:

- alj will write blurb about intended js api for rust module
- pie will handle making js bindings, try native and fallback to wasm
