# Kora docs source and local reference

## Canonical source

Use `KoraFiveM/kora_docs_next` as the directory of shared guides and the
resource catalog. Each resource repository owns that resource's documentation.
Resolve repository default branches before fetching files; do not depend on a
particular published site URL.

The repository is a Docusaurus site. Its source has two main parts:

- `docs/` contains shared, unversioned guides. Fetch those from this repository.
- `catalog/resources.json` is the resource directory. It maps each listed
  resource to its owning Git repository, documentation path, entry page, latest
  stable version, and the commit and tag for each published version. It covers
  resources registered to the docs site; do not assume it lists every Kora
  resource repository.
- Resource documentation lives in its owning repository, commonly in `docs/`
  or a README file. Use the catalog's repository, `docsPath`, version, and
  pinned commit to retrieve released documentation from that repository.
  Resolve the entry page and linked pages from the repository's actual file
  listing; do not assume the entry is named `index.md` or that every page uses
  `.md`.
- `resource-<resource-id>_versioned_docs/` directories in the docs repository
  are published snapshots. They can help locate and search documented content,
  but the owning resource repository is the source to consult and cite.

The Docusaurus build uses generated working files. Prefer the committed shared
guides and catalog over `.generated/` or assumptions based on a website route.
For resource pages, use the catalog's `latest` value unless the user's project
identifies another version. If `docsAvailable` is false, `docsPath` is empty,
or the resource is absent from the catalog, search the owning resource
repository for documentation before concluding that none exists.

## Select a local reference location

Use an existing user-selected cache if one is available. Otherwise keep the
small reference in the agent's permitted task working location and continue.
If reuse across tasks would be useful, offer a shared cache outside the project
as an optional choice. Do not put downloaded docs into the installed skill
folder unless the user approves that addition.

## Refresh and retrieve only relevant pages

On each skill use, check the upstream default branch and refresh the local
catalog and shared-guide index from `KoraFiveM/kora_docs_next` when network
access is available. Search or refresh a lightweight index of `KoraFiveM`
repositories when the catalog does not identify the relevant resource or its
documentation; the catalog is not exhaustive. Record repo name, description,
default branch, and known documentation path when available, but do not download
every repository's docs. Refresh cached resource pages from
their owning repositories using the version commit recorded in the catalog.
Keep a small record of each source repository, commit SHA, and path with cached
pages so the agent can identify which docs revision it consulted.

Use the GitHub integration, `gh`, Git, or a browser/API tool available in the
current agent. Do not assume a specific connector is installed. Prefer fetching
the catalog and selected Markdown files over cloning every repository. Use only
the user's already configured GitHub access; never ask them to paste a token or
credential into chat.

For a task:

1. Read `catalog/resources.json` and identify relevant shared guides and
   resource IDs. Inspect the project's manifests, dependencies, and version
   declarations to select matching resource docs where possible.
   If the catalog does not resolve a resource, search the organization repository
   index because the catalog only covers resources registered to the docs site.
2. Fetch needed shared guides from `KoraFiveM/kora_docs_next`. For a resource,
   use its catalog entry to fetch the relevant files from the owning resource
   repository at the catalog-pinned commit for the selected version. Follow
   links within those pages when they are relevant.
3. Search the local pages for the API or behavior in question. If the local
   reference is incomplete, search the owning resource repository's file tree
   for additional documentation and retrieve only the relevant pages. If no
   catalog entry exists, search the refreshed `KoraFiveM` organization
   repository index for the resource and its documentation instead of treating
   the catalog as exhaustive or guessing a repository name. Inspect only likely
   repositories' file trees and fetch only relevant docs. If organization-wide
   discovery is unavailable, say what sources were searched and ask for the
   resource repo.
4. If the resource is not in the catalog, inspect the owning repository's tags,
   manifest version, and docs to match the project's installed version where
   possible. Use the default branch only when no matching released docs exist,
   and label that source as current or unreleased rather than a pinned release.
   Apply the same rule when the task explicitly concerns unreleased work.
   If the project names a version absent from the catalog, use the same tag and
   documentation search. Disclose any mismatch; do not present latest stable
   documentation as verified for that installed version.
5. When citing a fact, link to the actual source path in the owning repository.
   Use a commit-pinned GitHub URL when the exact commit is known. Cite shared
   guides from `KoraFiveM/kora_docs_next` and state the resource version when
   relevant.
6. If GitHub or network access is unavailable, use the newest local reference
   and disclose that it could not be refreshed. Do not present stale content as
   current. If neither local nor remote docs are available, ask the user for
   access or report the documentation gap.

If a resource repository is private or GitHub returns a not-found/access error,
check whether the matching pinned documentation snapshot is available in
`KoraFiveM/kora_docs_next`. Use that snapshot only as a clearly identified
fallback. If no snapshot is available, ask the user to connect an account with
access or provide the documentation; never treat an access error as proof that
the resource has no docs.

Do not copy additional pages into the installed skill's `references/` folder
until the user explicitly approves. An approved copy should retain its source
repository path, resource version, and commit SHA so it can be refreshed later.

## Examples of source selection

- A project manifest identifies resource version `1.2.0`. Find that version in
  the catalog, fetch the owning repository's docs at its recorded commit, and
  cite a URL pinned to that commit. If `1.2.0` is absent, inspect the owning
  repository's tags and state which revision supports the answer.
- A resource is absent from the catalog. Search the organization repository
  index for its owner, then inspect likely repositories for relevant docs. If
  the repository is private, use a matching published snapshot only when one
  exists and label it as a fallback. If network access is unavailable, identify
  the local reference revision and disclose that it could not be refreshed.
