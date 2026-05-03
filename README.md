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
        └── skills/
            └── code-greeter/
                └── SKILL.md      # code-greeter skill
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
