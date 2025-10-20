# Migrate Project automation from Projects (classic) REST to Projects V2 GraphQL

Summary

We were using the classic Projects REST API from workflows (e.g. `project-board-automation.yml`). That REST API has been deprecated and removed. I updated the workflow to use Projects V2 GraphQL calls, but we should review and test the migration in a sandbox environment and optionally create a PR that:

- Verifies the target Project V2 exists and has a single-select status field with options: backlog, in progress, in review, done (or update the workflow to accept mapping config).
- Adds robust pagination when projects/items exceed current first: limits.
- Adds unit/integration tests or a dry-run mode for the workflow.
- Confirms permissions and token scopes: the GITHUB_TOKEN in Actions should have access to the project's operations. If not, a PAT with proper scopes may be required.

Acceptance criteria

- The `project-board-automation.yml` step no longer errors when running on repositories with Projects V2.
- Issue and PR events create or update Project V2 items with the expected status field values.
- Edge cases (missing project, missing field, missing option) are logged and do not crash the workflow.

Suggested owner: @CHI-CityTech/team (please assign a team member)

Steps to validate

1. Create a sandbox repository and a Project V2 titled "Research Progress" with a single-select field named "Status" and options: backlog, in progress, in review, done.
2. Trigger issue opened/closed and PR events and observe Project V2 items being created/updated.
3. If failures occur, consult Actions logs; the workflow logs helpful messages for missing fields/options.

Notes

- I added `documents/setup/PROJECTS-MIGRATION.md` with background and a small GraphQL example.
- The workflow currently fetches `projectsV2(first:20)` and `items(first:100)` — these are reasonable defaults but should be paginated for large projects.
- The workflow uses `github.graphql()` inside `actions/github-script@v7` so no new action dependencies were added.
