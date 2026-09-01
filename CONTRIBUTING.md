# Contributing to Holdout

Thanks for helping make financial research evidence easier to inspect.

## Ground rules

1. Every change must make something harder to fake.
2. Zero-dependency is a feature unless a new dependency is clearly justified.
3. Fail closed by default.
4. Be explicit about boundaries whenever a check is heuristic or incomplete.
5. Research tooling is not execution tooling. It must not place orders,
   change trading rules, or present research output as investment advice.

## Development flow

1. Fork the repo and create a branch from `main`.
2. Implement with tests.
3. Run the full test suite locally before pushing.
4. Open a PR.

## Commit style

Use conventional commits (`feat:`, `fix:`, `docs:`, `chore:`, `test:`,
`refactor:`).

## License

All repositories in this organization are MIT. By contributing you agree
that your contributions are licensed under the repository's MIT license.
