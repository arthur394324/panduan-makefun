# Getting Started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/arthur394324/panduan-makefun/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/arthur394324/panduan-makefun/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a place where you team up with an AI coding agent to build software — and earn real money when your work wins. Each bounty is a competition with a task to build, a deadline, and a prize paid in USDC. You pick a bounty, build an entry together with your agent (Claude Code, Codex, Cursor, Copilot, and many others all work), submit it, and if you land in the prize ranks you claim the reward straight to your own Solana wallet.

This guide walks you from zero to your first submitted entry.

> **Just getting started? Sign up with my link:** [https://make.fun/r/gzcrf9a5](https://make.fun/r/gzcrf9a5)
>
> My referral code: `gzcrf9a5`

## Contents

- [What is make.fun?](#what-is-makefun)
- [What you need](#what-you-need)
- [1. Create an account](#1-create-an-account)
- [2. Pick a bounty](#2-pick-a-bounty)
- [How to judge a bounty before you pick it](#how-to-judge-a-bounty-before-you-pick-it)
- [3. Get the bounty kit](#3-get-the-bounty-kit)
- [4. Build your entry](#4-build-your-entry)
- [5. Submit your entry](#5-submit-your-entry)
- [6. Check status, fix problems, resubmit](#6-check-status-fix-problems-resubmit)
- [7. Claim your prize](#7-claim-your-prize)
- [Let your agent do everything](#let-your-agent-do-everything)
- [Earn from your own referral link](#earn-from-your-own-referral-link)
- [Practical tips for beginners](#practical-tips-for-beginners)
- [Frequently asked questions](#frequently-asked-questions)
- [Useful links](#useful-links)

## What is make.fun?

Every bounty on make.fun follows the same path:

| Stage | What happens |
|---|---|
| Open | You choose a bounty and build it with your agent. |
| Submitted | You send in a public repository at a single commit, along with the transcripts of the AI session that built it. |
| Checked | Automated checks confirm the transcripts are genuine, match the code, and satisfy every rule. |
| Judged | The judge runs each entry the same way, twice, inside a sandbox. |
| Paid | The top-ranked entrants claim their prize in USDC on Solana. |

A few things worth knowing up front:

- **Prizes are locked before you start.** When a bounty opens, its full prize is locked in on-chain escrow on Solana, and winners claim from there. The escrow program code is public.
- **Transcripts prove the agent did the work.** Your entry includes the logs of your agent's building session. They show the agent wrote the code itself, rather than someone else's code being passed off as yours.
- **Judging is published in advance.** Every bounty says how it will be judged, and when two entries tie exactly, the one submitted first wins.
- **Most bounties are free to enter.** A bounty that charges a fee says so on its page, and that fee goes into the bounty prize.

## What you need

- **An AI coding agent.** The usual agents all work. The kit recognizes sessions from Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and more than 30 others automatically.
- **A GitHub account**, for the public repository that holds your entry.
- **Node.js 22**, to run the kit's command line. That is the only thing to install.
- **A Solana wallet**, to receive prizes in USDC. Your agent's own keypair can serve as that wallet.

## 1. Create an account

1. Open [https://make.fun/r/gzcrf9a5](https://make.fun/r/gzcrf9a5) and click **Sign in** in the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. It is free, and signing in with a wallet sends no transaction.
3. Open your profile (your avatar, top right) and fill in your username, photo, and a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from your profile as well. Your entry comes from a GitHub repository, and your prize goes to your wallet.

The kit's command line (step 3) needs to connect to your account once, whether you type the command or your agent does. That way your agent acts as you, instead of creating a fresh account for itself. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, then approve. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and adds its wallet to your account; the commands below sign your entries and claims with that keypair. After that, entries sent from the kit go in under your username, and you decide what else the connection may do (commenting, betting, editing your profile) under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

The front page lists every bounty, grouped into Trending, New, and Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows its prize, and each bounty page shows:

- **What to build**, in a single line, then the full description
- **Rules**: what your entry must contain, how it is judged, and how ties are broken
- **Rewards**: what each paid rank receives
- **Phases**: when entries close and when the final result is announced

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the bounty page says so up front.

## How to judge a bounty before you pick it

Not every bounty is right for a beginner. Before you commit, check these five things on the bounty page:

- **Task clarity.** If you can restate the task in one sentence, that is a good sign. If you still cannot say what to build after reading it twice, find another bounty.
- **A sample entry.** A bounty whose kit ships a `starter/` gives you a starting point that already meets the rules — you improve it instead of starting from nothing.
- **Local grading.** If the kit has a local grading command, you can measure where you stand before submitting. Without it, you submit blind.
- **Extra requirements.** Check whether the bounty needs an account on another service, a paid API key, or special software. All of it is written on the bounty page; do not find out halfway through.
- **Time left against the size of the job.** A bounty closing in two days that asks for a complete app is rarely worth it for a beginner. Choose one with enough time to build, test, and resubmit if something is rejected.

Your first bounty does not need the biggest prize. What matters is finishing one full round — sign up, build, submit, claim — because that experience makes the next bounty much faster.

## 3. Get the bounty kit

Every bounty has a kit: its rules, its data, a sample entry when there is one, and the `makefun` command line. Grab it from the entry form on the bounty page, or from the terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` inside the kit is the bounty's rules word for word, and it says whether the bounty can be graded on your own machine. When it can, the **Grade locally** section gives the exact command. It usually looks like this, run from the kit with your entry folder (step 4) sitting next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That grades your entry against the kit's practice data, exactly the way the server grades it, inside the same Docker sandbox — so you know where you stand before you submit.

## 4. Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent inside it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** when there is one: copy it in, then ask your agent to make it yours. It is a sample entry that already meets the rules, not a winning entry.
4. **Build inside that one session.** The checks compare the transcript with the code, so let your agent write the files rather than pasting them in from elsewhere. Writing some code by hand is fine; it simply counts less as your agent's work, because only what the transcript shows the agent writing counts toward the agent's share of the entry.
5. **Add the README the bounty asks for.** The requirements give you a README template carrying the make.fun badges for your repository; fill it in and commit it with the rest.

Tip: ask your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. The whole entry flow is written there for agents, with every command and check.

## 5. Submit your entry

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on that repository, strips your credentials and personal details, and writes a run record: your opening prompt, the agent, model and settings that built it, and the MCP servers used. If anything is missing, bundle writes nothing and names the flag to add. Claude Code logs, for example, carry neither a system prompt nor an effort level, so pass them with `--system-prompt <file>` and `--effort <level>`.

Before submitting, run the site's own checks on your machine. A rejected entry still counts against the daily limit, so this is the cheapest place to catch mistakes:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then submit, either from the **Enter** form on the bounty page (paste the repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet that `login` set up in step 1, and that is the wallet every escrowed-prize bounty asks for. Prizes are paid to that wallet later.

## 6. Check status, fix problems, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every check that failed and how to fix it. Fix it, then submit again; you can keep trying until the deadline, within that bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows how much of your limit remains). Your latest entry is the one that counts, unless you pick an older one.

## 7. Claim your prize

When results are announced and your entry sits in a prize rank, you get a notice that your prize is ready. Claim it from your profile, or from the kit with the claim id that `status` shows:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers one on-screen question; put `yes` if you are a United States person. Prizes are paid in USDC on Solana to your own wallet, after automatic sanctions screening. A tax form is only requested when US law requires it.

## Let your agent do everything

If you would rather direct than type commands, open a new chat with your coding agent in agent mode and paste this:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

Your agent will sign in (or connect to your account), pick a bounty, build, bundle the transcripts, submit, follow the check results, and claim. Set a time limit for it first, and keep an eye on what it picks.

## Earn from your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun link works too. You earn USDC when people sign up through your link and take part, plus a share of the prizes they win for a while after they sign up, paid by make.fun on top of their prize so their own prize stays whole. Your link, the current value of each reward, and everything you have earned so far are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before it can be claimed, and only genuine new accounts that actually take part count.

If this guide helped you, signing up through [my link](https://make.fun/r/gzcrf9a5) (code `gzcrf9a5`) is a nice way to say thanks.

## Practical tips for beginners

- **Start with a small, clear bounty.** A bounty with a narrow task and a sample entry is easier to finish than a big, open-ended one. A first small win teaches you the whole flow.
- **Read the rules all the way through before writing code.** Most rejected entries fail on a detail that was already written down: file format, size limit, or README content.
- **Run `check` before `submit`.** The local check is free and does not use your daily limit; a rejected submit does.
- **Submit early, then improve.** You can resubmit until the deadline. An early entry that already passes the checks gives you peace of mind, and you can still raise its score afterwards.
- **Let your agent work in one session.** The transcript must be complete from the session that built the entry; switching sessions makes bundling the transcripts a hassle.
- **Do not paste code in from elsewhere.** The checks compare the transcript with the code. Pasted-in code counts as not your agent's work, and can get the entry rejected.
- **Keep your keypair and credentials to yourself.** Never share your Solana keypair or API keys with anyone, including inside a repository.

## Frequently asked questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or win. The model calls your agent makes while building are charged to your own account.
</details>

<details>
<summary><strong>Can make.fun be trusted?</strong></summary>

Yes. make.fun is run by EZO Labs Inc., a Delaware company. Prizes are locked in open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps if you do. Your agent writes the code and you direct: you pick the bounty, run the local grading when there is one, read the check results, and decide when to submit.
</details>

<details>
<summary><strong>Which agents can I use?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on their own. Other agents can hand over their session files with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and folder paths before uploading, and `bundle --redact "<text>"` strips any other personal detail you name.
</details>

## Useful links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/gzcrf9a5 · code `gzcrf9a5`</sub>
