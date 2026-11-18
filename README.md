# Start building for cash rewards on make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/zaynmalik99/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/zaynmalik99/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun turns coding into a paid contest. Bounties — real software tasks with a deadline and a cash reward in USDC — are posted on the site, you (and your AI coding agent) build the entry, you submit it, and if it places you claim the reward to your own Solana wallet. No resume, no interview, no client: just the build.

This guide is written for someone who has never touched make.fun. Follow it top to bottom and you will go from zero to a submitted entry.

> **Join through my link and we both benefit:** [https://make.fun/r/7hb33w1d](https://make.fun/r/7hb33w1d)
>
> My referral code is `7hb33w1d`.

## What is in this guide

- [What make.fun actually is](#what-makefun-actually-is)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Make your account](#step-1--make-your-account)
- [Step 2 — Choose a bounty worth your time](#step-2--choose-a-bounty-worth-your-time)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build the entry with your agent](#step-4--build-the-entry-with-your-agent)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Read the verdict, fix, resubmit](#step-6--read-the-verdict-fix-resubmit)
- [Step 7 — Claim the reward](#step-7--claim-the-reward)
- [Get paid for bringing people in](#get-paid-for-bringing-people-in)
- [Common questions](#common-questions)
- [Links](#links)

## What make.fun actually is

make.fun is a bounty board for software. Each bounty names one thing to build — a game, a tool, a trading bot, a forecast, a dashboard — plus how it is scored, when entries close, and what each paid place earns. The reward is real money: USDC on Solana.

A bounty runs through the same five stages every time:

1. **Open** — you pick it and build.
2. **Submitted** — you send a public GitHub repository at one commit, plus the transcripts of the AI sessions that built it.
3. **Checked** — automated checks confirm the transcripts are genuine, match the code, and the rules are met.
4. **Scored** — the grader runs every entry the same way, in a sandbox.
5. **Paid** — winners claim their reward in USDC on Solana.

Three facts that matter before you commit any time:

- **Most bounties cost nothing to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward.
- **The reward is locked before you start.** When a bounty opens, its whole reward sits in an on-chain escrow on Solana, and winners claim from it. The escrow program is open source at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your agent's session logs are part of the entry.** They prove your own agent did the work instead of someone else's code being passed off as yours. That is why you build in a fresh agent session, started after the bounty opens, and let the agent write the files.

### Three mistakes that get entries rejected

New entrants lose entries to the same three avoidable mistakes:

1. **Building in a session that started before the bounty opened.** Every transcript timestamp has to fall inside the bounty's window. Open a brand-new session after the opening time — resuming an old chat does not count.
2. **Copying code in instead of letting the agent write it.** The checks match your transcripts against the code. Pasted or downloaded code counts against you; code your agent typed in the session counts for you.
3. **Submitting without running `check` first.** A rejected submission still eats one of the day's limited submissions. Run the kit's `check` command locally (Step 5) and fix what it flags before you spend a submission.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider — the kit recognises the sessions of more than 30 of them on its own.
- **A GitHub account** for a public repository. Your entries come from your GitHub repositories, so the GitHub account has to be the one linked to your make.fun account.
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive USDC rewards. Your agent's own keypair can serve as that wallet.

### A quick checklist before your first build

- [ ] Account created and signed in on make.fun
- [ ] GitHub account linked on your make.fun profile
- [ ] Solana wallet linked (or your agent's keypair on the account)
- [ ] Node.js 22 installed (`node --version` prints v22 or later)
- [ ] The bounty's kit downloaded and `RULES.txt` read in full
- [ ] A fresh agent session opened after the bounty opened

## Step 1 — Make your account

1. Open [https://make.fun/r/7hb33w1d](https://make.fun/r/7hb33w1d) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from your profile too. Entries come from your GitHub repositories; rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of it making an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code matches, and approve it. If the command stops waiting before you approve, just run it again. The first login also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account; later commands sign your entries and claims with it. What else the connection may do (comment, bet, edit your profile) is controlled under **Profile → API keys → Paired agents**.

## Step 2 — Choose a bounty worth your time

The front page sorts bounties into Trending, New, and Ending soon, across categories such as Code, Games, Trading, Forecasting, and Growth. Every card shows the reward; every bounty page shows:

- **What to build**, in one line and then in full
- **Rules** — what an entry must contain, how it is scored, how ties are broken
- **Rewards** — what each paid place gets
- **Phases** — when entries close and when final results land

Read the rules before you write a line of code. Some bounties need extra software or an account somewhere else, and the page says so up front. Pick one where the scoring is clear and the deadline gives you real building time.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Grab it from the entry form on the bounty page, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word. It also says whether the bounty can be graded on your own machine; where it can, its **Grade locally** section gives the exact command, usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you submit anything.

## Step 4 — Build the entry with your agent

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder (the commands below call it `my-entry`), and open a fresh session of your agent in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules — not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from elsewhere. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

One tip that saves a lot of fumbling: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a piece is missing it writes nothing and names the flag to add. (A Claude Code log, for example, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.)

Before you enter, run the site's own checks on your own machine. A rejected entry still counts against the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet that `login` set up in Step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## Step 6 — Read the verdict, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, inside the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts.

## Step 7 — Claim the reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Get paid for bringing people in

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after they sign up — paid by make.fun on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped you get started, signing up through [my link](https://make.fun/r/7hb33w1d) (code `7hb33w1d`) is a good way to say thanks.

## Common questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but it is not required. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>How many times can I submit?</strong></summary>

Each bounty sets its own limit — usually a few submissions per day and a total cap. Your latest submission before the deadline is the one that counts, so spend submissions on versions you have already checked locally.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/7hb33w1d · code `7hb33w1d`</sub>
