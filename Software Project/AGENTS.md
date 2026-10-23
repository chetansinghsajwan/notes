It’s an open format for guiding agents on how to perform actions.
For example,
```markdown
# Sample AGENTS.md file
## Dev environment tips
- Use `pnpm dlx turbo run where <project_name>` to jump to a package instead of scanning with `ls`.
- Run `pnpm install --filter <project_name>` to add the package to your workspace so Vite, ESLint, and TypeScript can see it.
- Use `pnpm create vite@latest <project_name> -- --template react-ts` to spin up a new React + Vite package with TypeScript checks ready.
- Check the name field inside each package's package.json to confirm the right name—skip the top-level one.
## Testing instructions
- Find the CI plan in the .github/workflows folder.
- Run `pnpm turbo run test --filter <project_name>` to run every check defined for that package.
- From the package root you can just call `pnpm test`. The commit should pass all tests before you merge.
- To focus on one step, add the Vitest pattern: `pnpm vitest run -t "<test name>"`.
- Fix any test or type errors until the whole suite is green.
- After moving files or changing imports, run `pnpm lint --filter <project_name>` to be sure ESLint and TypeScript rules still pass.
- Add or update tests for the code you change, even if nobody asked.
## PR instructions
- Title format: [<project_name>] <Title>
- Always run `pnpm lint` and `pnpm test` before committing.
```
Add sections that help an agent work effectively with your project. Popular choices:
- Project overview
- Build and test commands
- Code style guidelines
- Testing instructions
- Security considerations
# ==About==
AGENTS.md emerged from collaborative efforts across the AI software development ecosystem, including [OpenAI Codex](https://openai.com/codex/), [Amp](https://ampcode.com/), [Jules from Google](https://jules.google/), [Cursor](https://cursor.com/), and [Factory](https://factory.ai/).
We’re committed to helping maintain and evolve this as an open format that benefits the entire developer community, regardless of which coding agent you use.
AGENTS.md is now stewarded by the [Agentic AI Foundation](https://aaif.io/) under the Linux Foundation.
# ==To Read==
- https://addozhang.medium.com/agents-md-a-new-standard-for-unified-coding-agent-instructions-0635fc5cb759
# ==References==
- [https://agents.md](https://agents.md/)