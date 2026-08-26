# Contributing to Foolproof Labs

Thanks for helping make self-deception structurally impossible.

## Ground rules

1. **Every change must make something harder to fake.** A feature that
   cannot be tied to a form of self-deception (overfitting, look-ahead,
   PIT drift, forgotten tuition, post-hoc rationalization, dirty data)
   probably does not belong in this organization.
2. **Zero-dependency is a feature.** New hard dependencies need a strong
   justification in the PR description.
3. **Fail-closed by default.** When in doubt, a check that refuses to run
   is better than a check that silently passes.
4. **Honesty about boundaries.** If your change sits at a heuristic
   boundary (e.g. value-dependent semantics, vendor-specific conventions),
   say so in the docstring and the README. Never claim verifiability
   where the theory does not allow it.

## Development flow

1. Fork the repo and create a branch from `main`.
2. Implement with tests. The project family standard is `pytest`, Python
   3.11+, zero (or near-zero) dependencies.
3. Run the full test suite locally before pushing.
4. Open a PR. CI runs the suite on Ubuntu / Windows / macOS.

## Commit style

Conventional commits (`feat:`, `fix:`, `docs:`, `chore:`, `test:`,
`refactor:`) — keep history clean and machine-readable.

## Issue handling

Issues are handled on weekends. Bug reports that describe a
*plausible-but-wrong* result (silent self-deception) get priority.

## Good first issues

Every repository labels beginner-friendly issues `good-first-issue`. These
are deliberately small and self-contained: a missing test, a docstring
boundary statement, an example fixture. If you are new to the organization,
pick one, mention it in your PR, and maintainers will review quickly.

## License

All repositories in this organization are MIT. By contributing you agree
that your contributions are licensed under the repository's MIT license.
