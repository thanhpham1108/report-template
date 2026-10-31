# Report Template

A ready-to-use report template: HCMUT cover page, member and role tables that
fill themselves in from one list, table of contents, appendix, references.
Nothing to install — you edit the text, upload the folder to Overleaf, and
download the PDF.

---

## Use it in 4 steps

### 1. Download this project

On this page click the green **Code** button → **Download ZIP**, then unzip it
somewhere easy to find (for example your Desktop).

The report itself is inside the **`report`** folder. That is the only folder that
matters.

### 2. Fill in the cover page

Open **`report/metadata.tex`** in any text editor (Notepad, VS Code, or directly
in Overleaf later). Replace the text between the braces:

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
\newcommand{\reporthead}{Database Systems Assignment}
```

- `\doctype` is the red line: `PROJECT REPORT`, `ASSIGNMENT`, `LAB REPORT`…
- `\reporthead` is the small text in the top-right corner of every page.
- Leave a line empty (`{}`) and it disappears.

### 3. Fill in the members

Open **`report/members.tex`**. One line per person:

```latex
\newcommand{\members}{%
    \memberrow{Nguyen Van A}{2452001}{Analysed requirements, wrote section 1}{100}%
    \memberrow{Tran Thi B}{2452002}{Designed the EERD and schema}{100}%
}
```

The four fields are **name**, **student ID**, **role**, **completion %**. This
one list builds both the table on the cover and the "Roles of the members" page,
numbered automatically. Add or delete lines freely.

Then write the report in **`report/sections/`**. `01-introduction.tex` is a small
demo — replace it. To add a chapter, create `report/sections/02-design.tex` and
add one line in `main.tex`:

```latex
\input{sections/02-design.tex}
```

### 4. Zip it and upload it

1. Open the **`report`** folder.
2. Select everything **inside** it (Ctrl + A).
3. Right-click → **Send to → Compressed (zipped) folder**
   (Windows 11: **Compress to ZIP file**).
4. Go to [overleaf.com](https://www.overleaf.com) → **New Project** →
   **Upload Project** → choose that zip.
5. Press **Recompile**, then use the **Download PDF** button.

> **Check the zip before uploading:** double-click it. You should see
> `main.tex` immediately. If you only see a single `report` folder, you zipped
> the folder instead of its contents — go back, open `report`, and zip what is
> inside.

The compiler stays on **pdfLaTeX** and `main.tex` is already the main document,
so there is nothing to configure. If references show `[?]`, press Recompile once
more.

---

## What to change, and what to leave alone

| What you want to change | File |
|---|---|
| Cover text, course, instructor, date, page header | `report/metadata.tex` |
| Names, student IDs, roles, completion % | `report/members.tex` |
| Chapters, abstracts, conclusions, appendix | `report/sections/` |
| References | `report/refs.bib` |
| Logo | replace `report/images/logo.png` (keep the name) |
| Colours and layout | `report/format/` — you can ignore it |

## Optional pages

At the top of **`report/main.tex`** five switches turn pages on and off:

```latex
\newif\ifshowmembers  \showmemberstrue    % cover member table + roles page
\newif\ifshowabstract \showabstracttrue   % abstract
\newif\ifshowlof      \showloftrue        % list of figures
\newif\ifshowlot      \showlottrue        % list of tables
\newif\ifshowappendix \showappendixtrue   % appendix
```

Change a `true` to `false` to drop that page. A short assignment usually turns
off the abstract, the two lists and the appendix.

## Writing help

```latex
% a figure (file goes in report/images/)
\begin{figure}[H]
    \centering
    \includegraphics[width=0.8\linewidth]{diagram.png}
    \caption{What the reader should notice.}
    \label{fig:diagram}
\end{figure}
See Figure~\ref{fig:diagram}.

% a box
\begin{definition}[Access control]
    A mechanism that restricts which subjects may do what.
\end{definition}

% code
\begin{lstlisting}[language=SQL]
SELECT * FROM students;
\end{lstlisting}

% citation: add the entry to refs.bib first
\textcite{key}      \parencite{key}
```

Available boxes: `theorem`, `definition`, `problem`, `example`, `corollary`,
`solution`. Named sections and labels are numbered automatically; keep labels
unique with a prefix (`fig:`, `tab:`, `sec:`).

## If something goes wrong

- **`File not found` for an image** — the filename in the zip differs in case or
  spelling from what you typed. Names are case-sensitive.
- **The zip opens into a folder instead of `main.tex`** — see the check in step 4.
- **`[?]` instead of a citation** — press Recompile again; the bibliography runs
  on the second pass.
- **Vietnamese letters look wrong** — switch the Overleaf compiler to
  **XeLaTeX** (Menu → Compiler) and follow the note at the top of
  `report/format/preamble.sty`.

## For reference

This template came from a database-systems assignment (YouMed EERD); the
untouched original is kept in the repository history, in case you ever want to
compare.
