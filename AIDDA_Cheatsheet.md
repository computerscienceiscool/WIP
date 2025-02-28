# AIDDA Cheat Sheet

## Overview
**AI-Driven Development Assistant (AIDDA)** automates Go development using **Test-Driven Development (TDD)** principles with AI assistance.

- Iterates over test cases and comments to generate code.
- Analyzes test quality, highlighting ambiguities.
- Uses **GPT-4-128k** for better context understanding.
- Runs on **Linux** via a shell script (`aidda.sh`).

---

## 1. Usage & Command-line Options

usage: aidda.sh { -b branch} { -I container_image } {-a sysmsg | -c | -t | -s sysmsg }
   [-A 'go test' args ] [ -p input_patterns_file ] [outputfile1] [outputfile2] ...

### **Modes of Operation**
| Option | Description |
|--------|-------------|
| `-a sysmsg` | Skip tests and provide **advice** from the LLM. |
| `-c` | **Write code** based on test cases. |
| `-t` | **Write test cases** instead of code. |
| `-s sysmsg` | **Execute a custom system message**. |

### **Version Control & Containerization**
| Option | Description |
|--------|-------------|
| `-b branch` | Specifies a **Git branch**. |
| `-I container_image` | Specifies a **container image** (Docker). |

### **Test Customization**
| Option | Description |
|--------|-------------|
| `-A 'go test' args` | Pass extra arguments to `go test`. |
| `-T test_timeout` | Set test timeout (e.g., `-T 1m`). |

### **Chat & Input Handling**
| Option | Description |
|--------|-------------|
| `-C chatfile` | Continue from an **existing chat file**. |
| `-p input_patterns_file` | Use an **input patterns file** for file selection. |

---

## 2. Workflow from Inside Vim

### **Step 1: Open Prompt File in Vim**
1. Open the AIDDA prompt file: `:e .aidda/prompt`
2. The structure of the file is as follows:
   - First line: Commit message summary (short)
   - Second line: (Blank)
   - Third+ lines: Commit message body
   - Last few lines: AIDDA directives (not part of commit message)
   - **Sysmsg:** System message sent to LLM.
   - **In:** Input files (space-separated, no spaces within filenames).
   - **Out:** Output files (same format as `In`).

🚨 **Common Mistake:** Forgetting to list output files in `In:` → Causes unexpected file rewrites.

### **Step 2: Run AIDDA inside Vim**
1. Start the AIDDA menu: `\v`
2. Generate or regenerate code: `g (generate)` or `r (regenerate)`

### **Step 3: Reviewing & Editing Code**
1. Show the diff view: `\dv`
2. Select a file from the left-hand file list.
3. Toggle file list visibility: `\b`
4. Expand/collapse code folds inside the file: `zi`
5. To **revert** changes in a file: Select file → Hit `X`
6. Close the diff view: `\dc`

### **Step 4: Commit Changes**
1. Open the AIDDA menu: `\v`
2. Commit changes: `c (commit)` or `f (force-commit)`
3. If prompted about an updated prompt file:
   - Review the changes (`\dv`).
   - Undo (`u`) and redo (`R`) if necessary.
   - Manually commit using Git if needed: `git commit -m "Your message"`

---

## 3. Debugging & Optimization
- **File rewrites too extreme?** Ensure all `Out:` files are also in `In:`. Use `X` to revert and re-run.
- **Workflow issues?** Consider replacing `.stamp` files with **Git timestamps**. Cache prompt file content for better commit consistency.

---

## 4. Future Plans
- Transitioning from shell script (`aidda.sh`) to Go (`aidda.go`).
- Potential support for non-Go languages.

---

This cheat sheet now includes a detailed walkthrough for using AIDDA **inside Vim** while programming in Go.
