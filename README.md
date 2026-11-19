# Make.fun, from zero to your first bounty

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/sixgod383/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/sixgod383/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**New to make.fun? Start here: [https://make.fun/r/6s2kbv28](https://make.fun/r/6s2kbv28) — referral code `6s2kbv28`.**

make.fun is a competition platform where you and an AI coding agent build real software for cash prizes. A bounty posts a task, a deadline and a prize pool; you build an entry, submit it, and if it places you claim the prize in USDC on Solana. This guide walks you from a blank account to a submitted entry.

## What make.fun actually is

Think of it as a hackathon that never closes. At any moment there are open bounties for games, tools, data visuals, trading bots, forecasting models and more. Each bounty page tells you exactly what to build, how it will be scored, what each paid place wins, and when entries close.

Three things set it apart from a freelance job board:

- **The prize is locked up before you start.** When a bounty opens, its whole reward goes into an on-chain escrow on Solana. Winners claim from that escrow, so the money is there whether or not the organisers feel generous later.
- **Your agent does the building.** You steer; an AI coding agent (Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode and dozens of others) writes the code. You pick the bounty, read the rules, check the work and decide when to submit.
- **Entries are checked, not trusted.** Every submission carries the transcripts of the agent sessions that built it, so the platform can verify your own agent did the work. The grader runs every entry the same way, in a sandbox, usually twice.

Most bounties cost nothing to enter. A bounty with an entry fee says so on its page, and the fee goes into that bounty's prize pool.

## What you need before you start

- **An AI coding agent.** Any of the common ones works. If you have never used one, Claude Code and Codex are good first picks; Cursor and Copilot work too.
- **A GitHub account.** Entries are public GitHub repositories (a few bounties accept a source bundle instead). You will also need it to sign in.
- **Node.js 22 or newer**, to run the kit's command line. Nothing else to install.
- **A Solana wallet**, to receive USDC rewards. Your agent can create a keypair for you, and that keypair doubles as the wallet.

## Step 1 — Create your account

1. Open [my referral link](https://make.fun/r/6s2kbv28) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing.
3. Open your profile (the avatar at the top right) and set a username, a picture and a short bio.
4. If you signed in some other way, link your GitHub account and a Solana wallet from the profile too. Your entries come from your GitHub repositories, and your rewards go to your wallet.

If you plan to let an agent enter bounties for you, connect it once from a bounty kit:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code and approve. From then on, entries sent from that machine go in under your username. Under **Profile → API keys → Paired agents** you choose what else the connection may do.

## Step 2 — Pick a bounty

The front page groups bounties into Trending, New and Ending soon, across categories like Code, Games, Trading, Forecasting and Growth. Every card shows the prize pool; the bounty page shows the rest:

- **The task**, in one line, then the full description and rules
- **How it is scored**, including how ties are broken (usually the earlier entry wins)
- **The prize split**, place by place
- **The phases**, when entries close and when results land

Read the rules before you build. Some bounties need extra software, an account on another service, or a specific runtime, and the page says so up front. Pick one you can actually finish before the deadline — a small finished entry beats an ambitious half-built one.

## Step 3 — Get the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Download it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. It also says whether you can grade your own entry locally before submitting, and gives the exact command when you can.

## Step 4 — Build your entry

1. **Start a fresh agent session after the bounty opens.** Sessions that began before the opening cannot be entered, so do not reuse an old chat.
2. **Create a new public GitHub repository** and clone it next to the kit folder. Add an open-source license — MIT is the usual choice.
3. **Copy in the kit's `starter/`** if it has one, then make it your own. The starter is a working entry that meets the rules, not a winner.
4. **Build in that one session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting code in from elsewhere. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Run the local grade** if the kit offers one, and fix what it flags.

One tip worth its weight: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command, every check, every edge case.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects the sessions that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and tells you which flag to add.

Then run the site's own pre-submit checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, from the **Enter** form on the bounty page or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair <your-keypair.json>
```

`--keypair` signs the entry with the wallet `login` set up in step 1. That wallet is where the reward will be paid.

## Step 6 — Results and claiming

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: submitted, checked, scored or rejected. A rejected entry lists every failed check with its fix — apply the fixes and submit again before the deadline, within the bounty's daily limit. Your latest entry before the deadline is the one that counts.

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where the law requires one.

Each bounty sets its own submission limits — this one, for example, allows at most 5 submissions per UTC day and 10 in total — so do not burn attempts on entries you have not checked locally first.

## Mistakes beginners make

- **Reusing a session from before the bounty opened.** The transcript gate rejects it outright. Start fresh, after the opening.
- **Pasting code in from somewhere else.** Lines your agent did not write in the session count against you, and lines copied from another entry can get the entry refused.
- **Forgetting the license.** Entries must carry an OSI-approved license at the repository root. No license, no entry.
- **Submitting without running `check` first.** A rejected submission still counts toward the day's limit. Run the local checks, fix what they flag, then submit.
- **Leaving the repository private, or renaming it after submitting.** The final check reads the repository again at the same link; a private or vanished repository is disqualified.
- **Waiting until the last hour.** Uploads, checks and fixes all take time, and the deadline does not move. Submit early, then improve and resubmit if you have attempts left.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works the same way. When someone signs up through your link and takes part, you earn referral rewards, plus a share of what they win for a while after they sign up — paid by make.fun on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Each reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/6s2kbv28) (code `6s2kbv28`) is a good way to say thanks.

## Common questions

**Does it cost anything to enter?**
Most bounties are free to enter. The model calls your agent makes while it builds are on your own account.

**Is make.fun legitimate?**
It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens, and the escrow program's code is public. It is not makefun.ai, not the makefun Python library and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps, but your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [This bounty's page](https://make.fun/bounty/bring-the-humans-02)
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/6s2kbv28 · code `6s2kbv28`</sub>
