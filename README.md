# TAMU Dissertation LaTeX Template

A LaTeX template for Texas A&M University theses and dissertations. It is based on the official TAMU Thesis Office template (included in `tamu/`) and adds some helpers: the dissertation is split into one file per chapter, front matter is configured in one place, and chapters based on published papers get the reprint-permission footnote automatically.

The repository contains placeholder content only. Replace it with your own.

## Structure

```
dissertation.tex        Main file: metadata, chapter order, your own packages
refs.bib                Bibliography (sample entries; replace with your own)
parts/
  00-front.tex          Abstract, dedication, acknowledgments, contributors/funding, nomenclature
  10-intro.tex          Chapter: Introduction
  20-chapter-a.tex      Chapter with examples of figure, table, equation, algorithm, citation
  30-chapter-b.tex      Chapter (placeholder)
  40-chapter-c.tex      Chapter (placeholder)
  90-conc.tex           Chapter: Conclusion and future work
  95-appendix-a.tex     Example appendix
tamu/
  tamuhelper.sty        Required packages, helper commands, title page, front matter, bibliography macros
  tamuconfig.sty        Formatting configuration from the TAMU Thesis Office
  *.txt, *.pdf          Original TAMU documentation and manuals
```

## Quick start

1. Open `dissertation.tex` and edit the block under `CONFIGURATIONS`: title, full name, degree, committee, department head, graduation month and year, and department.
2. Edit `parts/00-front.tex` for the abstract, dedication, acknowledgments, contributors and funding sources, and nomenclature.
3. Write each chapter in its own file under `parts/`.
4. Add your references to `refs.bib`.
5. Compile (see below).

The placeholder text comes from the `lipsum` package. Delete the `\lipsum[...]` commands as you write, and remove `\usepackage{lipsum}` when none remain.

## Compiling

```
pdflatex dissertation
bibtex dissertation
pdflatex dissertation
pdflatex dissertation
```

Or using my (LaTeX wrapper)[https://github.com/Naheel-Azawy/latexwrapper]:
```
latexwrapper dissertation.tex
```

Or using Overleaf.

The original TAMU notes recommend XeLaTeX (`tamu/Compile.txt`). If you use it, replace `pdflatex` with `xelatex` and `-pdf` with `-xelatex`. On Overleaf, set `dissertation.tex` as the main document and choose the compiler in the menu.

All required packages are loaded by `\mkpre{}` in `tamu/tamuhelper.sty`. Add your own packages and macros in the marked block at the top of `dissertation.tex`. Packages beyond a standard TeX Live installation include `lipsum`, `soul`, `algorithm`, `algpseudocode`, `arydshln`, `forarray`, `capt-of`, `subcaption`, and `bibentry`. A full TeX Live or Overleaf installation includes all of them.

## Adding and reordering chapters

Each chapter has two parts: a heading in `dissertation.tex` and a content file in `parts/`.

Chapter without a paper behind it:

```latex
\chapternormal{Chapter Title}\label{chap:my-label}
\includepart{50-my-chapter}
```

Chapter based on a published paper, which adds the footnote "This chapter including figures/tables are reprinted with permission from ...":

```latex
\chapterwithpreprint{Chapter Title}
{\mybibentry{bibtex-key}}\label{chap:my-label}
\includepart{50-my-chapter}
```

Here `bibtex-key` is the key of the paper in `refs.bib`, and `50-my-chapter` is the file `parts/50-my-chapter.tex` without the extension. Chapter titles are converted to uppercase automatically.

## Appendices

The appendix is defined at the end of `dissertation.tex` inside `\mkappendices{...}`. Add more appendices in the same way:

```latex
\mkappendices{
\chapternormal{First appendix}\label{chap:appendix-a}
\includepart{95-appendix-a}

\chapternormal{Second appendix}\label{chap:appendix-b}
\includepart{96-appendix-b}
}
```

Refer to an appendix with `\appx{appendix-a}`. To remove the appendix, delete the `\mkappendices` block.

## Helper commands

| Command | Purpose |
|---|---|
| `\chap{label}` | "Chapter N" reference to `\label{chap:label}`. Works across chapters. |
| `\appx{label}` | "Appendix X" reference to `\label{chap:label}`. |
| `\llabel{sec:x}`, `\llabel{fig:x}`, `\llabel{tbl:x}`, `\llabel{algo:x}` | Chapter-aware (local) label. Prevents label clashes between chapters. |
| `\sect{x}`, `\fig{x}`, `\tbl{x}`, `\algo{x}` | References to labels created with `\llabel`. |
| `\mybibentry{key}` | Prints the full formatted reference inline, for example in a list of publications. |
| `\TODO{text}` | Highlights a note for yourself. |

Important: `\sect`, `\fig`, `\tbl`, and `\algo` only resolve labels defined with `\llabel` in the same chapter file. To refer to something in another chapter, use a standard `\label` and `\ref`, or refer to the chapter with `\chap{}`. See `parts/20-chapter-a.tex` for working examples.

## Committee and title page

The title page in `tamu/tamuhelper.sty` (`\mktitlepage`) is set up for two co-chairs and four committee members, and `dissertation.tex` defines `\tamuchairone`, `\tamuchairtwo`, and `\tamumemberone` to `\tamumemberfour` accordingly.

To change the number of committee members, edit the committee table in `\mktitlepage` in `tamu/tamuhelper.sty` and add or remove the matching `\newcommand` lines in `dissertation.tex`. For a single chair, comment out the `Co-Chairs of Committee` line and uncomment the `Chair of Committee` line in the same table. Update `parts/00-front.tex` (Contributors section) to match.

## Other settings

- Bibliography style is `ieeetr`, set in `\mkbib` in `tamu/tamuhelper.sty`.
- Roman numerals for chapters: uncomment the `\thechapter` line in `dissertation.tex`.
- In lists, do not start an `\item` with a square bracket, because LaTeX reads it as the item label. Use `\item {[text]}` if needed.
- Before submission, check the output against the current TAMU Thesis Office requirements (`tamu/` contains the manual and FAQs). Requirements change, and this template is not an official TAMU product.

## Credits

- Base template: Texas A&M University Thesis Office (`tamu/tamuconfig.sty` and the documents in `tamu/`).
- `tamu/tamuhelper.sty` and the chapter-splitting workflow: by Naheel Faisal Kamal, 2024, based on the original TAMU template.
