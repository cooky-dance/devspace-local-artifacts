# DevSpace Local Artifacts

DevSpace Local Artifacts is a self-hosted MCP server for operating on approved
local workspaces from ChatGPT, Claude, or another MCP-capable host. This
release keeps the server local and adds guarded binary-artifact support for
Windows plus a Base64 fallback for hosts that cannot send native file
parameters.

## What this release adds

- `download_artifact` on Linux and Windows, with traversal, overwrite,
  symlink/junction, size, and publication-integrity checks.
- `write_binary` for strict Base64 strings and Base64 data URLs (maximum 32 MiB
  per inline payload).
- bounded MCP JSON parsing so oversized binary requests fail with a clear
  response instead of consuming unbounded memory.
- focused artifact, server-order, file-tool, performance, typecheck, and build
  checks.

## Requirements

- Node.js `>=22.19 <27`
- npm and Git
- Bash on Windows (Git Bash, WSL, MSYS2, or Cygwin)
- ngrok CLI only when a remote MCP host must reach the local server

The complete Node dependency graph is committed in `package-lock.json`.
`better-sqlite3` is a native dependency and `node-pty` is optional.

## Install from GitHub

```powershell
git clone https://github.com/cooky-dance/devspace-local-artifacts.git
Set-Location devspace-local-artifacts
npm ci
npm run build
npm install -g .
```

The global install exposes the `devspace` command. To work from the checkout,
use `npm run dev` instead.

## Install and configure ngrok

On Windows, run the bundled installer:

```powershell
.\scripts\install-ngrok.ps1
```

It uses the official Winget package `ngrok.ngrok` when ngrok is not already on
`PATH`. Token configuration is intentionally separate and interactive:

```powershell
ngrok config add-authtoken <YOUR_NGROK_TOKEN>
```

The token, `ngrok.yml`, OAuth password, local paths, and tunnel URL are runtime
data and must not be committed.

## Start DevSpace and the tunnel

Initialize once and choose only the local roots that the MCP host may open:

```powershell
$env:PORT = "3003"
$env:DEVSPACE_ARTIFACTS = "1"
devspace init
```

Start the server in one terminal:

```powershell
$env:PORT = "3003"
$env:DEVSPACE_ARTIFACTS = "1"
devspace serve
```

Start the tunnel in a second terminal, targeting the same port:

```powershell
ngrok http 3003
```

Set `DEVSPACE_PUBLIC_BASE_URL` to the HTTPS origin printed by ngrok (without
`/mcp`). The MCP client endpoint is that origin plus `/mcp`. A public tunnel is
not required when the MCP client runs on the same machine.

## Artifact workflow

1. Call `open_workspace` for an approved project directory.
2. For a host-provided native file, call `download_artifact` with the returned
   `workspaceId` and an unused relative destination such as
   `public/images/generated.png`.
3. If the host cannot provide a native file, call `write_binary` with a complete
   Base64 string or `data:image/png;base64,...` URL.

Both tools refuse absolute paths, traversal, existing destinations, and unsafe
parent directories. The feature is opt-in; keep `DEVSPACE_ARTIFACTS` unset or
`0` when binary writes are not needed.

## Verification

```powershell
npm run typecheck
npm test
npm run build
npm pack --dry-run
```

See [docs/dependencies.md](docs/dependencies.md),
[docs/setup.md](docs/setup.md), [docs/configuration.md](docs/configuration.md),
[docs/artifact-exchange.md](docs/artifact-exchange.md), and
[docs/security.md](docs/security.md) for detailed setup and safety boundaries.

## License

MIT. See [LICENSE](LICENSE).
