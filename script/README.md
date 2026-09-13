# Microblog

Requires [Rugo](https://github.com/rubiojr/rugo) (tested with v0.30.1) and Vim.
Run from the repository root:

```sh
rugo run script/microblog.rugo add
```

Write the post's Markdown body in Vim, then `:wq` to publish. The tool adds
today's date and indents continuation lines, so paragraphs, lists and code
blocks stay within the entry. An empty draft cancels; `:cq` aborts with the
draft retained for recovery. Failed updates also retain the draft and print
its path.

Each successful add keeps the latest three dated entries in the README and
moves older entries to `archives/YYYY.md`. Archive links appear below the
posts, newest year first. Entries with the same date keep their order, with
new posts first.

```sh
# Reorganize existing entries without opening Vim.
rugo run script/microblog.rugo archive

# Use a specific date or README.
rugo run script/microblog.rugo add --date 2026-09-13
rugo run script/microblog.rugo archive --readme /path/to/README.md

# Show CLI help.
rugo run script/microblog.rugo --help

# Run the tests.
rugo rats script/microblog.rugo
```

The README must have a `# Changelog` section. Entries use
`* YYYY-MM-DD - text`, with continuation lines indented by at least two spaces
or a tab. Other Markdown sections are preserved. The archive directory is
relative to the README. Archive files use `# Changelog — YYYY` followed by
the same entry format. The HTML-comment-delimited archive links are managed
by the tool.
