# Cagoule Agent Plugin

Install the Cagoule skill plugin and the CLI, then ask your agent to deploy or run a model. The skill can install the CLI through Homebrew when available. First use runs `cagoule login` and opens Google sign-in; choose your `@anorak.games` account. Agents and terminal commands use the same executable and device login. Remote workflows need no tokens, Google CLI, Go, Python, Docker, Cagoule checkout or GitHub account. Agents must support local command execution.

The public marketplace contains the packaged plugin. Platform access requires Anorak Google sign-in.

## Codex desktop — workspace installation

A workspace administrator adds the marketplace once:

1. Open **Workspace settings → Plugins**.
2. Select **Add → Import marketplace**.
3. In **Source**, enter `https://github.com/anorak-games/cagoule-agent-plugin`.
4. Leave **Path** and **Branch, tag, or commit** empty: the marketplace is at the repository root and uses the default branch.
5. Select **Import marketplace** and authorize GitHub access when prompted.
6. Check the import results, open **Cagoule**, and set its installation policy to **Available** for the employees who will use it.

Employees can then install it:

1. Open Codex in the workspace where the administrator imported the marketplace.
2. Open **Plugins**, find **Cagoule**, and click **Install**.
3. Start a new local task and ask: “Use Cagoule to find the available models.”
4. When the browser opens, sign in with your `@anorak.games` Google account. A previously saved login may be reused without opening the browser.

This route requires workspace administrator access for the initial import. For an individual installation without workspace import, use the Codex CLI commands below; they also configure the local desktop app. Restart the desktop app after installing.

## Codex CLI

Run in a terminal:

```sh
codex plugin marketplace add https://github.com/anorak-games/cagoule-agent-plugin.git
codex plugin add cagoule@cagoule
```

Start a new Codex session and ask: “Use Cagoule to find the available models.” Sign in with your `@anorak.games` account when prompted.

## Claude desktop — Code tab

These instructions apply to local Claude Code sessions in the desktop app.

1. Register the marketplace once from a terminal with the Claude Code CLI available:

   ```sh
   claude plugin marketplace add https://github.com/anorak-games/cagoule-agent-plugin.git
   ```

2. Open the desktop app's **Code** tab and start a **Local** session.
3. Click **+** beside the prompt box, then **Plugins → Add plugin**.
4. Find **Cagoule** in the **Cagoule** marketplace and install it for your user account.
5. Start a new session and ask: “Use Cagoule to find the available models.” Sign in with your `@anorak.games` account when prompted.

## Claude Code CLI

Run in a terminal:

```sh
claude plugin marketplace add https://github.com/anorak-games/cagoule-agent-plugin.git
claude plugin install cagoule@cagoule --scope user
```

Start a new Claude Code session and ask: “Use Cagoule to find the available models.” Follow browser sign-in when prompted.

## Command line

Install the CLI used by both people and agents:

```sh
brew install anorak-games/cagoule/cagoule
cagoule login
cagoule deploy path/to/app
```

Homebrew serves macOS and Linux from the same formula; `brew upgrade cagoule` updates it. Windows users download `cagoule_<version>_windows_<arch>.zip` from the [releases](https://github.com/anorak-games/homebrew-cagoule/releases). The command line and the plugin share one device login. Run `cagoule` with no arguments for the command list.

## Updates

Check explicitly with `cagoule version --check` or `cagoule --json version --check`. Plain `version` and `--version` work offline. The check reports current, outdated, ahead, development or unknown; an unavailable check exits nonzero. It does not update the CLI or block other commands.

For Homebrew, run `brew update && brew upgrade cagoule`. For Windows/manual installations, download the matching OS/architecture archive from the latest release, verify its SHA-256 against `SHA256SUMS`, and replace the executable on PATH. Do not automatically replace development builds.

The plugin contains skills and metadata; CLI updates do not require a plugin refresh. Refresh the plugin when its guidance changes. Users moving from the bundled MCP plugin must refresh it once, install the CLI, and restart their agent session to retire the old MCP server. Sign-in survives this change.


For Codex, refresh the marketplace and install the current version:

```sh
codex plugin marketplace upgrade cagoule
codex plugin add cagoule@cagoule
```

For Claude Code:

```sh
claude plugin marketplace update cagoule
claude plugin update cagoule@cagoule --scope user
```

Restart the desktop app or start a new CLI session after updating. Your Anorak login is stored separately and survives plugin updates and reinstalls.

## Using it

Ask “Create a variant of EXISTING_MODEL using this public Hugging Face LoRA”, or “Deploy this public Hugging Face model and generate a small example.” The agent exports application source or creates a project, edits it, imports weights in the cloud, builds and generates. It returns exact app revisions, request IDs, links and local output paths.

Sign-in is kept in macOS Keychain, Windows Credential Manager, or an owner-only Linux file in your configuration directory. `cagoule logout` removes the shared device login; it does not stop builds or generations. Use `cagoule cancel ID` to stop generation. After a client restart, the agent can recover status using IDs and reuse the same generation operation to avoid duplicate work.

Public Hugging Face repositories are supported, including gated ones once the platform's Hugging Face account has accepted the repository's terms; private repositories are not. Only the platform's single-worker inference contract is supported. Model-specific loading code is still required. Source archives that were previously deleted cannot be recovered from an image.

Installation references: [Codex workspace import](https://help.openai.com/en/articles/20001504), [Codex local marketplaces](https://developers.openai.com/plugins/build/plugins#add-a-marketplace-from-the-cli), [Claude Code desktop plugins](https://code.claude.com/docs/en/desktop#install-plugins).
