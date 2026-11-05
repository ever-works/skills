---
name: graphify
description: >
    Use when a Task needs you to understand or navigate a code repository: where a symbol is defined, what calls
    it, how two modules connect, or what a change will affect. Applies when the Graphify CLI is installed or the
    repository already has a graphify-out/graph.json. Answers come from a local knowledge graph of the code
    (tree-sitter AST, no LLM, no API key), so you open only the files that matter instead of reading the tree.
license: Apache-2.0
compatibility: Requires the graphify CLI (PyPI package graphifyy, Python 3.10 or newer). Building and querying a graph needs no network and no API key.
tags: [code-navigation, knowledge-graph, refactoring, developer-tools]
metadata:
    author: ever-works
    version: '1.0.0'
allowed-tools: Bash(graphify:*)
---

# Navigate code with Graphify

[Graphify](https://github.com/Graphify-Labs/graphify) (Apache-2.0) turns a repository into a knowledge graph of
its code: files, functions, classes and types as nodes; imports, calls, references and inheritance as edges, each
with the source file and line. A question to the graph costs a few hundred tokens; reading the files it would
have sent you through costs thousands. Use the graph as a map, then confirm in the file before you edit.

## 1. Check what you have (seconds)

- `graphify --version` answers: the CLI is installed.
- `graphify-out/graph.json` exists at the repository root: there is a graph. In a git worktree the graph usually
  lives in the primary checkout; pass it with `--graph <primary checkout>/graphify-out/graph.json`.

| CLI | Graph | Do this |
| --- | --- | --- |
| yes | yes | Ask the graph first (section 2). |
| yes | no | Build one (section 4) when the Task will read a lot of code; for a one-file change, just read the file. |
| no | any | Install it only when your environment allows installs (`uv tool install graphifyy` or `pipx install graphifyy`). Otherwise work without it. Never block or fail a Task on Graphify. |

## 2. Ask the graph first

| Command | Answers |
| --- | --- |
| `graphify query "<question>"` | The nodes that match the question's words and their neighbourhood (breadth-first), each with file and line. `--budget N` caps the answer at N tokens (default 2000). |
| `graphify explain "<symbol>"` | One node and its connections: who imports it, what it calls, who calls it. |
| `graphify path "<A>" "<B>"` | The shortest chain of edges from A to B: how one module reaches another. |
| `graphify affected "<symbol>"` | What depends on the symbol (reverse traversal): read this before you change or delete it. |

- Use real identifiers in questions (`explain "createInvoice"`, not `explain "the invoice thing"`).
- Exit code 3 means the name is ambiguous: the candidates are listed, re-run with the one you mean. Exit code 4
  means no node matched: rephrase with an identifier from the code.
- Then open the files and lines the answer names. Fall back to search (`git grep`, `rg`) for string literals,
  configuration values, anything the graph does not model, and code changed since the graph was built: in a
  worktree that uses the primary checkout's graph, that includes every file your branch added or changed.

## 3. Keep the graph current

After you change code in a checkout that has its own `graphify-out/`, run `graphify update .` from the repository
root. It re-extracts the code (AST only, no LLM, no cost) so the next question sees your change. Do not build a
second graph inside a git worktree: it is slow on a large repository. Use the primary checkout's graph and search
your branch's own changes directly.

## 4. Build a graph

From the repository root: `graphify update .` (the same command builds the first graph and refreshes it later).
It writes `graphify-out/graph.json` plus a short `GRAPH_REPORT.md`. Expect seconds for a small repository and up
to an hour for a large TypeScript monorepo; run it once, early, not per question.

## 5. Never commit graphify-out/

`graphify-out/` is per-machine build output. Keep it out of every commit and pull request: if `git status` lists
it, add `graphify-out/` to `.git/info/exclude` (local, not a repository change) rather than staging it.
