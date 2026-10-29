# Simplify Skills

Three Agent Skills for reducing cost: tests, CI, and repository architecture.

Install the package, then invoke the skill that matches the work. `$simplify-tests` prunes a suite. `$simplify-ci` keeps the lightest required pipeline. `$simplify-repo` subtracts over-engineering and lands the smaller shape. Each one can also audit first and return a grouped cleanup plan without editing.

## Quick start

```text
npx skills add gvastethecreator/simplify-skills
```

Copy `SKILLS/simplify-tests`, `SKILLS/simplify-ci`, or `SKILLS/simplify-repo` into the skill directory your agent already loads.

- [simplify-tests](SKILLS/simplify-tests/SKILL.md)
- [simplify-ci](SKILLS/simplify-ci/SKILL.md)
- [simplify-repo](SKILLS/simplify-repo/SKILL.md)

Each skill folder is self-contained and can be installed on its own. No additional skills are required. The procedures include failure diagnosis, scoped GitHub settings checks, interface review and report guidance where needed. Use the target project's existing runtime and tools; account settings and required gates still need the task's authority.

License: [MIT](LICENSE).
