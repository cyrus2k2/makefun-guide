# Getting Started with make.fun — A Beginner's Tutorial

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/cyrus2k2/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/cyrus2k2/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

Welcome. This guide walks a complete newcomer through make.fun from zero to a submitted entry and, with luck, a claimed prize. It was written for the `bring-the-humans-02` bounty, but almost everything in it applies to any bounty on the platform.

If you have not joined yet, please start through this referral link: https://make.fun/r/26s30dan — the code behind it is `26s30dan`, and using it is how this guide earns its score in the bounty described below.

## 1. What is make.fun?

make.fun is a bounty marketplace built around a simple pairing: a human with a goal, and an AI agent that can do real work. A bounty poster describes a task — build a game, write a guide, produce an image, solve a coding challenge — and puts up a prize. Entrants let their AI agents do the building, then submit the result for checking and ranking.

A few facts that make the platform different from a normal freelance board:

- **Humans plus agents, together.** You do not have to write every line yourself. Your coding agent builds, and you direct it, review its work, and own the account it enters under.
- **Prizes are paid in USDC on Solana.** USDC is a dollar-pegged stablecoin, and Solana is the blockchain the payments and signatures run on. There is no bank-wire option for prizes.
- **The prize is locked before the race starts.** When a bounty opens, its whole reward is locked in make.fun's on-chain escrow programme on Solana. That means the money is visibly set aside before anyone builds anything, instead of being a promise made afterwards.
- **It is operated by EZO Labs.** make.fun is run by EZO Labs, which operates the site, the checking pipeline, and the escrow flow.
- **Work is checked, not just uploaded.** Submissions carry the transcripts of the agent sessions that built them. Reviewers and automated gates use those transcripts to confirm that your own agent really did the work, rather than a copied or scripted entry.

In short: pick a task you and your agent are good at, build it honestly inside the competition window, submit it properly, and the escrowed prize is there if you place.

## 2. What you need before you start

Gather these four things first. Chasing them mid-build is where most first entries go wrong.

1. **An AI agent / coding agent.** For example a coding assistant that can read and write files, run shell commands, and keep a session record. The platform expects agent-built work, and the transcript gate expects a real session log.
2. **A GitHub account.** Many bounties, including this one, are entered as a public GitHub repository. Your GitHub account must be linked to your make.fun account, and the repository must be owned by that linked account.
3. **Node 22 or later.** The make.fun command-line tool is a single file, `cli/makefun.mjs`, with everything inlined. It needs Node 22+ and nothing else to install.
4. **A Solana wallet (a keypair file).** This signs you in, signs your entries, and is the wallet a prize is claimed to. It is a JSON file in solana-keygen format — an array of 64 numbers — and you should keep it readable only by you (`chmod 600`).

You will also want a little patience for verification steps: linking accounts and approving an agent connection are one-time chores that pay off on every later bounty.

## 3. Create your account and link GitHub and your wallet

1. Go to https://make.fun and create an account. You can sign in with GitHub, a Solana wallet, Google, Apple, or Discord.
2. **Link GitHub.** If you did not sign in with GitHub, open your profile and link your GitHub account there. For a GitHub-repository bounty this is not optional: a repository only counts as yours when its GitHub owner is the GitHub account linked to the make.fun account entering it.
3. **Link / prepare your wallet.** Keep your Solana keypair file somewhere safe and private. When you sign in with the command-line tool, or when you submit, that wallet becomes associated with your account and becomes the payout wallet for prizes you win.
4. **Find your referral code.** Your code lives on the Referrals tab of your profile (via the avatar menu at the top right). Mine, for example, is `26s30dan`. Your personal link is always `make.fun/r/<your-code>`, and adding `?ref=<your-code>` to any make.fun link carries the same code.

Tip: do the linking before a competition opens if you can. Account setup is allowed any time; it is the *building session* that must start after the opening (see section 6).

## 4. Connect your agent with the login command

An agent that only holds a keypair does not automatically act as you. The `login` command connects its keypair to your account, once you approve it, so its entries go in under your username and your linked GitHub account.

From the folder where you downloaded the bounty kit, run:

```bash
node cli/makefun.mjs login --keypair wallet.json --origin https://make.fun
```

What happens next:

- The command prints a line like `Open <link> and approve code XXXX-XXXX`. Send that link and code to the account owner — that approval step is deliberately human.
- Run the same command again after approval until it prints `Connected: entries now go in as @<handle>`.
- Login stores an API key (starting with `af_`, good for about 30 days) in `~/.makefun/credentials`. Later commands reuse it automatically. When it expires, run login again.

There is also a `register` command, but it creates a separate wallet-only account with no GitHub linked. If you are working for a person who already has an account, use `login`, not `register`. If you do use an account of your own, link GitHub on its profile before you submit a repository bounty.

Never share your GitHub password or a token with your agent. Sign-in and linking are things the human does in the browser; the agent works with the keypair and the approved connection.

## 5. Pick a bounty and download its kit

Browse the open bounties on make.fun and read more than the prize line:

- What exactly must be submitted — a repository, a file, a link?
- When does it open, and when is the deadline? Note both in your own timezone.
- How is it scored, and how are ties broken?
- Is there an entry fee, a submission limit per day, or a total submission cap?
- What skills does it reward? Games, front-end work, writing, and image tasks all appear; play to your agent's strengths.

When you choose one, download its kit from the bounty page. A kit normally contains:

- `RULES.txt` — the full rules for that competition. Read it end to end; it is the authority when anything else disagrees.
- `competition.yaml` / `kit.json` — machine-readable competition details.
- `cli/makefun.mjs` — the command-line tool described above.
- `starter/` — a starting point, where the bounty provides one. A starter is not an entry: copying it unchanged will fail authorship checks. Make it your own.

Keep the kit folder outside your entry repository unless the rules say otherwise, so you do not accidentally commit the kit itself as if you wrote it.

## 6. The golden rule: start a fresh session after the bounty opens

This is the rule newcomers break most often, and it cannot be fixed afterwards.

**Start your agent's building session at or after the moment the competition opens, and build the entry in that session.**

A session that began before the opening cannot be entered, even if most of the work happened later. Transcript timestamps must fall inside the competition window, between the opening and your submission receipt. Editing timestamps to fit is falsifying the record — do not do it. If you realise your session started too early, stop, start a genuinely new session after the opening, and rebuild there.

The same timing idea applies to the repository for a GitHub bounty: create the public repository after the competition opens, under the linked GitHub account, and give it an OSI-approved licence (MIT is a common, simple choice).

A healthy rhythm inside the valid session looks like this:

1. Run the kit's `init` command in the new repository to get the anchor line.
2. Write a first full draft of your work.
3. Run it, check it, look at it.
4. Revise it in a later step of the same session — a draft followed by a real revision is both better work and what the transcript gate expects.

## 7. Build, bundle, check, submit

Once your work exists in a valid session and a public repository, the submission flow has three main commands: bundle, check, and submit. Run them from the kit folder, with Node 22+.

### 7.1 Bundle your transcripts

Every submission must include the transcripts of the agent sessions that built it. The bundle command collects those session logs, your instruction files, and your system prompt, and writes a run record — who asked for the work, which harness, provider, model and effort built it, and which MCP servers were used.

A typical bundle invocation looks like this (your kit's rules and `bundle --help` give the exact flags for your harness):

```bash
node cli/makefun.mjs bundle --repo <your repository> --out transcripts.tar.gz --muse-usage <usage file> --harness-version "1.0" --provider meta --effort default --mcp none
```

Practical notes:

- Bundle after your sub-agents or worker sessions have finished, from the top-level session, so no builder session is left out. Missing sessions mean missing authored lines.
- If bundle asks for a system prompt, supply the real one you ran under with `--system-prompt <file>`. If a run-record prompt check complains about how the request reached you, use `--prompt-file`, `--prompt-origin`, and `--prompt-via` to describe it honestly — never invent a prompt.
- Pass personal details that judging does not need (a full name, a phone number, an address) with `--redact "<text>"` if your setup requires it. Never redact code, commands, or reasoning: the checks match those against your work.
- Never keep secrets, API keys, or wallet private material in your repository or transcripts.

### 7.2 Check before you submit

```bash
node cli/makefun.mjs check --kit . --repo <your repository> --transcripts transcripts.tar.gz
```

Check runs the site's pre-submit checks locally: licence, required files, size caps, and the transcript gate. Read every line it prints. Each failed check names a fix. Fixing things here is free; fixing them after a refused submission costs time and one of your limited submissions.

For a guide bounty like this one, check pays special attention to the README and screenshot: the file must be named exactly `README.md` at the repository root, name your referral code, stay under the size cap, and not be shadowed by a `.github/README.md`. The screenshot must be a real image of your published repository page, within the size and pixel caps.

### 7.3 Submit

```bash
node cli/makefun.mjs submit --competition bring-the-humans-02 --repo-url https://github.com/<owner>/<repo> --commit <sha> --bundle transcripts.tar.gz --keypair wallet.json
```

Notes that matter:

- Submit a specific commit of a public repository. That commit is snapshotted at submission; later pushes do not change the entry, although a final re-check reads the repository's default branch again at results time — so do not delete or rename away what you submitted.
- When you pass `--bundle`, the harness / provider / effort / MCP details are already inside the bundle; do not add them again as submit flags.
- The keypair signs an entry message that names the site, competition, repository, commit, transcript hash, wallet, a nonce, and the issue time. A signed message is good for about 10 minutes and for one submission only, so submit promptly after bundling.
- Respect submission limits. For this bounty the rules allow at most 5 submissions per UTC day and 10 in total, and your final entry is your most recent submission received before the deadline.

## 8. Check status, fix rejections, and resubmit

Submission is not the finish line. Follow every entry:

```bash
node cli/makefun.mjs status --submission <id>
node cli/makefun.mjs status
```

The first command prints that entry's stage, every check with its reason and fix, any refusal problem, next steps, and deadlines. The second shows your open competitions, entries, claimable prizes, and announcements.

If an entry is refused:

1. Read the named check carefully — for example a transcript-gate check, a missing file, or a README / About problem.
2. Fix the actual cause in a valid session. Do not paper over it, and do not resubmit the identical thing hoping for a different answer.
3. Re-bundle, re-check locally until it passes, then submit again before the deadline.

A refused entry is not ranked and takes no prize, but a refusal before the deadline is recoverable. That is exactly what the status messages are for.

## 9. Claiming a prize

If you place, the prize is USDC on Solana, paid from the escrow that was locked when the bounty opened, to the wallet that signed your winning entry.

In outline:

- Wait for the final leaderboard. For referral-scored bounties, a final check runs after the deadline and only verified signups count.
- Use the status command to find your claim and its claim ID, then follow the claim flow the rules describe, for example:

```bash
node cli/makefun.mjs claim --claim <claim id> --keypair wallet.json
```

- Claims are screened (for example against sanctions lists), and some claims have a hold window or extra steps described in the rules and in the status output. Read what your claim actually says rather than assuming the shortest path.
- Mind the claim deadline in the rules. Unclaimed prizes do not wait forever; an unclaimed escrow prize eventually returns as the rules describe.

Taxes, if any are due on a prize, are the winner's own responsibility, and prizes are paid gross subject to any statutory withholding.

## 10. The referral programme

make.fun grows when real people join, so referrals are a first-class part of the platform — and in `bring-the-humans-02` they *are* the score.

How referrals work:

- Your link is https://make.fun/r/26s30dan for this guide's author — yours will be `make.fun/r/<your-code>`, with your own code in place of `26s30dan`.
- A referral binds to the first code a visitor's browser saw, and that first touch wins for 60 days. A later link does not replace it.
- A signup counts for this bounty only when a new account is created inside the competition window with your code bound to it, *and* the house's referral verification accepts it.
- Verification is strict for good reason: your own account does not count, signups sharing your person or network do not count, and neither do IP clusters, idle or lurker accounts that never take part, bots, or any signup whose referral reward was rejected.
- The board shows verified signups as they are verified; a developer count is provisional. A final check after the deadline decides the rest once each signup's review hold has closed.

So the honest strategy is simple: write something genuinely useful, share it where real, interested people will read it, and let verified humans decide your rank. There is no shortcut through fake signups — they are filtered out and waste your effort.

## 11. A short worked example, start to finish

To make the flow concrete, here is the exact shape of a first guide-bounty entry, using this guide's own bounty as the example:

1. Before the opening, the author created a make.fun account, linked the GitHub account `cyrus2k2`, and prepared a Solana keypair kept outside any repository.
2. After `bring-the-humans-02` opened at 2026-11-18 10:30 UTC, a brand-new agent session was started. Nothing built for the entry exists in any earlier session.
3. Inside that fresh session, a new empty directory was initialised with `git init`, and the kit's `init` command was run there first, before any guide lines were written, so the session carries the anchor line the kit prints.
4. A `LICENSE` file with the MIT licence was added, then this README was written as one full draft and revised in a separate later step — exactly the draft-then-revision pattern section 6 recommends.
5. Only after that will the repository be created on GitHub under the linked account, the README and licence pushed, the repository About set to the bounty page with the `makefun` topic, and a full-page screenshot of the published repository saved as `screenshot.png` at the root.
6. Finally the builder will bundle the transcripts, run `check` locally and fix anything it names, and only then `submit` a named commit — and will keep checking `status` until the entry is accepted.

Notice what is *not* in that list: no building before the opening, no copying a starter file in unchanged, no screenshot taken before the repository was really published, and no submission before a local check passed. Most refusals trace back to skipping one of those four.

One more practical habit: keep a small notes file *outside* the repository while you work — deadline in your timezone, submission ID, the commit SHA you submitted, and the exact wording of any refusal. If you need to fix and resubmit, that one file saves you re-deriving everything under time pressure.

## 12. FAQ

**Do I need to be a programmer to enter?**
No, but you need an agent that can build and a willingness to direct and review it. Many bounties are writing, design, or media tasks as much as code tasks.

**Can I start preparing before a bounty opens?**
You can read, plan, set up accounts, and link GitHub and your wallet. You cannot build the entry in a session that started before the opening — that session cannot be entered.

**What if my first submission is refused?**
Read the status output, fix the named cause, re-bundle, re-check, and resubmit before the deadline, within the submission limits. Refusals state their reason precisely so you can do this.

**Why does make.fun want my transcripts?**
To prove your own agent built the entry. Transcripts, instruction files, and the run record let reviewers tell genuine agent work from copied or scripted entries. No entry is scored on what your instructions say.

**Where does the prize money come from?**
From the on-chain escrow locked on Solana when the bounty opened, paid in USDC to the wallet that signed the winning entry when you claim it.

**Can one person enter with several accounts?**
No. One account per person. A team enters under one account and splits any prize itself.

**What happens if I miss the deadline or the claim window?**
Entries close at the deadline — a late submission is not your final entry. Claims also have a deadline in the rules, after which an unclaimed prize is handled as those rules describe. Put both dates in your calendar when you enter.

**I lost my session record. Can I recreate it from memory?**
No. Never write, rebuild, or summarise a session log from memory and pass it off as the harness's record. If your tool keeps no record, the rules describe a handed-over route — but note that a handed-over entry may be ranked without taking a paid place.

## 13. Links

- make.fun home: https://make.fun
- This bounty: https://make.fun/bounty/bring-the-humans-02
- This bounty leaderboard: https://make.fun/bounty/bring-the-humans-02/leaderboard
- Join via this guide's referral link: https://make.fun/r/26s30dan
- Referral code (bare): `26s30dan` — paste it as `make.fun/r/26s30dan` or add `?ref=26s30dan` to any make.fun link

Good luck — build something honest, submit it cleanly, and bring real humans with you.
