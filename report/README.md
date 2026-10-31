# Report Template

A reusable LaTeX report template (A4, 10pt) with the HCMUT cover, one list that
fills both member tables, optional front/back matter, theorem boxes, code
listings, and biblatex + biber.

## Layout

```
report-template/          <- git repository (never upload this)
├── .git/
├── .gitignore
├── .gitattributes
├── report.zip            <- generated, ready to upload (not tracked)
└── report/               <- THE LATEX PROJECT: upload only this
```

```
report/
├── main.tex            <- document order + on/off switches
├── metadata.tex        <- cover text and running header
├── members.tex         <- ONE list, fills both member tables
├── refs.bib            <- bibliography
├── sections/           <- your writing
│   ├── 00-abstract.tex
│   ├── 01-introduction.tex   <- demo, replace it
│   ├── 99-conclusion.tex
│   └── A-appendix.tex
├── images/logo.png
└── format/             <- styling, you normally leave it alone
    ├── preamble.sty    <- packages, palette, header/footer, boxes
    └── cover.tex       <- cover page + member roles page
```

## The three files you edit

`metadata.tex` — cover text and header:

```latex
\newcommand{\orglineA}{VIETNAM NATIONAL UNIVERSITY HO CHI MINH CITY}
\newcommand{\orglineB}{HO CHI MINH CITY UNIVERSITY OF TECHNOLOGY}
\newcommand{\orglineC}{FACULTY OF COMPUTER SCIENCE AND ENGINEERING}

\newcommand{\doctype}{PROJECT REPORT}      % the red line on the cover
\newcommand{\coursename}{DATABASE SYSTEMS}
\newcommand{\coursecode}{CO2014}
\newcommand{\reporttitle}{ASSIGNMENT 1}
\newcommand{\instructor}{Vu Ngoc Tu, CSE-HCMUT}
\newcommand{\locationdate}{Ho Chi Minh City, March, 2026}
\newcommand{\reporthead}{Database Systems Assignment}   % top-right of every page
```

`members.tex` — one line per member, fields are
**name**, **student ID**, **role**, **completion**:

```latex
\newcommand{\members}{%
    \memberrow{Nguyen Van A}{2452001}{Analysed requirements, wrote section 1}{100}%
    \memberrow{Tran Thi B}{2452002}{Designed the EERD and schema}{100}%
}
```

That single list produces the cover table (Name | Student ID) and the roles
page (No. | Name | Role | Completion %), numbered automatically. An empty list
hides both.

`sections/` — your content. To add a chapter, create `sections/02-design.tex`
and add one line in `main.tex`:

```latex
\input{sections/02-design.tex}
```

## Switches in main.tex

```latex
\newif\ifshowmembers  \showmemberstrue    % cover member table + roles page
\newif\ifshowabstract \showabstracttrue   % abstract
\newif\ifshowlof      \showloftrue        % list of figures
\newif\ifshowlot      \showlottrue        % list of tables
\newif\ifshowappendix \showappendixtrue   % appendix
```

Set any to `false` for a shorter report. Page numbering: front matter roman,
body restarts at 1.

## Rebranding for another organisation

1. Replace `images/logo.png` with your logo (same filename).
2. Change the three `\orgline` lines in `metadata.tex`.
3. Optional: the palette lives at the top of `format/preamble.sty`
   (`lblue`, `dblue`, `blued`, `blu`, `gray1`, `jspink`, `gree`, ...).

Leave an `\orgline` empty to hide it; `\reportlogofalse` removes the logo from
cover and header.

## Writing

- Images: put them in `images/`, already on the graphics path:
  `\includegraphics[width=0.8\linewidth]{diagram.png}` (case-sensitive).
- Tables: `booktabs` rules, `tabularx` for full-width wrapping columns.
- Boxes: `theorem`, `definition`, `problem`, `example`, `corollary`,
  `solution`, `Proof`.
- Code: `\begin{lstlisting}[language=SQL] ... \end{lstlisting}`, or inline
  `\codeinline{SELECT * FROM t;}`.
- Citations: add to `refs.bib`, then `\textcite{sample-article}` or
  `\parencite{sample-online}`. IEEE style; change `style=` in
  `format/preamble.sty` for APA or numeric.
- References print at the end via `\printbibliography` and appear in the TOC.

## Building on Overleaf

1. Upload `report.zip` (already generated at the repository root).
2. **New Project -> Upload Project**, compiler **pdfLaTeX**. `main.tex` is the
   main document, already at the project root.
3. Biber runs automatically; if citations show `[?]`, recompile once more.

Regenerate the zip after changes (from the repository root, after committing):

```powershell
git archive --format=zip -o report.zip HEAD:report
```

Windows' `Compress-Archive` writes backslash paths that Overleaf's unzip treats
as literal filenames, so use the command above.

## Vietnamese text

Babel loads English as the main language with Vietnamese available. For a
mostly-Vietnamese report, switch the compiler to **XeLaTeX** and in
`format/preamble.sty` replace:

```latex
% remove
% \usepackage[utf8]{inputenc}
% \usepackage[T1]{fontenc}

% add
\usepackage{fontspec}
```

## The original report

The template was cleaned from a database-systems assignment (YouMed EERD); the
original is preserved in git:

```bash
git log --oneline
git show 4dec451 --stat
git archive 4dec451 -o original-report.zip
```
