# my-plugins — Claude Code Plugin Marketplace

A personal Claude Code plugin marketplace.

## Structure

```
plugin-marketpalce/
├── .claude-plugin/
│   └── marketplace.json          # marketplace definition
└── plugins/
    └── hello-world/              # example plugin
        ├── .claude-plugin/
        │   └── plugin.json       # plugin manifest
        ├── commands/
        │   └── hello.md          # /hello-world:hello slash command
        ├── agents/
        │   └── welcome-bot.md    # welcome-bot subagent
        ├── skills/
        │   └── code-greeter/
        │       └── SKILL.md      # code-greeter skill
        └── .mcp.json             # MCP servers (PyPI-pinned via uvx)
```

## Usage

### 1. Register the marketplace (local path)

In a Claude Code session:

```
/plugin marketplace add /Users/seungjoonlee/git/plugin-marketpalce
```

### 2. Install the plugin

```
/plugin install hello-world@my-plugins
```

### 3. Try it out

After installing, three things become available:

- **Slash command**: `/hello-world:hello Joon`
  → Greets you by name and prints the current directory and git branch.

- **Subagent**: When you enter a new codebase, ask "use welcome-bot to explain this project"
  → Summarizes what the project is, the tech stack, key folders, and how to run it.

- **Skill**: "apply code-greeter to add a header to src/index.ts"
  → Inserts or refreshes a standardized greeting comment block at the top of the file.

- **MCP servers**: `time` and `fetch` (Python, distributed via PyPI)
  → "현재 서울 시간 알려줘" / "https://example.com 페이지 내용 가져와".

## MCP server version control (PyPI 예시)

`plugins/hello-world/.mcp.json` 은 `uvx` 로 PyPI 패키지를 실행해서 MCP 서버를
띄우는 방식입니다. **PyPI 패키지명에 `==<version>` 을 붙여서 버전을 고정**하면
플러그인 사용자 환경마다 동일한 서버 버전이 실행되는 것을 보장할 수 있어요.

```json
{
  "mcpServers": {
    "time": {
      "type": "stdio",
      "command": "uvx",
      "args": ["mcp-server-time==0.6.2", "--local-timezone=Asia/Seoul"]
    }
  }
}
```

- `uvx <pkg>` → 항상 최신 버전 (재현성 ❌)
- `uvx <pkg>==1.2.3` → 버전 고정 (재현성 ✅, 강의/배포용 권장)
- `uvx <pkg>@latest` → 매 실행마다 최신 확인 (느릴 수 있음)

> 사전 준비: `brew install uv` (또는 `pipx install uv`).
> npm 기반 MCP 서버라면 `command: "npx"`, `args: ["-y", "@scope/pkg@1.2.3"]`
> 형태로 동일한 버전 고정 패턴을 적용할 수 있습니다.

## Marketplace management commands

```
/plugin marketplace list           # list registered marketplaces
/plugin marketplace update         # refresh marketplaces
/plugin marketplace remove my-plugins
/plugin                            # interactive plugin manager
```

## Adding a new plugin

1. Create a directory at `plugins/<plugin-name>/`.
2. Write `plugins/<plugin-name>/.claude-plugin/plugin.json`.
3. Add `commands/`, `agents/`, `skills/`, `hooks/`, or MCP server config as needed.
4. Add a new entry to the `plugins` array in the root `.claude-plugin/marketplace.json`.
5. Run `/plugin marketplace update` to refresh.
