# latex-mermaid
LaTeX package to interface with mermaid (npm)

Source : https://github.com/Kochise/latex-mermaid

## Requirements

*   A working **Node.js** installation with `npm`. The `@mermaid-js/mermaid-cli` tool, which this package depends on, is a Node.js application.
*   **Shell escape enabled** during LaTeX compilation. This is required to allow LaTeX to run the `mmdc` command to generate diagrams.

    ```shell
    pdflatex -shell-escape your_document.tex
    xelatex -shell-escape your_document.tex
    ```

## installation

The installation process involves two main steps: installing the `@mermaid-js/mermaid-cli` tool and then installing the LaTeX package file.

### 1. Install `@mermaid-js/mermaid-cli`

You need `npm` (which comes with Node.js) to install the command-line tool. It's recommended to install it globally.

```shell
npm install -g @mermaid-js/mermaid-cli
```

### 2. Install the LaTeX package

#### For MiKTeX on Windows

Get 'nvm' for Windows : https://github.com/coreybutler/nvm-windows/releases<br>
Uncompress/install it where you need.<br>

```batch
\>cd nvm
\nvm>nvm install 12.17.0
\nvm>nvm use 12.17.0
\nvm>node -v
v12.17.0
```

Copy the package files:
```batch
xcopy /c /h /e /r /q /y . <miktex>\texmfs\install\tex\latex\mermaid
start "" "<miktex>\miktex-portable.cmd"
```

Refresh the filename database.<br>
Update the package database.<br>
You should be ready to go.<br>

Alternatively, you can use https://github.com/Kochise/win_portable

#### For TeX Live (macOS, Linux, Windows)

1.  Place the package files in your local `texmf` tree. You can find the root of this tree by running `kpsewhich -var-value TEXMFLOCAL`.
2.  Copy the package sources into the `tex/latex/` subdirectory of your local `texmf` tree.

    **On macOS / Linux:**
    ```shell
    # Example path, use the output of kpsewhich if different
    sudo cp -r . /usr/local/texlive/texmf-local/tex/latex/mermaid
    ```
    **On Windows:**
    You can use the File Explorer to copy the package folder into the directory given by `kpsewhich -var-value TEXMFLOCAL`, inside its `tex\latex\mermaid` subfolder.

3.  Update the TeX Live file database.
    ```shell
    sudo texhash
    ```
    On Windows, run `texhash` from a command prompt with administrator rights.

## Usage

1.  **Load the Package**

    Load the package in your preamble. It's recommended to set an `imagepath` where the generated diagrams will be stored. This path will be created automatically.

    ```latex
    \documentclass{article}
    \usepackage[imagepath=build]{mermaid}
    ```

3.  **Using an Output Directory (Out-of-Tree Builds)**

    If you compile your LaTeX document with an output directory (e.g., using the `-output-directory=build` flag), you **must** inform the `mermaid` package of this path. Use the `latexoutdir` option to specify the same path. This ensures that the package can correctly locate and process the generated diagram files.

    ```latex
    % Command line compilation
    xelatex -shell-escape -output-directory=build your_document.tex
    ```

    ```latex
    % In your preamble
    \documentclass{article}
    \usepackage[imagepath=images, latexoutdir=build]{mermaid}
    ```

    In this example, diagrams will be generated inside the `build/images` directory.

2.  **Create a Diagram**

    Use the `mermaid` environment to create your diagrams. The environment takes three arguments:
    - `[width]` (optional): The width of the figure (e.g., `8cm`, `0.8\textwidth`). Defaults to `\columnwidth`.
    - `{caption}`: The figure caption.
    - `{label}`: A unique label for the diagram, which is also used as the base for filenames.

    ```latex
    \begin{mermaid}[0.8\textwidth]{A Simple Flowchart}{flowchart-example}
    graph TD
      A[Start] --> B{Is it working?};
      B -->|Yes| C[Great!];
      B -->|No| D[Go back];
    \end{mermaid}
    ```

3.  **Customizing Options (Themes, etc.)**

    You can pass any command-line options to the `mmdc` tool using the `\mermaidsetup` command. This is useful for setting themes, background colors, or other parameters. The setting persists until it is changed again.

    ```latex
    % Set a global theme for all subsequent diagrams
    \mermaidsetup{-t neutral}

    % You can change it anytime for specific diagrams
    \mermaidsetup{-t dark -b '#333'}
    \begin{mermaid}{A Dark Diagram}{dark-diagram}
      graph TD
        A --> B
    \end{mermaid}
    ```

4.  **Compile Your Document**

    You must compile your LaTeX document with the `-shell-escape` flag enabled to allow the package to run the `mmdc` compiler.

    ```sh
    pdflatex -shell-escape your_document.tex
    ```
    or
    ```sh
    xelatex -shell-escape your_document.tex
    ```

    The package features a caching system. Diagrams will only be recompiled if their source code changes, speeding up subsequent compilations.

## options

What is available through 'mermaid-cli' :

```
% Output the version number
-V, --version

% Theme of the chart, could be default, forest, dark or neutral. Optional. Default: default (default: default)
-t, --theme [theme]

% Width of the page. Optional. Default: 800 (default: 800)
-w, --width [width]

% Height of the page. Optional. Default: 600 (default: 600)
-H, --height [height]

% Input mermaid file. Required.
-i, --input <input>

% Output file. It should be either svg, png or pdf. Optional. Default: input + ".svg"
-o, --output [output]

% Background color. Example: transparent, red, '#F0F0F0'. Optional. Default: white
-b, --backgroundColor [backgroundColor]

% JSON configuration file for mermaid. Optional
-c, --configFile [configFile]

% CSS file for the page. Optional
-C, --cssFile [cssFile]

% JSON configuration file for puppeteer. Optional
-p, --puppeteerConfigFile [puppeteerConfigFile]

% Output usage information
-h, --help
```

Cannot be specified on a group basis, only in the preamble.<br>
Just add the requested parameters without the leading dash(es).<br>

```latex
\usepackage[b={transparent}]{mermaid}
```

or :

```latex
\usepackage[backgroundColor={#F0F0F0}]{mermaid}
```

Avoid :

```
i, input	: because used by the provided text
o, output	: because using the refname
width		: because used to specify the tex picture width, not the output width
```

## limitations

Your imagination.

## greetings

Based on https://github.com/dakusui/latex-ditaa<br>
Also interested into https://github.com/sile-typesetter/sile<br>
