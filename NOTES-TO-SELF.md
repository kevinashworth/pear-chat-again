# Pear Chat - Notes to self - pear-chat-again

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

## Step 3

Follow along with [Touch, set the upgrade link, and seed](https://docs.pears.com/p2p/getting-started/build-a-peer-to-peer-chat/ship/#touch-set-the-upgrade-link-and-seed):

I have a version of Pear Chat running on Mascot and another one running on kMBA. I suppose that is what to do first?

1. I ran:

```zsh
pear touch
```

Result:

```text
pear://y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy

Deployment operations
  stage      Local/dev checks, unsigned builds, branches, throwaways
  provision  Prerelease, stakeholder preview, QA, or dogfooding
  multisig   Production, stakeholder multisig, machine-independent


Use the link above
  $ pear stage --dry-run pear://y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy <dir>
```

2. Then I ran:

```zsh
npm pkg set upgrade=pear://y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy
```

There was on command-line output but `package.json`'s `upgrade` was changed to the above.

3. Then I ran:

```zsh
pear seed pear://y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy
```

I left it running. Its output was approximately:

```text
Seeding:         pear://y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy
Verlink:         pear://0.0.y55qf5xo818ygibi787mif35phg1ofn9rxexca1gnjnwjxc335zy
App:             -
Drive Length:    0   [100%]
Blobs Length:    0/0 [  0%]
Blobs Size:      0B
Discovery Key:   9jwmmwgw9qwje4me6tteak6uj5rnr8tjqp4tz6ubbcn7mxa7wawo
Content Key:     pending
Firewalled:      true
NAT Type:        consistent
Whoami:          zibjoe1hip3zdh7kj8yahzkdixyrs965sbwjadsfgm6u5ckjajhy
Uptime:          30s
Network:         [ Peers 0 ]  [ ⬆ 0B - 0B/s ]  [ ⬇ 0B - 0B/s ]
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
^_^ announced
```

What do I do now? I guess I run `npm version patch`. I have to commit changes above plus to package.json first.

And then, since I seem to have all the other files listed on this page, I ran

```zsh
npm run make
```

The output didn't change for 30 minutes or so, and I typed ctrl-C, and tried again.

After some chat with LLM, determined it was an `npm install` issue due to using `npq` and install scripts not running. Reinstalled and the `make` command completed. Adding an `.npmrc` file to this repo with ignore-scripts=false`. I am not liking having to undo these security steps, but for now it seems the way to go.

2026-11-12

Coming back after a few days.

1. Committed changes to kevin/part-3 ... might push soon
2. Ran `pear seed [...]` as above
3. Opened `PearChat.app` in Mascot's `Applications` folder, and saw a `peer join` event on the still-running `pear seed`
4. Went to kMBA ... oh yeah, I did a bunch here due to install scripts blocked messages from npm ... will push & merge & pull to deal with this
