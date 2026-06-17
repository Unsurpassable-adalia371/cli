# Buddy CLI (`bdy`)

Work seamlessly with [Buddy](https://buddy.works) from the command line.

`bdy` is Buddy's official command-line interface for managing CI/CD pipelines, agents, tests, artifacts, sandboxes, distributions, and more — straight from your terminal.

📚 Full documentation: https://buddy.works/docs/cli/getting-started

## Installation

### npm (recommended)

Requires Node.js 14+.

```bash
# macOS / Linux
sudo npm i -g bdy

# Windows
npm i -g bdy
```

### Homebrew (macOS)

```bash
brew tap buddy/bdy
brew install bdy
```

### APT (Linux x64 / ARM64)

```bash
sudo apt-get update && sudo apt-get install -y software-properties-common
sudo gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/buddy.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys eb39332e766364ca6220e8dc631c5a16310cc0ad
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/buddy.gpg] https://es.buddy.works/bdy/apt-repo prod main" | sudo tee /etc/apt/sources.list.d/buddy.list > /dev/null
sudo apt-get update && sudo apt-get install -y bdy
```

> On ARM64, replace `arch=amd64` with `arch=arm64` in the `echo` command above.

### Chocolatey (Windows)

```powershell
choco install bdy --pre
```

### Direct download

Prebuilt binaries are available for macOS (Apple Silicon), Linux (x64/ARM64), and Windows (x64) on the [installation page](https://buddy.works/docs/cli/getting-started).

### Verify & update

```bash
bdy version          # show current and latest version
sudo npm i -g bdy    # update (or use your package manager's upgrade command)
```

## Authentication

A [Buddy account](https://buddy.works) is required to use the CLI.

Log in interactively:

```bash
bdy login
```

New to Buddy? Create an account from the CLI:

```bash
bdy register
```

### Linking a project

Once logged in, link a directory to a Buddy project:

```bash
mkdir my-proj
cd my-proj
bdy proj link
```

After linking, every `bdy` command run from this directory is executed against the linked project — no need to pass the project name each time.

Check who you're logged in as, or log out:

```bash
bdy whoami
bdy logout
```

### Tokens & non-interactive login

For CI/CD pipelines and non-browser environments, authenticate with a [personal access token](https://buddy.works/docs/api/getting-started/oauth2/personal-access-token):

```bash
bdy login --token <token>
```

The login command accepts the following options:

| Option | Description | Environment variable |
| --- | --- | --- |
| `--token <token>` | Personal access token | `BUDDY_TOKEN` |
| `--api <url>` | API URL for On-Premises installations | `BUDDY_API_ENDPOINT` |
| `--region <region>` | Region: `us`, `eu`, or `as` | `BUDDY_REGION` |
| `-w, --workspace <domain>` | Workspace URL handle | `BUDDY_WORKSPACE` |

Each option can be supplied as a flag or via its environment variable — handy for pipelines and headless setups:

```bash
export BUDDY_TOKEN=<token>
export BUDDY_WORKSPACE=<your-workspace>
bdy login
```

## Agent integration (plugin & skills)

The [Buddy Plugin](https://github.com/buddy/buddy-plugin) lets coding agents deploy applications, expose local services, and manage infrastructure on Buddy. It bundles a consolidated skill covering sandboxes, artifacts, tunnels, domains, distributions, and pipelines, plus two commands:

- `/deploy [name] [path]` — deploy a static site or server application
- `/expose [port]` — open a tunnel to a locally running service

Make sure the CLI is installed first (`sudo npm install -g bdy`), then:

**Claude Code** — install the plugin (commands + skills):

```bash
claude plugin marketplace add buddy/buddy-plugin
claude plugin install buddy@buddy-plugin
```

**Other agents** — install the skills only:

```bash
npx skills add buddy/buddy-plugin
```

Then authenticate and link your project:

```bash
bdy login
cd your-project
bdy proj link
```

The plugin auto-detects your project type: static sites generate versioned artifacts with public URLs, while server applications deploy to sandboxes with HTTPS endpoints. Monorepos can deploy multiple applications, each getting its own URL and endpoint.

## Commands

| Command | Description |
| --- | --- |
| `bdy login` / `sign-in` | Log in to Buddy |
| `bdy register` / `sign-up` | Register a new Buddy account |
| `bdy whoami` | Check login information |
| `bdy logout` | Log out from Buddy |
| `bdy workspace` / `ws` | Manage workspaces |
| `bdy project` / `proj` | Manage projects |
| `bdy pipeline` / `pip` | Interact with the pipeline service |
| `bdy artifact` / `art` | Interact with the artifact service |
| `bdy sandbox` / `sb` | Interact with sandboxes |
| `bdy agent` | Install and run `bdy` as an OS service (Windows, macOS, Linux) |
| `bdy tunnel` | Manage tunnel configuration |
| `bdy crawl` | Manage web crawls (Markdown, HTML, PNG/JPEG screenshots; suite linking) |
| `bdy tests` | Manage unit tests and visual regression (Storybook, URL captures, runners) |
| `bdy domain` | Manage zones used in distribution routes |
| `bdy distro` | Manage distributions |
| `bdy api` | Contact the Buddy API directly |
| `bdy update` / `version` | Show version and update the CLI |

Run any command with `-h` / `--help` for detailed usage:

```bash
bdy --help
bdy pipeline --help
```

## Feedback

Found a bug or have a feature request? Let us know through [Buddy support](https://buddy.works/support) or the [documentation](https://buddy.works/docs/cli/getting-started).
