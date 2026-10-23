**GitHub Actions** is a CI/CD and workflow automation platform built directly into GitHub. It lets you automate tasks like building, testing, and deploying code — triggered by events like a push or pull request.
### ==Components==
#### ==Workflow==
- A list of jobs to run.
- Defines _what_ to run and _when_
- Stored as YAML files in `.github/workflows/`
- One repo can have many workflows (e.g. one for CI, one for deploy, one for auto-labelling)
#### ==**Event**==
- The trigger that starts a workflow
- Examples: A commit is pushed, a PR is created.
- Types: `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual), `release`, `issue_comment`, and 40+ more
- You can filter by branch, tag, or file pat
#### ==**Job**==
- A set of steps that run on one runner
- Jobs run in parallel by default; use `needs:` to chain them sequentially
- Each job gets a fresh, isolated environment
#### ==**Step**==
- The smallest unit inside a job
- Either `run:` (shell command) or `uses:` (an Action)
- Steps within a job share the same runner and filesystem
#### ==**Action**==
- A reusable set of jobs to reduce amount of repetitive code.
- Come from the [GitHub Marketplace](https://github.com/marketplace?type=actions) (100,000+ available) or your own repo
- Written in JavaScript or as Docker containers
- Examples: `actions/checkout`, `actions/setup-node`, `aws-actions/configure-aws-credentials`
#### ==**Runner**==
- The virtual machine that executes jobs
|Type|OS options|Cost|
|---|---|---|
|GitHub-hosted|Ubuntu, Windows, macOS|Paid minutes (free for public repos)|
|Self-hosted|Any OS, your hardware|Free execution, $0.002/min platform fee (from Mar 2026)|
|Larger runners|4–64 core VMs|Higher per-minute rate|
### ==**Pricing (As of 2026)**==
Public repositories get free CI/CD. For private repos:
|Runner OS|Cost per minute (USD)|Cost per minute (INR ~₹83)|
|---|---|---|
|Linux (small)|~$0.002|~₹0.17|
|Linux (2-core)|~$0.006|~₹0.50|
|Windows|~$0.010|~₹0.83|
|macOS|~$0.062|~₹5.15|
Prices were cut by ~39% effective January 1, 2026, but a new $0.002/min platform charge was added for self-hosted runners starting March 1, 2026.
**Free plan includes:** 2,000 Actions minutes/month and 500MB storage for private repos.
# ==References==

> [!info] Understanding GitHub Actions - GitHub Docs  
> Learn the basics of core concepts and essential terminology in GitHub Actions.  
> https://docs.github.com/en/actions/get-started/understand-github-actions