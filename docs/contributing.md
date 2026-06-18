# Contributing

Contributions to `brotni-examples` are welcome.

## What makes a good example

Good examples are:

- **Small** — readable in under 5 minutes
- **Realistic** — based on a real pattern teams encounter
- **Safe** — no secrets, no real customer data, no proprietary code
- **Independent** — work with `--dry-run` mode without real infrastructure
- **Educational** — teach a specific concept clearly

## How to contribute

1. Fork the repository.
2. Create a branch: `git checkout -b add-my-scenario`.
3. Add your example in the appropriate directory.
4. Include a `README.md` in your directory explaining what the example does and how to run it.
5. Use only synthetic data — never include real customer records or secrets.
6. Open a pull request.

## Directory conventions

| Type | Directory | Naming |
|------|-----------|--------|
| Scenario | `scenarios/<name>/` | kebab-case |
| Recipe | `recipes/<type>/<variant>/` | kebab-case |
| Context | `contexts/<type>/<variant>/` | kebab-case |
| Dataset | `datasets/<category>/` | kebab-case |
| CI snippet | `github-actions/<name>/` or `gitlab-ci/<name>/` | kebab-case |

## YAML conventions

- Use `apiVersion: brotni.dev/v1` in all spec files.
- Include a `metadata.description` field.
- Add a comment at the top of each file explaining its purpose and how to use it.
- Never include real secrets, tokens, or credentials.
- Use `# placeholder` or environment variable references for any sensitive values.

## License

By contributing, you agree that your contributions will be licensed under
the Apache License 2.0.
