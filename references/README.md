# references/ — the shelf

Source PDFs live here, sorted by type (enforced: an entry's BibTeX type
must match its section):

```
references/
├── paper/    @article, @inproceedings, @incollection — journal and
│             conference papers
├── book/     @book — published books
└── manual/   @manual — official vendor documentation (datasheets,
              user guides, e.g. Xilinx UG/DS documents)
```

This directory is the **only** source of truth the AI may cite from,
enforced by code, not convention:

- Every entry in `references.bib` must carry a
  `file = {references/<name>.pdf}` field pointing to a file that exists
  here (validator check `E-BIBSRC`). No file on the shelf, no citation.
- Every `\cite`/`@key` must resolve to a bib entry (`C-KEY`), and entry
  identifiers (DOI/ISBN) are verified against Crossref/OpenLibrary in CI.
- The AI's internet access is gated (`enforcement.web_gate`): it must ask
  you before any web search, and may only *suggest* new references.

## The reference workflow

1. The AI works from this shelf. When a claim has no supporting source
   here, it must say so — not improvise.
2. If you approve, it searches the web and suggests references
   (title, authors, year, DOI/ISBN, why it fits).
3. **You** download the PDF and place it in this folder.
4. The AI adds the bib entry with the `file` field and only then cites it.

Example entry:

```bibtex
@book{mano2013,
  author    = {M. Morris Mano and Michael D. Ciletti},
  title     = {Digital Design},
  publisher = {Pearson},
  year      = {2013},
  isbn      = {978-0132774208},
  file      = {references/book/mano2013-digital-design.pdf}
}
```

Note: `@manual` is not in `allowed_types` by default — official vendor
documentation is permitted via `type_exceptions` in
`harness/40-citations.json` (e.g. `{"type": "manual",
"key_prefix": "xilinx"}`), so only the vendors you explicitly trust
can enter the bibliography as manuals.
