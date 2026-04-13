# Context Vaults

This file lists other Wiki-PM vaults that can be referenced as context.
The `/wiki-context` command reads the `index.md` of each vault listed here
and injects their TLDRs as read-only context — content is never copied.

---

> ⚠️ **Path safety rule:** Only reference sibling folders at the same directory level as this vault.
> Never use `../../` patterns that escape upward past the parent folder — this can allow an LLM agent
> to traverse into unrelated directories (other projects, system files, SSH keys).
>
> **Safe:** `path: ../company-okrs-vault` (sibling vault in the same workspace folder)
> **Unsafe:** `path: ../../` or `path: ~/other-project` or any path containing `..` more than once

---

## How to add a reference

```
- path: ../sibling-vault-folder
  description: One sentence — what knowledge lives there
  use_when: When to pull context from this vault
```

The LLM reads only the `index.md` of referenced vaults —
it does not traverse arbitrary files or subdirectories.

## Active References

<!-- Add your cross-vault references below this line -->
