Check that a GitHub repository is connected to this project. If not, stop
immediately and write reports/OSS_CONTRIBUTOR_AUDIT.md with only this message:

> This workflow requires a connected GitHub repository.
> Connect one from the project integrations page and run again.

Do not continue if there is no connected repository.

When a repository is connected, read its structure first: README.md,
CONTRIBUTING.md (note if absent), open issues, recent pull requests (last 20),
issue labels, issue templates, FUNDING.yml and repo description metadata.

Apply the contributor-onboarding skill to audit the repository across five
dimensions and produce a prioritized fix list.

If the focus input is supplied, weight that dimension's findings higher in
the priority ranking. If skip_dimensions is supplied, note the skipped
dimensions in the report header and omit them from scoring.

Write reports/OSS_CONTRIBUTOR_AUDIT.md only.

Do not open pull requests against the connected repository. Do not modify any
file in the connected repository. Do not post, send, publish, request secrets
or start another workflow.
