# Installing Superpowers for Codex

Quick setup to enable superpowers skills in Codex.

## Installation

1. **Clone superpowers repository**:
   ```bash
   mkdir -p ~/.codex/superpowers
   cd ~/.codex/superpowers
   git clone https://github.com/obra/superpowers.git .
   ```

2. **Create personal skills directory**:
   ```bash
   mkdir -p ~/.codex/skills
   ```

3. **Update ~/.codex/AGENTS.md** to include this superpowers section:
   ```markdown
   ## Superpowers System

   <EXTREMELY_IMPORTANT>
   You have superpowers. Superpowers teach you new skills and capabilities. RIGHT NOW run: `~/.codex/superpowers/.codex/superpowers-codex bootstrap` and follow the instructions it returns.
   </EXTREMELY_IMPORTANT>
   ```

   **Hybrid Mode Guidance (Codex):**
   - Default to hybrid: subagents only for review/diagnostics.
   - Main agent owns edits, tests, and commits.
   - Subagents must not create/remove worktrees, finish branches, or spawn other agents.
   - If subagents are unavailable, run reviews manually with the templates.

## Verification

Test the installation:
```bash
~/.codex/superpowers/.codex/superpowers-codex bootstrap
```

You should see skill listings and bootstrap instructions. The system is now ready for use.
