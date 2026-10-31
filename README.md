# Report Template

A reusable LaTeX report template (A4, 10pt, HCMUT cover by default) with a
parameterised title page, one-list member tables, optional front/back matter,
theorem boxes, and biblatex + biber.

## Structure

```
report-template/
├── main.tex            <- document order + on/off switches
├── metadata.tex        <- organisation, course, title, instructor, date
├── members.tex         <- ONE list drives both member tables
├── preamble.sty        <- packages, layout, header/footer, theorem boxes
├── branding.tex        <- palette + logo path
├── titlepage.tex       <- cover, built from metadata
├── contributions.tex   <- roles / completion table, built from members
├── refs.bib            <- bibliography database
├── COMPONENTS.tex      <- preview/copy-paste cookbook (not in the report)
├── sections/
│   ├── 00-abstract.tex
│   ├── 01-introduction.tex   <- demo, replace it
│   └── 99-conclusion.tex
├── appendices/A-appendix.tex
└── images/logo.png
```

## Quick start

1. Copy the whole folder and rename it after the report.
2. Edit `metadata.tex` (organisation, course, title, instructor, date).
3. Edit `members.tex` (one line per member; roles may contain commas).
4. Write `sections/`, adding one `\input` line per file in `main.tex`.

## Metadata

```latex
\newcommand{\orglineA}{VIETNAM NATIONAL UNIVERSITY HO CHI MINH CITY}
\newcommand{\orglineB}{HO CHI MINH CITY UNIVERSITY OF TECHNOLOGY}
\newcommand{\orglineC}{FACULTY OF COMPUTER SCIENCE AND ENGINEERING}

\newcommand{\doctype}{PROJECT REPORT}
\newcommand{\coursename}{DATABASE SYSTEMS}
\newcommand{\coursecode}{CO2014}
\newcommand{\reporttitle}{ASSIGNMENT 1}
\newcommand{\instructor}{Vu Ngoc Tu, CSE-HCMUT}
\newcommand{\locationdate}{Ho Chi Minh City, March, 2026}
\newcommand{\reporthead}{CO2014 --- Database Systems}   % running header
```

- `\doctype` is the red line on the cover: `PROJECT REPORT`, `ASSIGNMENT`,
  `LAB REPORT`, `THESIS`, anything.
- The running header is `\reporthead`; keep it short.

## Rebranding for another organisation

1. Replace `images/logo.png` with your logo (same filename).
2. Change the three `\orgline` entries in `metadata.tex`.

Leave an `\orgline` empty to hide that line. Set `\reportlogofalse` in
`branding.tex` to remove the logo from both the cover and the header.

## Members

```latex
\newcommand{\members}{%
    \memberrow{Nguyen Van A}{2452001}{Analysed requirements, wrote section 1}{100}%
    \memberrow{Tran Thi B}{2452002}{Designed the EERD and schema}{100}%
}
```

Fields: **name**, **student ID**, **role**, **completion**. The same list
produces the cover table and the contribution table, numbered automatically.
Leave `\members{}` empty to hide both.

## Switches in main.tex

```latex
\newif\ifshowmembers  \showmemberstrue   % cover + contribution tables
\newif\ifshowabstract \showabstracttrue  % abstract page
\newif\ifshowlof      \showloftrue       % list of figures
\newif\ifshowlot      \showlottrue       % list of tables
\newif\ifshowappendix \showappendixtrue  % appendix
```

For a short assignment, turn off `showabstract`, `showlof`, `showlot` and
`showappendix`.

## Writing sections

Create `sections/02-design.tex`:

```latex
\section{Design}
\label{sec:design}

\subsection{Data model}
Text, then a reference: Figure~\ref{fig:schema}.
```

and add one line in `main.tex`:

```latex
\input{sections/02-design.tex}
```

Page numbering: front matter is roman, the body restarts at 1, the header shows
`current / total` automatically (`lastpage`).

## Figures and tables

Put images in `images/` and reference them by filename only — the folder is
already on the graphics path:

```latex
\includegraphics[width=0.8\linewidth]{diagram.png}
```

Filenames are case-sensitive on Overleaf. Use lowercase, no spaces.

Tables: `booktabs` for rules, `tabularx` for full-width wrapping columns. See
`COMPONENTS.tex`.

## Citations

`biblatex` + `biber`, IEEE style. Add entries to `refs.bib`, then:

```latex
\textcite{sample-article}      % Doe and Roe (2024) show ...
\parencite{sample-online}      % (WHO, 2025)
```

References are printed at the end by `\printbibliography` in `main.tex` and
appear in the table of contents. To change style, edit one line in
`preamble.sty`: `style=ieee` -> `style=apa`, `style=numeric`, ...

## Theorem-like environments

Available everywhere: `theorem`, `definition`, `problem`, `example`,
`corollary`, `solution`. They are styled with the brand palette and defined in
`preamble.sty`.

## Previewing components

Set `COMPONENTS.tex` as the main document in Overleaf to see every building
block (figures, subfigures, tables, lists, theorem boxes, code, citations,
equations, callout, landscape page). Copy what you need, then set `main.tex`
back as the main document.

## Building on Overleaf

1. Zip the **contents** of `report-template/` so `main.tex` is at the zip root.
2. Overleaf: **New Project -> Upload Project**.
3. Compiler: **pdfLaTeX**. Biber runs automatically for biblatex; if the
   bibliography shows `[?]`, recompile once more or check
   **Menu -> Settings -> TeX Live version** is current.
4. `main.tex` must be the main document.

## Vietnamese text

The template loads `babel` with English as the main language and Vietnamese
available. For a mostly-Vietnamese report, switch the compiler to **XeLaTeX**
and replace the font block in `preamble.sty`:

```latex
% remove
% \usepackage[utf8]{inputenc}
% \usepackage[T1]{fontenc}

% add
\usepackage{fontspec}
```

## The original report

This template was cleaned from a database-systems assignment (YouMed EERD).
The original is preserved in git:

```bash
git log --oneline          # 4dec451 is the original import
git show 4dec451 --stat
git checkout 4dec451 -- .  # restore into the working tree (careful)
```

## Conventions

- One idea per paragraph; use `\autoref`/`\ref` for every figure and table.
- Keep labels unique and prefixed: `fig:`, `tab:`, `sec:`, `eq:`, `app:`.
- Prefer `booktabs` over vertical rules.
- Put images in `images/`, content in `sections/`, nothing else at the root.
