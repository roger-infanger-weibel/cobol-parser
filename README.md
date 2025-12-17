# COBOL/JCL Cross-Reference Scanner

This project contains Python tools to analyze an IBM Mainframe (z/OS) source code structure (COBOL, JCL, Copybooks) and identify cross-references between them.

## Features

-   **Generic Scanning**: Scans all files in a directory regardless of extension.
-   **Cross-Reference Detection**:
    -   **PGM**: Identifies programs executed in JCL or called dynamically (matches `PGM=NAME` or `PGM='NAME'`).
    -   **CALL**: Identifies COBOL `CALL` statements (quoted or unquoted).
    -   **COPY**: Identifies COBOL `COPY` statements.
-   **Logic Exclusions**: Automatically ignores lines containing `SECTION.` to avoid false positives in section headers.

## Files

1.  **`scan_xrefs.py`**: The main scanner script. Run this to analyze your project.
2.  **`benchmark_scanner.py`**: A performance testing script comparing different regex strategies (whole-file vs line-by-line).
3.  **`line_scanner.py`**: A simple example script demonstrating the line-by-line scanning logic.

## Usage

1.  Open `scan_xrefs.py` and ensure the `base_dir` variable points to your project folder (or modify it to accept arguments).
    ```python
    base_dir = r"c:\path\to\your\project"
    ```
2.  Run the script:
    ```bash
    python scan_xrefs.py
    ```

## Output Format

The script outputs a pipe-delimited list of references:

```text
SOURCE                         | TARGET               | TYPE       | LINE
----------------------------------------------------------------------------------------------------
JOB01.jcl                      | MAIN01               | PGM        | //STEP01 EXEC PGM=MAIN01
MAIN01.cbl                     | SUBR01               | CALL       |        CALL 'SUBR01'
MAIN01.cbl                     | REC01                | COPY       |        COPY REC01.
```
