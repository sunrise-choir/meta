# rusty sunrise choir :nut_and_bolt: :sunrise: :pray: 

hi folx,

i found some money and i'm keen to sponsor development towards the next generation of the core Scuttlebutt protocol.

**i'm keen to sponsor a team working on a future-proofed, maintainable, and accessible Scuttlebutt implementation in Rust.**

## backstory

i joined the Scuttlebutt project some 4 years ago, brimming with excitement and passion. despite being technically adept, i haven't contributed much code to the project, instead i play with cheerleading, community gardening, technical support, historical archeology, and whatever else i do.

as i've been here, i've noticed _so many_ people be very ideologically motivated to contribute, but then struggle and struggle and struggle with our tech stack and associated resources. for instance, i've noticed people excited about Scuttlebutt are more likely to contribute to Dat or IPFS, why is that?

as i see it, we're stuck in the "innovators" stage of the [technology adoption life cycle](https://en.wikipedia.org/wiki/Technology_adoption_life_cycle).

thanks to the generosity and leadership of people like [@dominic](@EMovhfIrFk4NihAKnRNhrfRaqIhBv1Wj8pTxJNgvCCY=.ed25519), we now have a well-designed core protocol and fully-functional reference implementation. at the same time, our core protocol has unspecified behaviors, missing context on fundamental design hypotheses, and knowledge centralized in a few minds. our reference implementation has cryptic code, scattered documentation, confusing modularity.

even to the core maintainers among us, we still struggle with understanding the next module where a bug crops up. not everyone is willing to spend hours trying to understand the deep poems within our source code, or know how to search for the "living documentation" of the protocol, or overcome feeling like an imposter when every step of the contribution process is a barrier.

to sum up, Scuttlebutt is an amazing innovation. yet it's inaccessible to new developers, unmaintainable by existing developers, and not yet prepared for the next stage of adoption. i want to help the project evolve to the next stage, the "early adopters". i think this means capturing all our learnings from Scuttlebutt today, designing the next generation of clear well-described specifications, and empowering the next generation of developers to implement, document, and test these.

what i'm not interested in: staying within the "innovator" stage. i'm not interested in re-inventing our wheels again, i think [@dominic](@EMovhfIrFk4NihAKnRNhrfRaqIhBv1Wj8pTxJNgvCCY=.ed25519) has done a really great job at building the core Scuttlebutt protocol and reference implementation, i want our evolution to focus on improving accessibility and maintainability. the only core protocol changes i'm really interested in are future-proofing, or changes that improve our ability to adapt to the unknown future.

## principles

i want a Scuttlebutt

- written in Rust or WebAssembly
- backwards-compatible with the existing network
- accessible to new developers
- maintainable by existing developers
- well-specified for humans to audit design
- well-documented for humans to grok implementation
- well-tested for computers to check implementation
- composed of orthogonal parts that each focus on a separate concern
- friendly to other human and computer languages
- designed for crypto upgrades we should expect
- designed for future changes we didn't expect 

## roadmap

(see previous personal wishlist: %9W+HJ7fPvNF8sxzJAVht/rx7zx/l3QS4e0EEv2wIui8=.sha256)

> measure twice, cut once

### minimum viable core

what are the fundamentals of the Scuttlebutt protocol?

1. capture the existing Scuttlebutt status quo
   - what is the problem space we are exploring?
   - what are our orthogonal solutions to cover the problem space?
   - what design hypotheses are our solutions trying to test?
   - what existing prior art exists to solve similar problems?
   - what have we learned so far from our exploration?
   - do we want to change any fundamental design decisions?
   - do we need to adapt any design details?
1. design a complete map for the next core Scuttlebutt protocol
1. write a thorough specifications for each part of the next core Scuttlebutt protocol
1. document accessible details of the next #ssbrs core protocol implementation
1. write maintainable tests for the next #ssbrs core protocol implementation
1. implement the next #ssbrs core protocol implementation
   - note: async modules may be delayed while the story for async Rust (`async` / `await`, [`futures`](https://docs.rs/futures)) is in limbo.

at this stage we MUST make hard decisions in order to limit our scope: what features are _must have_ versus what features are _nice to have_.

we SHOULD focus on changes that address well-known real-world problems with our current stack, or converging on better solutions given our learnings from exploring the problem space; we SHOULD NOT focus on changes for the sake of open-ended experimentation, or diverging from our current solutions to explore new territories within a larger problem space.

as a [straw proposal](https://en.wikipedia.org/wiki/Straw_man_proposal) for the next core protocol stack:

- data codec (#hsdt)
- multiplexing system (#bpmux)
- handshake and streaming cypher (secret-handshake, box-stream)
- private messages (private-box)
- database (flume logs and views)
- replication gossip (epidemic broadcast tree)
- flooded gossip (simple blobs, out-of-order messages)
- gossip scheduler (%HJTzjmTcr8FsKt34wmvN5ett4imneEzHp7dCab2rGpg=.sha256)
- minimal core rpc ([minsbot](%di/8i95Kx8pMLR8S2PrIQYiYaIHKo/n/DgmJB5vlM5s=.sha256))

features: 

- `ssb://` url identifiers ([urlsafe base64](%cx4sDkZEor3BJWlF6GfwXCStXSul3fPbxlLG+MDv2yQ=.sha256))
- [off-chain message content](%PJSLTxC9285oM1AlRuWEgb61BMHmHwoNMJgg/96FAHo=.sha256) #offchain-content / #blob-content
- friendly bindings to re-use Rust bindings in other languages
- gracefully upgrade core message formats, typed contents, etc
- gracefully upgrade crypto keys, hash functions, etc
- gracefully change data codecs, transports protocols, replication protocols, etc
- be able to delete a post or feed

resources:

- scuttlebutt.rs website
- how to contribute to #ssbrs (shared opinions, processes, etc)
- how to use the #ssbrs cli

packages:

- ssb cli client
- ssb cli server

this milestone SHOULD NOT become a moving target. in order for a proposal to be accepted into the minimum viable core milestone, it SHOULD be convergent. any proposals that fail to converge within a reasonable time will be re-targeted for the next milestone.

### next generation core

as a [straw proposal](https://en.wikipedia.org/wiki/Straw_man_proposal) for the next generation protocol stack:

modules:

- rpc plugins
- user invites (%LMYARKcJ5/HVrkfyGo0oSV4j/whFmpeiJlQnvPw53PE=.sha256)
- p2p relay (%Ci9CLOEmqt1/7ghJkHV76jzdWbB1cQxa92os7AShUB4=.sha256)
- multiple devices (#sameas)
- (private) groups
- dat blobs

packages:

- ssb pub server
  - external viewer
  - admin interface (stats, invites, etc)
- ssb mobile app
- ssb desktop app

resources:

- how to run an #ssbrs pub server
- how to build a mobile app on #ssbrs
- how to build a desktop app on #ssbrs

features:

- moderation
  - be able to block a feed (or block via following a list of blocked feeds)
  - flags: %QRrL2wc9o4LoPb2We8FbZC7WlbsfBRM7CnLeguNmILw=.sha256
- reactions: %+r7V7cUGV/S2qTIud9bYOiaz+NABIM9oQ17W9BLZC/E=.sha256
- i18n + l10n for user interfaces

## team roles

### member

a "member", in the context of the "rusty sunrise choir" team, is a person who has been accepted to be a part of the team.

### sponsor

a "sponsor" makes agreements to provide livelihood to "rusty sunrise choir" working members.

### navigator

the "navigator" manages the roadmap into achievable milestones, tracks progress, and cuts releases.

the "navigator" is the link between the sponsors and the developers, to ensure communication is as good as possible.

### developer

a "developer" works on tasks that make progress towards the roadmap.

these tasks may include design, documentation, testing, coding, reviews, or more.

### supporter

a "supporter" helps "developers" do well, which may include participating in conversations, giving feedback, or pairing on tasks.


## process

### how to become a "rusty sunrise choir" developer

to be funded for "rusty sunrise choir" work, you MUST be accepted as a developer on the team.

to express interest, you MUST reply to this message or contact the "navigator" privately.

"rusty sunrise choir" developers are paid a flat rate per day of work towards the current roadmap.

note: the "rusty sunrise choir" only sponsors a maximum of 20 working days per month.

the rate is negotiated by the "navigator" when you become a developer, with a default of:

```
(5000 USD per month / 20 working days per month) == 250 USD per working day
```

each "developer" will be matched with one or many "sponsor"s, as per the matching agreements to fund and be funded.

when the team changes membership, the "navigator" MUST post a public update to a relevant thread.

if a team developer wants to change their team commitment or decides to leave the team, they SHOULD give a least a months notice in advance of changes.

#### how to perform a day of developer work

you SHOULD work on something listed on the current roadmap. if you want to work on something not on the current roadmap, you SHOULD seek permission by the "navigator" before starting work.

at the start of your day, you MAY meet other team members for a standup.

at the end of your day, you SHOULD post a dev diary entry to a relevant thread in the "rusty sunrise choir".

your work SHOULD be licensed under an open source license (Apache-2.0, CC-BY 4.0, AGPL-3.0, or CC-BY-SA 4.0 preferred).

### how to be paid for a month of developer work

to be paid, you MUST send the "navigator" an invoice for the previous month's work (with a maximum of 20 days).

the "navigator" will forward your invoice to the relevant "sponsor"(s).

you MUST be able to legally accept payment in your local jurisdiction for your services.

### how to becomes a "rusty sunrise choir" sponsor

to fund "rusty sunrise choir" work, you MUST be accepted as a sponsor on the team.

to express interest, you MUST reply to this message or contact the "navigator" privately.

once you become a "sponsor", you will be matched with an incoming "developer"(s) based on your agreement to fund.

if a sponsor becomes unable to fulfill existing team obligations, they SHOULD give at least a months notice in advance of changes.

### how to become a "rusty sunrise choir" supporter

anyone can become a "rusty sunrise choir" supporter.

you MAY participate in "rusty sunrise choir" design discussions.

before giving feedback, you SHOULD ask a "rusty sunrise choir" developer if they want feedback.

you MAY offer to pair with a "rusty sunrise choir" developer.

## faq

### who is on the "rusty sunrise choir" team so far?

[@dinosaur](@6ilZq3kN0F+dXFHAPjAwMm87JEb/VdB+LC9eIMW3sa0=.ed25519) is the founder of the team, he will be playing the role of "sponsor", "navigator", and "supporter".

[@dinosaur](@6ilZq3kN0F+dXFHAPjAwMm87JEb/VdB+LC9eIMW3sa0=.ed25519) reached out privately to some butts in advance: [@piet](@U5GvOKP/YUza9k53DSXxT0mk3PIrnyAmessvNfZl5E0=.ed25519), [@matt](@FbGoHeEcePDG3Evemrc+hm+S77cXKf8BRQgkYinJggg=.ed25519), [@aljoscha](@zurF8X68ArfRM71dF3mKh36W0xDM8QmOnAS5bYOq8hA=.ed25519), [@dominic](@EMovhfIrFk4NihAKnRNhrfRaqIhBv1Wj8pTxJNgvCCY=.ed25519), [@mix](@ye+QM09iPcDJD6YvQYjoQc7sLF/IFhmNbEqgdzQo3lQ=.ed25519), [@andrestaltz](@QlCTpvY7p9ty2yOFrv1WU1AE88aoQc4Y7wYal7PFc+w=.ed25519)

[@piet](@U5GvOKP/YUza9k53DSXxT0mk3PIrnyAmessvNfZl5E0=.ed25519) and [@aljoscha](@zurF8X68ArfRM71dF3mKh36W0xDM8QmOnAS5bYOq8hA=.ed25519) have committed to be the initial developers.

[@matt](@FbGoHeEcePDG3Evemrc+hm+S77cXKf8BRQgkYinJggg=.ed25519) and [@mix](@ye+QM09iPcDJD6YvQYjoQc7sLF/IFhmNbEqgdzQo3lQ=.ed25519) have expressed interest.

### what is the initial budget?

[@dinosaur](@6ilZq3kN0F+dXFHAPjAwMm87JEb/VdB+LC9eIMW3sa0=.ed25519) has committed to sponsor 2 developers full-time for 6 months, or $60k USD.

```
20 days per month * $250 USD per day == $5k USD per month
```

```
$5k USD per developer per month * 2 developers * 6 months == $60k USD
```

### why would anyone be a sponsor?

the "return on investment" is a better Scuttlebutt, where _better_ is subjective.

### why the name "rusty sunrise choir"?

it's a placeholder name until we host a #naming-party.

- rusty: :nut_and_bolt: because we plan to get amongst the Rust programming language
- sunrise: :sunrise: because we are the start of a new day in the scuttleverse
- choir: :pray: because we sing the Chorus
