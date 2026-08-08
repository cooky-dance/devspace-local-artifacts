# Dependencies and ngrok

This package keeps the local MCP server and the public tunnel as separate
components. No tunnel token, local path, OAuth password, or generated config is
stored in this repository.

## Required runtime dependencies

- Windows 10/11, Linux, or macOS
- Node.js `>=22.19 <27`
- npm
- Git
- Bash on Windows (Git Bash, WSL, MSYS2, or Cygwin)
- ngrok CLI when the MCP host is outside the local machine

The Node dependencies are pinned through `package-lock.json`. Native
`better-sqlite3` and the optional `node-pty` package are installed by npm for
the active Node runtime.

## Install from a checkout

```powershell
git clone https://github.com/cooky-dance/devspace-local-artifacts.git
Set-Location devspace-local-artifacts
npm ci
npm run build
npm install -g .
```

The global install exposes the `devspace` command. A local development checkout
can use `npm run dev` instead.

## Install ngrok on Windows

Run the bundled installer from an elevated or normal PowerShell prompt:

```powershell
.\scripts\install-ngrok.ps1
```

The script installs the official `ngrok.ngrok` Winget package if `ngrok` is not
already on `PATH`. It does not configure or read an ngrok token. Add your own
token interactively, outside this repository:

```powershell
ngrok config add-authtoken <YOUR_NGROK_TOKEN>
```

Never commit `%HOMEPATH%\\AppData\\Local\\ngrok\\ngrok.yml`, a token, or a
public tunnel URL that identifies a private deployment.

## Start the local server and tunnel

Use the same port in both commands. The example below uses `3003`; choose a
different port if your configuration requires it.

```powershell
$env:PORT = "3003"
$env:DEVSPACE_ARTIFACTS = "1"
devspace init
devspace serve
```

In a second PowerShell window:

```powershell
ngrok http 3003
```

Set `DEVSPACE_PUBLIC_BASE_URL` to the HTTPS origin printed by ngrok (without
`/mcp`) and connect the MCP client to the origin plus `/mcp`. A public tunnel is
only needed for a remote MCP host; local clients can use the local endpoint.
