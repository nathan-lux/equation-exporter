# LaTeX Equation PNG Exporter

Export the displayed equations in a LaTeX document as tightly cropped,
transparent PNGs. The script uses the document's preamble and follows its
section files so the output filenames match the compiled equation numbers.

## Requirements

- Python 3.9 or later
- `pdflatex` from a TeX distribution with `amsmath`, `refcount`, `xcolor`,
  `preview`, and the packages required by your document
- `pdftocairo` from Poppler

Both executables must be on your PATH. The script uses Python's standard library.

## Usage

From the repository folder, pass the main TeX file and the output folder:

```sh
python3 export_equations.py /path/to/main.tex ./equation-pngs
python3 export_equations.py /path/to/main.tex ./equation-pngs --color '#28649A' --dpi 900
```

The default is black at 600 DPI. Use a six-digit hex color or an `xcolor` name
such as `white`. Explicit colors in the TeX source are preserved.

Pass the main document with its preamble so LaTeX can resolve macros and
numbering across the files loaded by `\input` and `\include`. The script builds
in a temporary directory and leaves the source files untouched. Output folders
inside a directory named `Paper-Drafts` are rejected. Running the script again
replaces PNGs with matching filenames.

## Filenames

- Equation 4 with label `eq:power-spectrum` becomes `4_eq-power-spectrum.png`.
- A two-line equation 15 with label `eq:spectrum` becomes
  `15_eq-spectrum_line-01.png` and `15_eq-spectrum_line-02.png`.
- Each numbered `align` row uses its own number. Untagged rows share the next
  number in the alignment, or the last number if they follow it.
- Fully unnumbered displays use names such as `unnumbered-001.png`.

## Compatibility

- Pass the main document. The full document must compile with `pdflatex` and
  shell escape disabled. Documents that require XeLaTeX, LuaLaTeX, or
  shell escape need changes to the exporter.
- Supported displays include `equation`, `align`, `alignat`, `flalign`,
  `gather`, `multline`, `eqnarray`, and `displaymath`, their standard starred
  forms where applicable, and the delimiters `\[...\]` and `$$...$$`. Inline math
  is skipped. LaTeX supplies the numbers for subequations, appendix equations,
  and custom tags.
- The exporter scans for display environments and input paths written directly in
  the source. It can miss custom environments, macros that generate entire
  displays, and paths assembled by macros.
- Extracted equations are rendered using the document preamble. If an equation
  relies on a macro defined only in the document body, move the definition into
  the preamble for the export to work.
