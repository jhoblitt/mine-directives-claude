# mine-directives-claude

A [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces)
for config hygiene. One plugin, `mine-directives`, carrying an audit
recipe that mines accumulated per-project memory and past session
transcripts for durable, standing directives — and proposes lifting
each into its right home.

Per-project memory stays with one project; the global CLAUDE.md
applies everywhere. Standing rules ("always X", "never Y",
"prefer Z") accumulate in per-clone memory and raw transcripts and
quietly stop traveling with you. The skill classifies one-off tasks vs
standing rules, routes each to its right config layer,
cross-references what's already captured, and proposes every change as
a unified diff for approval.

## Install

Inside Claude Code:

```text
/plugin marketplace add jhoblitt/mine-directives-claude
/plugin install mine-directives@mine-directives-claude
```

or from a shell:

```sh
claude plugin marketplace add jhoblitt/mine-directives-claude
claude plugin install mine-directives@mine-directives-claude
```

To pick up updates later:

```sh
claude plugin marketplace update mine-directives-claude   # refresh the index
claude plugin update mine-directives@mine-directives-claude  # install the new version
```

or inside Claude Code:

```text
/plugin marketplace update mine-directives-claude
/plugin update mine-directives@mine-directives-claude
/reload-plugins
```

After the shell form, run `/reload-plugins` in running sessions — a
restart also works. The marketplace step alone only refreshes the
index; it does not update the installed plugin.

## Example prompts

- "Mine my memory and sessions for directives I haven't put in CLAUDE.md."
- "What standing rules haven't I captured?"
- "Audit the other clones' memory for global rules to uplift."
- "Do a CLAUDE.md hygiene pass."

## Scope

The heaviest payoff is the first run — excavating the backlog of rules
that never made it into the global file. After that, occasional sweeps
mostly confirm that capture habits are working; the large majority of
mined material is one-off noise, by design. The skill judges where
rules should live; it does not audit the prose quality of existing repo
CLAUDE.md files (compose it with a CLAUDE.md improver for that), and it
never auto-fixes: every proposed write is a diff requiring approval.

## Privacy

The skill reads local memory files and local session transcripts, and
adds no channel beyond your normal session traffic. Transcripts often
contain pasted secrets, so the procedure paraphrases rather than
quotes, scans every candidate diff for secrets before presenting it,
and treats checked-in files as publication targets. Every proposed
change is shown as a diff before any file is touched.

## Development

Validate after changes:

```sh
claude plugin validate .
```

Content changes land via PR. Commit messages follow
[Conventional Commits](https://www.conventionalcommits.org/) — commitlint
enforces this on every PR — and releasing is automated: on each merge to
`main`, [semantic-release](https://github.com/semantic-release/semantic-release)
derives the next version from the commit types in the changeset (`fix:`,
`docs:`, `refactor:`, `perf:` → patch; `feat:` → minor; a breaking change →
major; other types cut no release), writes it into the plugin manifest and
`CHANGELOG.md`, tags, and publishes a GitHub release. Never bump the
plugin version in a PR — the release commit owns that field.

## License

[Apache-2.0](LICENSE)
