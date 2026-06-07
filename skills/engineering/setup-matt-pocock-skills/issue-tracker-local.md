# Issue tracker: Local Markdown

Issues and PRDs for this repo live as markdown files in `~/.agents/<repo_name>/scratch/` — create the directory if it doesn't exist (`mkdir -p ~/.agents/<repo_name>/scratch`). The `<repo_name>` should match your current repo (i.e. if within `nova`, the path is `~/.agents/nova/scratch/`).

## Conventions

- One feature per directory: `~/.agents/<repo_name>/scratch/<feature-slug>/`
- The PRD is `~/.agents/<repo_name>/scratch/<feature-slug>/PRD.md`
- Implementation issues are `~/.agents/<repo_name>/scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`
- Triage state is recorded as a `Status:` line near the top of each issue file (see `triage-labels.md` for the role strings)
- Comments and conversation history append to the bottom of the file under a `## Comments` heading

## When a skill says "publish to the issue tracker"

Create a new file under `~/.agents/<repo_name>/scratch/<feature-slug>/` (creating the directory if needed).

## When a skill says "fetch the relevant ticket"

Read the file at the referenced path. The user will normally pass the path or the issue number directly.
