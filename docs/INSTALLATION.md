# Installation

Two ways to install. They produce the same behaviour — pick by how you manage the rest of your setup.

| | Plugin | Personal skill |
| --- | --- | --- |
| Command | `/three-pass-writing` | `/three-pass-writing` |
| Updates | `claude plugin update three-pass-writing@three-pass-writing` | `git pull` |
| Location | managed by Claude Code | `~/.claude/skills/three-pass-writing/` |
| Editable in place | not meant to be | yes |
| Team sharing | `--scope project` | manual |

Do not install both at once. Two copies means two entries in the skill list.

## Requirements

- Claude Code v2.1.220 or later:

```bash
claude --version
```

- Sonnet **and** Opus available on your plan. The pipeline pins pass 2 to Opus; without it, every pass falls back to your session model and the review loses most of its value.

## Option A — plugin

Add this repository as a marketplace, then install the plugin it contains:

```bash
claude plugin marketplace add RAAAAAGEEEEE/three-pass-writing
```

```bash
claude plugin install three-pass-writing@three-pass-writing
```

To install for a whole team, from inside the project repository:

```bash
claude plugin install three-pass-writing@three-pass-writing --scope project
```

Restart Claude Code so the subagents load.

## Option B — personal skill

Clone the repository directly into your personal skills folder:

```bash
git clone https://github.com/RAAAAAGEEEEE/three-pass-writing ~/.claude/skills/three-pass-writing
```

On Windows PowerShell:

```powershell
git clone https://github.com/RAAAAAGEEEEE/three-pass-writing "$env:USERPROFILE\.claude\skills\three-pass-writing"
```

The folder ships a `.claude-plugin/plugin.json`, so Claude Code loads it as a skills-directory plugin named `three-pass-writing@skills-dir` and picks up the three subagents in `agents/`. Without that file, the skill would still run but the model pinning would not.

Restart Claude Code after cloning.

## Verify

```bash
claude plugin list
```

Then, in a session:

```
/three-pass-writing
```

The skill should appear in the `/` menu, and the component inventory should list one skill and three agents:

```bash
claude plugin details three-pass-writing
```

## Update

Plugin — the qualified `plugin@marketplace` name is required here, the short name returns "Plugin not found":

```bash
claude plugin update three-pass-writing@three-pass-writing
```

Personal skill:

```bash
git -C ~/.claude/skills/three-pass-writing pull
```

Your `.claude/writing-context.md` files live in your own projects and are never touched by an update.

## Uninstall

```bash
claude plugin uninstall three-pass-writing
```

```bash
claude plugin marketplace remove three-pass-writing
```

Or, for the personal-skill install, delete `~/.claude/skills/three-pass-writing/`. The context files stay in your projects; delete them by hand if you want them gone.
