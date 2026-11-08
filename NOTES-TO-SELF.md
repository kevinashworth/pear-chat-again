# Pear Chat - Notes to self

## Getting Started

2026-11-06

Coming back to this `pear` folder after a few months, going to start with part 1 of 4 in the getting started path.

Getting started: https://docs.pears.com/p2p/getting-started/
Build a peer-to-peer chat Part 1: https://docs.pears.com/p2p/getting-started/build-a-peer-to-peer-chat/build-a-peer-to-peer-chat/

When I decided to add a `src` folder, the 2 side-by-side instances of the Pear Chat did not connect. I wondered what I had done wrong, but the LLM told me it was a rate-limiting thing:

> Your peers counter only moves once one worker actually finds and connects to the other, which depends on the public DHT, not on either window doing anything. Each worker must announce itself to the DHT's bootstrap servers and then keep querying for other peers on the same topic, but the bootstrap servers deliberately throttle clients that announce and look up too often from one address — every restart of both of your instances re-announcing, plus my repeated diagnostic joins earlier, all counted against the same quota, which throttles their ability to respond. On top of that, once announced, a fresh peer lookup has to find your peer before anyone can query you, and the protocol retries lookups only every few minutes. So even with everything configured right, the announce-lookup-connect chain can take minutes under load, and the "connected to swarm" line only confirms the _announce_ step — it says nothing about whether any _lookup_ will return a peer yet. That's why the swarm joined quickly while the peers counter stayed at zero until the throttle cooled down.

2026-11-07

I see that there are 3 bootstrap IP addresses that the Hyperswarm might ping. https://github.com/holepunchto/hyperdht/blob/fe0c9707f1b9fa5def9b4a7308f559aa0bbab84b/lib/constants.js#L17-L21

I'm continuing to use my `src` directory, so making several changes as I begin part 2. It should work, but it means I'm doing more work than blindly following along. Should help me learn more, no?

The alias I have been using in `~/.zshrc` (npm = npq-hero) led to some weird bugs. I changed that alias. (Hmm ... a bigger concern than this project, but that gets part 2 of Getting Started working again.)

I made small changes, such as adding username to the header.

2026-11-08

LLM: "The only real cost of the current layout is that src/workers/chat-room.js depends on something outside src. That works today because Forge packages the whole project, but it would break if you ever added a bundler or packaging filter scoped to src/. Worth a comment or a note in the README."

Commands I am using in this dir:
eza --long --header --icons --git --all
eza --long --header --icons --git --all --git-ignore
npm start -- --storage /tmp/pear-chat-user1 --name user1
npm start -- --storage /tmp/pear-chat-user2 --name user2 --invite CODEGOESHERE
