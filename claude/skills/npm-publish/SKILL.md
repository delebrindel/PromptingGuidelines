---
name: npm-publish
description: Publish an npm package safely, covering dist-tags, one-time passwords, and pre-release versions. Use when publishing or tagging a package, changing a version, or checking what the latest tag points to.
---

# npm publish

Publishing changes shared state. Get explicit approval for the exact version and dist-tag before running `npm publish`. An earlier approval does not carry over to a new version.

## Before publishing

1. Confirm the target: package name, version, dist-tag, and registry. Example: `@delebrindel/miso` `0.1.0-beta.0` with `--tag beta`.
2. Check the working tree is clean (`git status`) and the release commit is pushed. Pushing needs its own approval.
3. Check the package version in `package.json` matches the version you are publishing.
4. Review the file list: `npm pack --dry-run`. Only intended files should appear. For miso: `dist`, `src/theme.css`, `README.md`. Flag `.env`, build leftovers, or Storybook output.
5. Run `npm publish --dry-run --tag <tag>`. This runs `prepublishOnly` (typecheck, tests, build). Read the output. Do not assume it passed.
6. Check current tags: `npm view <pkg> dist-tags`.

## Publishing

- Run `npm publish --tag <tag>`. Access is set in `publishConfig` for scoped packages, so `--access public` is not needed per publish.
- If the account requires a one-time password, ask the user for a fresh code for this publish. Never reuse one. Never write it into files, logs, or commits.

## After publishing

- Check tags again: `npm view <pkg> dist-tags`. The new version should sit under the tag you intended.
- Known trap: npm points `latest` at the first version published when no `latest` tag exists, even with `--tag beta`. Miso's `latest` equals `0.1.0-beta.0` for this reason. Fix it only with approval: `npm dist-tag add <pkg>@<version> latest`.
- Verify the release installs: `npm view <pkg>@<version> version`. Then, in a scratch folder outside the repo, run `npm install <pkg>@<version>` and import it.

## Rules

- Versions are immutable. A bad release gets a new version, not a re-publish. Deprecate only with approval.
- Never publish a stable version or move `latest` unless the user names that version.
- Never weaken `prepublishOnly` to get a publish through.
- Never run `npm audit fix --force`.

## Checklist

- [ ] Approval for this version and tag
- [ ] Clean tree, release commit pushed
- [ ] `npm pack --dry-run` file list reviewed
- [ ] Dry run passed
- [ ] One-time password supplied fresh for this publish, if needed
- [ ] dist-tags checked after publish
