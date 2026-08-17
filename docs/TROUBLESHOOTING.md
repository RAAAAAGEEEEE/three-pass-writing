# Troubleshooting

## The skill does not appear in the `/` menu

Restart Claude Code — new top-level skill directories are picked up at startup, not mid-session.

Then check it is installed and where from:

```bash
claude plugin list
```

For the clone install, check the path is exactly right — the folder must sit directly under the skills directory and contain `SKILL.md` at its root:

```bash
ls ~/.claude/skills/three-pass-writing/SKILL.md
```

```powershell
Get-ChildItem "$env:USERPROFILE\.claude\skills\three-pass-writing\SKILL.md"
```

## "Not loaded — the name is already taken by an installed plugin"

You installed both ways. `claude plugin list` says it plainly:

```
three-pass-writing@skills-dir: × Not loaded — the name "three-pass-writing" is already
taken by an installed plugin (three-pass-writing@three-pass-writing), which takes precedence.
```

The installed plugin wins and the clone is ignored. Keep one: either delete `~/.claude/skills/three-pass-writing/`, or uninstall the plugin with `claude plugin uninstall three-pass-writing`.

## "Subagent type not found", or every pass runs on the same model

The subagents in `agents/` are loaded by the plugin manifest. If they are missing, `.claude-plugin/plugin.json` was not loaded.

Check the component inventory:

```bash
claude plugin details three-pass-writing
```

It should list one skill and three agents. If it lists none, validate the folder:

```bash
claude plugin validate ~/.claude/skills/three-pass-writing
```

For a skills-directory install, changes to `agents/` need a plugin reload — run `/reload-plugins` in the session, or restart.

The skill falls back to a general-purpose subagent with an explicit model override and tells you it did. Copy is still produced; the model pinning is what you lose.

## Pass 2 finds nothing, every time

Usually the context file is too thin: with no proof table and no banned list, the reviewer has nothing to check claims against and drops to style-only findings.

Open `.claude/writing-context.md` and fill **Quotable proof**, **Shipped / Not shipped**, and **Banned**. Those three sections generate most P0 and P1 findings.

## Pass 2 flags everything, every time

Either the drafts genuinely over-claim — check whether the flagged claims are actually sourced anywhere — or the rubric has been tightened past what your copy needs. Findings are ranked, so start by fixing P0s only and see whether the piece is shippable.

## The copy invented a feature we do not have

The **Not shipped** section is empty or incomplete. That list is what stops it. Re-run the audit:

```
/three-pass-writing refresh the writing context
```

and check the `NOT SHIPPED` block carefully. Anything behind a flag, stubbed, or in a branch belongs there.

## `[[to confirm: ...]]` markers in the final text

Working as intended: the pipeline needed a fact it did not have and refused to invent one. Supply the fact, or cut the sentence. If the same placeholder keeps coming back, add the answer to **Quotable proof** in the context file.

## It writes in the wrong language

The skill follows the language of your message. State it once explicitly ("write in French"), and record the default in the **Voice** section of the context file.

## The tone is nothing like mine

The **Voice** section probably holds adjectives instead of rules. Adjectives do not transfer. Replace them with observable rules, and paste two samples of your own writing into the brainstorm — that single input does more than any amount of tone description.

## `claude plugin marketplace add` fails

Check the repository is reachable and spelled correctly, that `git` is on your PATH, and that you are online. To install from a local clone instead:

```bash
claude plugin marketplace add /absolute/path/to/three-pass-writing
```

## It costs more than expected

Three subagent calls per piece, one on Opus. For a batch of variants, ask for them inside a single draft pass rather than running the pipeline per variant — see the cost note in [`references/three-pass-pipeline.md`](../references/three-pass-pipeline.md).

## Something else

Open an issue with your Claude Code version (`claude --version`), the install path you used, and the output of `claude plugin details three-pass-writing`. Never paste your `.claude/writing-context.md` into a public issue without reading it first.
