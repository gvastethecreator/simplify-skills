# Simplify Skills

Three Agent Skills for reducing cost: tests, CI, and repository architecture.

Install the package, then invoke the skill that matches the work. `$simplify-tests` prunes a suite. `$simplify-ci` keeps the lightest required pipeline. `$simplify-repo` subtracts over-engineering and lands the smaller shape.

## Quick start

```text
npx skills add gvastethecreator/simplify-skills
```

Copy `SKILLS/simplify-tests`, `SKILLS/simplify-ci`, or `SKILLS/simplify-repo` into the skill directory your agent already loads.

- [simplify-tests](SKILLS/simplify-tests/SKILL.md)
- [simplify-ci](SKILLS/simplify-ci/SKILL.md)
- [simplify-repo](SKILLS/simplify-repo/SKILL.md)

Failing checks stay `fix-ci`. Token permissions and Actions enable/disable stay `github-hygiene` when that skill is available.

License: [MIT](LICENSE).
