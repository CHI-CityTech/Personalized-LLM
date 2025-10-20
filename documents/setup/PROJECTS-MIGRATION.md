Why this change

GitHub deprecated the classic Projects REST API (the endpoints under /projects and related resources). Calls such as `GET /repos/{owner}/{repo}/projects` are scheduled for removal and will cause workflows using `github.rest.projects.*` to fail.

What I changed in workflows

- `.github/workflows/project-board-automation.yml`: added a defensive try/catch around `github.rest.projects.listForRepo` to prevent hard failures when the classic Projects API is removed. The workflow will now log an explanatory message and exit that step gracefully.

Why migrate

- Projects (classic) is deprecated and removed. Persistent automation should move to either:
  - The new Projects (V2) GraphQL API (recommended for full feature parity and long-term support), or
  - Alternative repository-level primitives (labels, milestones, issues) if they meet your needs.

Quick migration options

1) Migrate to Projects V2 GraphQL API

- Projects V2 uses GraphQL. It supports boards, fields, items, and automation via GraphQL queries and mutations.
- Example (list projects for a repository - GraphQL):

Query:

```
query($owner: String!, $repo: String!) {
  repository(owner: $owner, name: $repo) {
    projectsV2(first: 10) {
      nodes {
        id
        title
      }
    }
  }
}
```

Mutation examples (creating an item, moving, updating fields) are documented in GitHub's Projects V2 GraphQL API docs.

2) Replace automation with labels/milestones

- For simpler workflows, use labels on issues/PRs and actions that react to label changes. Labels are supported in the Issues REST API and are not deprecated.

Implementation notes

- `actions/github-script` supports GraphQL via `github.graphql()` or `github.request()` for REST. For Projects V2 you'll call `github.graphql(query, variables)` from within `actions/github-script` steps.
- GraphQL request example inside `github-script`:

```
const query = `query ($owner: String!, $repo: String!) { repository(owner: $owner, name: $repo) { projectsV2(first: 5) { nodes { id title } } } }`;
const result = await github.graphql(query, { owner: context.repo.owner, repo: context.repo.repo });
console.log(result);
```

Next steps

- Decide whether to move to Projects V2 (GraphQL) or simplify automation to labels/milestones.
- If Projects V2: replace calls to `github.rest.projects.*` with `github.graphql()` calls. Write a migration PR that implements the GraphQL queries/mutations and tests them on a sandbox repository.
- If labels/milestones: update workflows to tag issues/PRs and remove Projects-related automation.

Contact

If you'd like I can:
- Implement a Projects V2 migration for the automation in `project-board-automation.yml` (requires writing GraphQL queries to find the correct project and columns -> fields/items), or
- Convert the automation to a label-based approach and update the workflows accordingly.
