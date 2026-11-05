# GitHub Standard

How the College of Education's development team works in the `UF-College-of-Education` GitHub organisation. It applies to staff developers, team leads and student developers.

In the wiki each part below is its own tab: **Start here**, **Rules**, then sections 1 to 4.

---

## Start here

Read this page first. It covers what changed, how to make a change from start to finish, and what to do when GitHub stops you.

### What's changed (November 2026)

If you worked in the organisation before November 2026, six things are different:

1. **`main` and `dev` are protected.** Nobody can push to them directly, including team leads and organisation owners.
2. **`dev` is the default branch.** New work branches from `dev` and returns to `dev`. `main` only receives finished releases.
3. **Every change goes through a pull request.**
4. **A staff developer approves every pull request.** Pushing more commits after an approval cancels it, and a fresh approval is needed.
5. **Pull requests merge with a merge commit.** Squash and rebase merges are switched off.
6. **`main` and `dev` cannot be force-pushed or deleted, and release tags cannot be moved.** Your own feature branches are unaffected.

### Your first change

1. **Get the repository and start from `dev`:**

   ```
   git clone https://github.com/UF-College-of-Education/<repository>.git
   cd <repository>
   git switch dev
   git pull
   ```

2. **Make a branch** named `<your-github-username>/<what-it-is-for>`:

   ```
   git switch -c abanders-uf/nelms-fix-home-toolbar
   ```

3. **Make your change, then commit it** with a Conventional Commits message (§1.3):

   ```
   git add <files>
   git commit -m "fix: keep the home toolbar visible on small screens"
   ```

4. **Push your branch:**

   ```
   git push -u origin abanders-uf/nelms-fix-home-toolbar
   ```

5. **Open a pull request into `dev`.** GitHub prints a link after the push, or use `gh pr create --base dev`. Fill in the template (Templates, in section 4). GitHub asks the staff developers for a review automatically.
6. **Respond to the review.** If you push more commits, the reviewer approves again.
7. **Merge** with **Create a merge commit** once the pull request is approved.

Your change is now in `dev`. It reaches `main` when a staff developer or lead releases `dev` (§2.4).

### When GitHub stops you

| You see | It means | Do this |
|---|---|---|
| `GH013: Repository rule violations found … Changes must be made through a pull request` | You pushed straight to `main` or `dev` | Push to your own branch and open a pull request |
| The pull request shows **Review required** or **Merging is blocked** | No staff developer has approved it yet, or a newer push cancelled the approval | Wait for, or ask for, a staff approval |
| A force push to `main` or `dev` is rejected | Their history is protected | Make a new commit instead |
| Pushing a moved or deleted `v…` tag is rejected | Release tags are locked once pushed | Tag a new version |
| Only **Create a merge commit** is offered | Squash and rebase merges are off | Use the merge commit |

If you are stuck, ask a staff developer on your project or your team lead.

### About this standard

It covers work in the `UF-College-of-Education` organisation. WordPress sites run on their own established workflow, which the web team lead manages, and are outside it for now. It works alongside three other standards, which it does not repeat:

- **Coding Standards,** in the web team wiki, for language and style conventions.
- **Claude Development Standards,** for using Claude.
- **The UFIT Research Computing web application standard,** for projects that run on HiPerGator.

---

## Rules

Every rule, grouped by who it applies to. The section number points to the detail. Cite it in a review comment, for example "see §1.4".

### Everyone

- §1.1 Have a GitHub account with 2FA on, and `git` configured with that account's email, before your first change.
- §1.2 Branch from `dev`, name the branch `<github-username>/<purpose>`, and return it to `dev` by pull request.
- §1.3 Write commit messages in Conventional Commits form, and keep commits small and focused.
- §1.4 Every change reaches `dev` or `main` through a pull request that uses the template.
- §1.4 A pull request should stay under about 400 changed lines.
- §1.4 Before asking for review, lint your change, read your own diff, run it locally and update the docs.
- §1.5 Secrets live only in GitHub Actions or environment secrets. Commit `.env.example`, never `.env`, and never send a secret in a Slack message.
- §1.6 Restricted data never enters a repository, test fixtures and seed files included. Test data is synthetic.
- §1.7 A leaked secret is rotated or replaced at once and checked for misuse; a compromise or exposed confidential data is reported to the College ISM straight away.
- §1.8 Every repository has a README saying what it is, how to run and deploy it, who owns it and who built it, and keeps further documentation in `docs/`.
- §1.9 Using Claude in a repository follows the Claude Development Standards.
- §1.10 Every repository commits its formatter and linter configuration.

### Staff developers and team leads

- §2.1 A staff developer approves every pull request into `dev` or `main`. Team leads review each other's work where they can.
- §2.2 Reviewers respond within two business days, three at most.
- §2.3 `main` and `dev` take changes only through an approved pull request, with no bypass for anyone.
- §2.4 Release by merging `dev` into `main` once `dev` is stable, then tag the release `v<major>.<minor>.<patch>`.
- §2.5 Deploy every target from a release tag on `main`, apart from the shared dev instance, which follows `dev`.
- §2.6 HiPerGator runs code checked out from a release tag.
- §2.8 Required checks are added in stages: lint and formatting, then build, then tests.

### Organisation administration

- §3.1 Access follows role: owners Admin, team leads Maintain, staff developers Write across the organisation, students Write on their project's repositories only, everyone Read.
- §3.2 Access is granted to teams, never to individuals one by one.
- §3.3 Every member and outside collaborator must have 2FA on.
- §3.4 Student access is reviewed at the start of each semester.
- §3.5 Outside collaborators are reviewed at the start of each semester.
- §3.6 Staff developers and team leads create repositories. They are private unless published for research or released as open source, named project-first, and tagged with topics.
- §3.7 Builds and tests run on GitHub-hosted runners, deployments only on College-managed runners, and Actions usage stays within the organisation's allowance.
- §3.8 Every repository has secret scanning and push protection wherever GitHub provides them, and Dependabot alerts with someone who reads them.

### Templates and research outputs

- §4.1 Start a new repository from the College's shared configuration and templates.
- §4.2 Where it fits, register a research repository with a Zenodo DOI and ORCID.

---

## 1. Working day to day

For everyone who writes code in the organisation.

### 1.1 Getting set up

Before your first change:

1. Create a GitHub account if you do not have one, and turn on two-factor authentication (Settings → Password and authentication).
2. Tell `git` who you are, using the email address on your GitHub account:

   ```
   git config --global user.name "Your Name"
   git config --global user.email you@example.com
   ```

3. Ask a team lead to add you to the organisation and to your project's team.

### 1.2 Branches

Every repository has two long-lived branches:

| Branch | Holds | Changes by |
|---|---|---|
| `main` | The current final build. Nothing reaches it until it is final. | Pull request from `dev` |
| `dev` | Ongoing development. It is the default branch. | Pull request from a feature branch |

There is no staging branch and no branch per deployment target.

For your own work:

1. Branch from `dev`.
2. Name the branch `<github-username>/<purpose>`, for example `abanders-uf/nelms-fix-home-toolbar`.
3. Push it and open a pull request back into `dev` (§1.4).

You may force-push your own feature branch, for example after a rebase. `main` and `dev` cannot be force-pushed (§2.3).

### 1.3 Commits

Write commit messages in Conventional Commits form:

```
<type>(<optional scope>): <description>

<optional body>

<optional footer>
```

| Type | For |
|---|---|
| `feat` | Adding, changing or removing a feature |
| `fix` | Fixing a bug |
| `content` | Text or images shown to users |
| `refactor` | Restructuring code without changing behaviour |
| `perf` | A refactor that improves performance |
| `style` | Formatting only |
| `test` | Adding or correcting tests |
| `docs` | Developer documentation only |
| `build` | Build tools, dependencies, project version |
| `ops` | Infrastructure, deployment scripts, CI/CD, backups, monitoring |
| `chore` | Everything else, such as `.gitignore` changes; a repository's first commit is `chore: init` |

- **Description:** required. Use the imperative ("add", not "added"), start in lower case, and leave off the full stop: `feat: add email validation to registration form`.
- **Scope:** optional. Projects choose their own scopes, and an issue number is not a scope.
- **Breaking change:** mark it with `!` before the colon (`feat(api)!: remove status endpoint`) and explain it in a footer that begins `BREAKING CHANGE:`.
- **Footer:** where the change closes an issue, reference it there (`Closes #123`).

Commits should be small and do one thing, so a reviewer can follow them and one can be reverted without taking others with it.

### 1.4 Pull requests

Every change reaches `dev` or `main` through a pull request. Fill in the pull request template (section 4): what changed, how it was tested, and a short checklist. It takes about a minute.

A pull request should stay under about 400 changed lines. Review quality drops sharply above that. Split a larger change, or say in the description why it cannot be split.

Before asking for review:

- Lint the change.
- Read your own diff first.
- Run the change locally.
- Update the README or docs if behaviour changed.
- An AI review with an approved tool is encouraged. It does not replace the staff review.

Opening a pull request into `dev` or `main` asks the staff developers for a review automatically. Requests will also go to the team's Slack review channel once it is set up.

### 1.5 Secrets

Secrets live only in GitHub Actions secrets or environment secrets. A repository commits `.env.example`, listing each variable with no value, and each developer copies it to `.env` on their own machine. `.env` is never committed, and a secret is never sent in a Slack message.

### 1.6 Data

Restricted research data and student records never enter a repository. That includes test fixtures, seed files and sample exports. Test data is synthetic. Anything committed stays in the repository's history, even after the file is deleted.

### 1.7 When a secret leaks or data is exposed

If a secret is committed or exposed:

1. Rotate, renew or replace it at once.
2. Check the service's logs for any use you do not recognise.
3. If there is none, carry on. If there is, report it.

Report a compromised system, or real confidential data that has been committed or exposed, to the College ISM straight away. The ISM passes it to the right people.

Removing a secret from a repository's history is a separate step, done only with the organisation owner's express permission.

### 1.8 Documentation in a repository

Every repository has a README (Templates, section 4) saying:

- what it is
- how to run it locally
- how to deploy it
- who owns it
- who built it, partners outside the College included

Screenshots and Mermaid diagrams of the architecture or flow are encouraged. A repository with subprojects, such as `frontend` and `backend`, gives each its own README. Further non-sensitive documentation lives in a `docs/` folder.

### 1.9 Claude

Using Claude in a repository follows the Claude Development Standards. Among other things, Claude does not commit `.env` files, keys or credentials.

### 1.10 Formatter and linter configuration

Every repository commits its formatter and linter configuration, starting from the College's shared files (section 4). Style then gets settled by tools, not in review.

---

## 2. Reviewing and releasing

For staff developers and team leads.

### 2.1 Who approves

Every pull request into `dev` or `main` needs an approval from a member of `staff-devs`, the team that holds every staff developer. An approval from anyone else, students included, does not count. Team leads review each other's work where they can.

### 2.2 Turnaround

Aim to respond to a review request within two business days, and within three at most. Slow review pushes people towards large pull requests.

### 2.3 Protection

In every repository this standard covers, `main` and `dev`:

- take changes only through a pull request with a staff approval
- lose that approval when a new commit is pushed to the pull request
- accept merge commits only
- cannot be pushed to directly, force-pushed or deleted

The same applies to team leads and organisation owners; there is no bypass. In an emergency an organisation owner switches the protection off, makes the change, switches it back on, and records what was done.

### 2.4 Releases

When `dev` is known to be stable, open a pull request from `dev` into `main`. Once it is approved and merged, tag the merge commit with its version and push the tag.

Versions follow `v<major>.<minor>.<patch>`:

| The release contains | Raise |
|---|---|
| A breaking change | major |
| A `feat` or `fix` commit | minor |
| Anything else | patch |

A release tag cannot be moved or deleted once pushed. If a release is wrong, tag a new one.

### 2.5 Deployment

The shared dev instance follows `dev`, so the people a project serves can see current progress without asking. Every other target, from local hardware to HiPerGator and production, deploys from a release tag on `main`. The same build goes everywhere, and environment variables or per-target configuration carry the differences. One tag then names exactly what runs where.

### 2.6 HiPerGator

HiPerGator runs code checked out from a release tag, so a result can be traced to the exact code that produced it. Large files and models may need another route than `git`. Projects on HiPerGator also follow the UFIT Research Computing web application standard.

### 2.8 Required checks

Checks become required one stage at a time: lint and formatting first, then build, then tests as a project gains them. A check is made required only once it passes reliably, because a check that always fails gets ignored.

---

## 3. Access and administration

For team leads and organisation owners.

### 3.1 Roles

| Role | Access |
|---|---|
| Organisation owners | Admin |
| Team leads | Maintain on every repository |
| Staff developers | Write on every repository |
| Student developers | Write on their project's repositories only |
| Every member | Read on every repository |

Write lets someone push their own branches and open pull requests. It does not let them change `main` or `dev` directly (§2.3).

### 3.2 Teams

Access is granted to teams, never to individuals one by one.

- **`staff-devs`** holds every staff developer. Its members' approvals are the ones that count (§2.1).
- **`leads`** holds the two team leads.
- **Project teams,** such as `nelms`, give their members Write on that project's repositories. Students belong to their project's team and never to `staff-devs`.

### 3.3 Two-factor authentication

Every member and outside collaborator must have 2FA on.

### 3.4 Student access

Students are added to their project's team when they join a project. Their access is reviewed at the start of each semester.

### 3.5 Outside collaborators

People outside the organisation who need a repository are added as outside collaborators to that repository only. Their access is reviewed at the start of each semester.

### 3.6 Repositories

- **Creating.** Staff developers and team leads create repositories. A student who needs one asks a staff developer or lead.
- **Visibility.** Repositories are private. One is made public only when it is published for research or released as open source, decided for that repository on its own.
- **Naming.** Name a new repository with the project identifier first, in the casing that project already uses, then a lowercase hyphenated part: `PNLI-frontend`, `QPSI-learning-paths`, `SPARCP-notebooks`. The `CoE-` prefix is kept for organisation-wide configuration and shared assets. Existing repositories are not renamed, because a rename breaks every clone, remote, deployment script and link; where one project's repositories use different conventions, align the names when the chance arises.
- **Topics.** Tag each repository with GitHub topics for its project (for example `pnli`), its stack (for example `python`) and its status (for example `active`), so the organisation's repository list can be filtered without renaming anything.

### 3.7 Runners and Actions

Builds and tests run on GitHub-hosted runners. Deployments run only on College-managed runners. The organisation shares one Actions allowance, and spending past it is blocked, so checks run on pull requests rather than on every push where possible. Public repositories do not draw on the allowance.

Further development of runners and Actions is pending.

### 3.8 Secret scanning and Dependabot

Every repository uses secret scanning and push protection wherever GitHub provides them. Push protection stops a commit carrying a secret before it reaches GitHub. Every repository has Dependabot alerts turned on and someone who reads and acts on them.

---

## 4. Templates and research outputs

### 4.1 Starting a new repository

Start a new repository with:

- **`.editorconfig` and `.prettierrc.json`** from the `CoE-Config` repository.
- **The templates below,** saved as `.github/pull_request_template.md`, `.gitignore`, `.env.example` and `README.md`. The same files are in this standard's `templates/` folder.

#### Pull request template

```
## Issue / ticket

Closes #

## Description

What changed and why.

## Testing

How this was verified, and anything not covered.

## Checklist

- [ ] Reviewed my own diff
- [ ] Linted
- [ ] Ran the change locally
- [ ] Updated the README or docs if behaviour changed
- [ ] AI review with an approved tool (optional)
- [ ] Screenshots attached (optional, UI changes)
```

#### `.gitignore`

```
# Environment files: copy .env.example to .env locally; never commit .env
.env
.env.*
!.env.example

# Keys and credentials
*.pem
*.key
id_rsa*
id_ed25519*
id_ecdsa*
id_dsa*
credentials*.json

# Databases and data dumps: test data belongs in synthetic fixtures
*.db
*.sqlite*
*.dump

# Dependencies and environments
node_modules/
__pycache__/
.venv/

# Operating system and editor files
.DS_Store
Thumbs.db
.idea/
.vscode/
```

#### `.env.example`

```
# Copy this file to .env and fill in the values on your own machine.
# .env is never committed. Ask the project's lead how to get the real values;
# they are never sent in a Slack message.

# Which environment this is: local, dev or production
APP_ENV=

# Connection string for your local database
DATABASE_URL=

# Key for an external service the project calls
API_KEY=
```

#### README

```
# <Project name>

<One or two sentences: what this is, who uses it, and its state.>

## What it is

<What the project does and how it is put together. A Mermaid diagram or a screenshot helps here.>

## Run it locally

<Prerequisites, then the commands to install, configure (copy `.env.example` to `.env`) and start it.>

## Deploy it

<Where it runs and how a release reaches each place.>

## Owner

<The person responsible for this repository.>

## Team

<Everyone who has worked on it, including partners outside the College.>
```

### 4.2 Research outputs

Where it fits, a repository behind research can be registered on Zenodo for a DOI of its own, separate from any paper's. Minting one needs a release and a summary. Put the DOI badge at the top of the README, list the project team and external partners as authors, and add the DOI to each author's ORCID record. The web team wiki's "Registering Your Research Contributions" page walks through it. This is done where it fits and is not required.
