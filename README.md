# Hat Full of Data Workshops

A Hugo site for Hat Full of Data public conference workshops.

## Requirements

- Hugo Extended v0.157.0 or later (required by Stack v4).
- Go 1.23.0 or later and Git for Hugo Modules.
- Git authentication with access to the private
  `github.com/HatFullOfData/hatfullofdata-hugo-shared` repository.

Configure Go to fetch the private module directly rather than using the public
module proxy or checksum service. In PowerShell, for the current session:

```powershell
$env:GOPRIVATE = "github.com/HatFullOfData/*"
```

If you already use `GOPRIVATE`, append this pattern to its comma-separated list.

## Local development

Clone `hatfullofdata-hugo-shared` alongside this repository:

```text
Website/
  workshops/
  hatfullofdata-hugo-shared/
```

From this repository, run:

```powershell
hugo server
```

Open <http://localhost:1313/>. Hugo's server uses the development environment,
which replaces the shared module with the sibling checkout. The replacement is
relative to Hugo's default `themesDir` (`themes`), so it deliberately uses
`../../hatfullofdata-hugo-shared`, not `../hatfullofdata-hugo-shared`.

The shared module is imported before Stack v4 so its presentation files take
precedence. Reusable layouts, partials, shortcodes and CSS should remain in the
shared module. This repository's site-specific `layouts/home.html` composes the
existing card and footer partials without copying their markup or styling.

The home page displays the introductory paragraph from `content/_index.md`,
followed by workshop sections directly under `content/workshops/`, ordered by
their front matter `weight` and then title. It does not list individual labs.
Fabric Introduction lives in `content/workshops/fabric-introduction/`,
with an overview (`_index.md`) and
separate page bundles for prerequisites, labs and resources. These pages contain
placeholders ready for authoring. Put images directly alongside each page's
`index.md`, without an images subdirectory.

## Module versions

`go.mod` and `go.sum` pin the remote modules. A production build uses those
versions instead of the local replacement:

```powershell
hugo --environment production
```

To update the shared module after committing and pushing changes there:

```powershell
hugo mod get -u github.com/HatFullOfData/hatfullofdata-hugo-shared
hugo --environment production
```

## GitHub Pages deployment

The workflow in `.github/workflows/hugo.yaml` builds and deploys on pushes to
`main`. It can also be started manually from the Actions tab. It uses Hugo
Extended, the Go version from `go.mod`, and the remote module versions pinned in
`go.mod` and `go.sum`. The sibling development replacement is not used in CI.

### One-time GitHub setup

1. In `HatFullOfData/workshops`, open **Settings > Pages**. Set **Build and
   deployment > Source** to **GitHub Actions**. No `gh-pages` branch is needed.
2. Create a fine-grained personal access token in your personal GitHub settings
   under **Developer settings > Personal access tokens > Fine-grained tokens**:
   - Choose the resource owner that owns `hatfullofdata-hugo-shared`.
   - Select **Only select repositories**, then `hatfullofdata-hugo-shared`.
   - Grant **Repository permissions > Contents: Read-only** (Metadata read access
     is automatic). No write permission is needed.
   - Set an expiry and arrange to rotate the secret before it expires.
   - If the owner requires approval, have the token approved before deploying.
3. In `HatFullOfData/workshops`, open **Settings > Secrets and variables >
   Actions** and add a repository secret named `HUGO_SHARED_MODULE_TOKEN` containing
   that token. Do not put the token in any tracked file. The default
   `GITHUB_TOKEN` cannot read a different private repository.
4. Ensure GitHub Actions is enabled in **Settings > Actions > General**, and that
   repository/organization policies allow the `actions/*` actions in the workflow.
   If the `github-pages` environment has deployment restrictions, allow `main`.
5. Commit and push this site, including the workflow, `go.mod` and `go.sum`, to
   `main`. Follow **Actions > Deploy Hugo to GitHub Pages** for the deployment URL.

The workflow gets the base URL from GitHub Pages, including the project path
(`/workshops/`) or an existing custom domain. Local configuration remains at
`http://localhost:1313/`; no deployment URL is hard-coded into local development.
The workflow grants Pages write and OIDC permissions only to the deploy job,
and does not persist the private module credential in Git configuration or
include source/module caches in the uploaded site artifact.

Changes made only in the sibling shared checkout will not be deployed. Commit
and push them in the shared repository, update the shared module version here,
then commit and push the updated `go.mod` and `go.sum`.

GitHub Pages publishes the generated site publicly. Do not place private workshop
content or secrets in content or static assets.
