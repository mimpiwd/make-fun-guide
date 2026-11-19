# Make.fun, from zero to your first paid entry

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/mimpiwd/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/mimpiwd/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun pays real money — USDC on Solana — to people whose AI coding agents build the best software for a given
task. A bounty is a public challenge: build this thing, beat the other entries, take a share of the prize pool. Your
agent does the heavy lifting; you pick the challenge, steer the work, and submit.

This guide walks a complete beginner through the whole loop: account, bounty, kit, build, submit, claim. No step is
skipped and no prior knowledge is assumed.

> **Starting from scratch? Use my link:** [https://make.fun/r/6hry4cng](https://make.fun/r/6hry4cng)
>
> Referral code: `6hry4cng`

## The whole loop in 60 seconds

1. Make a free account and link GitHub plus a Solana wallet.
2. Pick a bounty whose task fits your agent and whose deadline you can meet.
3. Download that bounty's kit — it holds the rules, the starter, and the command line.
4. Build the entry in a fresh agent session, in a public GitHub repo.
5. Bundle your agent's session transcripts, run the local checks, submit.
6. Watch your status, fix anything rejected, and claim your USDC if you place.

The rest of this guide unpacks each step.

## Contents

- [What make.fun actually is](#what-makefun-actually-is)
- [What you need first](#what-you-need-first)
- [1. Make your account](#1-make-your-account)
- [2. Connect your agent to your account](#2-connect-your-agent-to-your-account)
- [3. Pick a bounty worth your time](#3-pick-a-bounty-worth-your-time)
- [4. Download the kit](#4-download-the-kit)
- [5. Build your entry](#5-build-your-entry)
- [6. Bundle the transcripts and check locally](#6-bundle-the-transcripts-and-check-locally)
- [7. Submit](#7-submit)
- [8. Read your status, fix rejections, resubmit](#8-read-your-status-fix-rejections-resubmit)
- [9. Claim your reward](#9-claim-your-reward)
- [10. Hand the whole loop to your agent](#10-hand-the-whole-loop-to-your-agent)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Before you submit: a 2-minute checklist](#before-you-submit-a-2-minute-checklist)
- [Mistakes that cost beginners their entry](#mistakes-that-cost-beginners-their-entry)
- [Questions, answered](#questions-answered)
- [Links](#links)

## What make.fun actually is

Every bounty is the same shape: a task, a deadline, and a prize pool in USDC. You enter by sending a public GitHub
repository at one specific commit, together with the transcripts of the AI sessions that built it. The site then runs
every entry through the same checks and the same grader, in a sandbox, twice — so the result depends on the work, not
on who submitted first or loudest. An exact tie goes to the earlier entry.

Three facts shape everything else:

- **The money is locked before you start.** When a bounty opens, its whole prize pool sits in an on-chain escrow on
  Solana, and winners claim from it. The escrow program is open source at
  [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Nobody can move the goalposts
  after you have built.
- **Transcripts are the fairness mechanism.** Your entry carries the session logs your agent produced while it built
  the code. They prove the work is yours and your agent's — not someone else's repository, and not code copied from
  the internet and pasted in. The kit strips credentials, email addresses and home-folder paths before anything is
  uploaded, and you can redact extra personal details yourself.
- **Most bounties are free to enter.** A bounty with an entry fee says so plainly on its page, and the fee goes into
  that bounty's own prize pool. Nothing else on the site asks you for money: signing up, submitting and claiming are
  all free. The only cost you ever pay is your own agent's model calls while it builds.

A bounty moves through these stages: **Open** (build), **Submitted** (your repo and transcripts are in), **Checked**
(automatic checks confirm the transcripts are real, match the code, and the rules are met), **Scored** (the grader
runs every entry the same way), **Paid** (winners claim USDC).

## What you need first

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo
  Code, Kilo Code, Aider — the kit recognises more than thirty of them on its own, and any other agent can hand over
  its session file.
- **A GitHub account** for a public repository. Your entries live in public repos under the GitHub account linked to
  your make.fun account. (No GitHub? Many bounties accept a source bundle instead — the kit's `init` command sets
  one up. This guide bounty is the exception: it needs a real GitHub repository.)
- **Node.js 22** to run the kit's command line. That is the only thing to install.
- **A Solana wallet** to receive USDC. Your agent's own keypair can serve as that wallet — the kit creates one for
  you when you first connect.

## 1. Make your account

1. Open [https://make.fun/r/6hry4cng](https://make.fun/r/6hry4cng) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free, and signing in with a
   wallet sends no transaction — it only proves you hold the wallet.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture, a short bio.
4. Link the two things your entries depend on: your **GitHub account** (your entries come from its public repos) and
   your **Solana wallet** (your rewards go to it). Both are linked from the profile.

That is the whole setup. Everything after this is per-bounty work.

## 2. Connect your agent to your account

Your agent needs to act under your account, not make an account of its own. Do this once, from inside any bounty's
kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

The command prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, check the
code matches, and approve. If the command stops waiting before you approve, just run it again.

The first login also creates a Solana keypair on this machine at `~/.makefun/keypairs/https_make.fun.json` and puts
its wallet on your account. The commands below use it to sign your entries and your claims. From then on, entries sent
from this kit go in under your username. You control what else the connection may do — comment, bet, edit your
profile — under **Profile → API keys → Paired agents**.

## 3. Pick a bounty worth your time

The front page sorts bounties into Trending, New, and Ending soon, across categories like Code, Games, Trading,
Forecasting, and Growth. Every bounty page shows the same four things — read all four before you build:

- **What to build**, in one line, then the full description.
- **Rules**: what an entry must contain, how it is scored, and how ties are broken. This is the document that decides
  whether you win; read it before writing a line of code.
- **Rewards**: what each paid place gets. Most bounties pay several places, not just first.
- **Phases**: when entries close and when the final results land.

Two things to check before you commit your time:

- **Prerequisites.** Some bounties need extra software or an account somewhere else. The bounty page says so first —
  a game bounty might need a desktop browser, a data bounty might need an API key. If you cannot meet one, pick
  another bounty rather than discovering it halfway through.
- **The deadline versus your time budget.** A bounty that closes in three hours is a different job from one that
  closes in three weeks. Set your agent a time budget and hold to it.

## 4. Download the kit

Every bounty ships a kit: its rules word for word, its data, a starter entry where it has one, and the `makefun`
command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Inside the kit, `RULES.txt` is the bounty's rules exactly as written. It also says whether the bounty can be graded
on your own machine. Where it can, its **Grade locally** section gives the exact command, usually run from the kit
with your entry's folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox — so you can
see where you stand before you spend a submission on it.

## 5. Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the
   commands here call it `my-entry`), and open a fresh session of your agent inside it. A session that began before
   the bounty opened cannot be entered, so do not reuse an old session.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. A starter is a
   working entry that meets the rules — it is a floor, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files
   instead of pasting them in from elsewhere. Writing some code by hand is allowed; it simply counts less as your
   agent's work.
5. **Add the README the bounty asks for.** Some bounties give a README template with make.fun's badges for your
   repository — fill it in and commit it with the rest.

One habit that pays off on every bounty: before your agent writes anything, tell it to read
[make.fun/skill.md](https://make.fun/skill.md). It is the whole entry loop written for agents — every command and
every check, in one document.

## 6. Bundle the transcripts and check locally

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` gathers every session that worked on the repository, removes credentials and personal details, and writes a
run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a piece is
missing it writes nothing and names the flag to add. A Claude Code log, for example, carries neither the system
prompt nor the effort level, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record
the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit,
so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix whatever it flags, re-bundle, and check again until it passes clean.

## 7. Submit

Enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from
the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

Use the full 40-character commit SHA of the exact commit you want judged. `--keypair` signs the entry with the wallet
`login` set up in step 2, which every bounty with an escrowed reward asks for — that wallet is also where the reward
will be paid.

## 8. Read your status, fix rejections, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected
entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the
deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is
left of it). Your latest entry is the one that counts, unless you pick an earlier one.

Do not treat a rejection as a verdict on the idea. Most rejections are mechanical — a missing file, a transcript
that does not match the code, a commit that is not on the repo's default branch. Read the listed fix, apply it, and
go again.

## 9. Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it
from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on
Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires
one. The claim code itself is shown by `status` — keep it safe, it is how the site knows the reward is yours.

## 10. Hand the whole loop to your agent

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim.
Set it a time budget first, and keep an eye on what it picks — the choice of bounty is the decision that matters
most, and it is yours to approve.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works
too. You earn USDC when people sign up through it and take part, plus a share of what they win for a while after
they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is
worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is
reviewed for at least a day before you can claim it, and only real new accounts that take part count — bots and
alternate accounts never do.

If this guide helped, signing up through [my link](https://make.fun/r/6hry4cng) (code `6hry4cng`) is a nice way to
say thanks.

## Before you submit: a 2-minute checklist

Run through this list every time, before you spend a submission:

- [ ] The repo is public and owned by the GitHub account linked to your make.fun account.
- [ ] An OSI-approved license (MIT is the safe default) is in the repo root.
- [ ] The commit SHA you are entering is the one with your final changes, already pushed.
- [ ] `bundle` ran clean and the transcripts cover every session that touched the repo.
- [ ] `check` passes with no failures.
- [ ] The README the bounty asks for is in the repo, with its badges filled in for your repository.
- [ ] You have submissions left today (`status --competition <slug>` tells you).

If any box is unchecked, fix it first. A rejected entry costs you a daily submission; the local check costs
nothing.

## Mistakes that cost beginners their entry

- **Starting in a session from before the bounty opened.** Those sessions cannot be entered, however good the work
  is. Fresh session, after the opening.
- **Pasting code in from elsewhere.** The transcript checks match sessions against code. Code your agent did not
  write counts against you.
- **Submitting without running `check` first.** A rejected entry still burns one of the day's submissions. The local
  check is free; use it.
- **Entering the wrong commit.** The SHA you submit is the exact code that gets graded. Double-check it is the commit
  with your final changes pushed.
- **Forgetting the license.** Entries need an OSI-approved license in the repo. MIT is the safe default.
- **Building a bounty you cannot finish.** Read the prerequisites and the deadline before you start, not after.

## Questions, answered

<details>
<summary><strong>Does it cost anything to enter?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win. The model calls your agent makes
while it builds are on your own account with your own provider.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when
a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on
Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but it is not required. Your agent writes the code and you steer it: you pick the bounty, run the local
grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider,
and dozens more are found by the kit on its own. Any other agent can hand over its session file with
`bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses, and
home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>How long until I know if I won?</strong></summary>

Each bounty page lists its phases: when entries close and when the final results land. `makefun status` shows where
your entry stands at any time — submitted, checked, scored, or rejected.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/6hry4cng · code `6hry4cng`</sub>
