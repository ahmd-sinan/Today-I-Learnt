# Linux CLI: Advanced Text Editors (`emacs`) & Variables 

**Date:** 2026-10-16

While `vi` is the undisputed king of pre-installed ubiquity, `emacs` is the other classic UNIX text editor. It has a massive, loyal community and is essentially an entire computing environment built around a text editor. Today I learned the basics of modeless editing in Emacs and how to configure system-wide default editors.

## 1. The Core Concept: Modeless Editing
Unlike `vi` (which forces you to switch between Command Mode and Insert Mode), **`emacs` is modeless**. You can type text directly into the file at any time, just like a standard word processor. 

To execute commands (like saving, quitting, or deleting lines), you use specific keyboard combinations instead of switching modes. 

**Understanding `emacs` Key Combinations:**
In official `emacs` documentation, shortcuts rely on two primary modifier keys:
1.  **`C-` (Ctrl):** For example, `C-x` means hold the **Ctrl** key and press `x`.
2.  **`M-` (Meta):** This is usually mapped to your **Alt** key (or pressing the `Esc` key followed by the letter).

## 2. Managing Files (Opening, Saving, and Quitting)
To open a file (or create a new one), pass the filename to the editor:
```bash
$ emacs notes.txt
```

| Shortcut | Action | Description |
| :--- | :--- | :--- |
| *(just type)* | **Insert Text** | Directly inserts text into the file buffer. |
| **`C-x C-s`** | **Save** | Holds `Ctrl`, presses `x`, then presses `s` to save the file. |
| **`C-x C-w`** | **Save As** | Writes the file, prompting you for a new filename. |
| **`C-x i`** | **Insert File** | Inserts the contents of a prompted external file at your current cursor position. |
| **`C-x C-c`** | **Quit** | Holds `Ctrl`, presses `x`, then presses `c` to exit (prompts to save if changes were made). |
| **`C-g`** | **Cancel / Abort** | Cancels a half-typed or frozen command (the "get me out of this" button). |

## 3. Movement and Editing Commands
While `emacs` allows standard arrow key navigation, traditional users rely on `Ctrl` combinations to keep their hands on the home row.

**Navigation:**

| Key | Movement Action |
| :--- | :--- |
| **`C-n`** / **`C-p`** | Move to the **n**ext line (down) / **p**revious line (up). |
| **`C-f`** / **`C-b`** | Move **f**orward one character (right) / **b**ackward one character (left). |
| **`C-a`** | Move cursor to the absolute beginning of the line. |
| **`C-e`** | Move cursor to the absolute **e**nd of the line. |
| **`M-f`** / **`M-b`** | Move **f**orward one word / **b**ackward one word. |
| **`M-<`** / **`M->`** | Move to the absolute beginning of the file / absolute end of the file. |

**Text Manipulation:**

| Key | Editing Action |
| :--- | :--- |
| **`C-d`** | **D**eletes the single character at the current cursor position. |
| **`C-k`** | **K**ills (deletes) the rest of the current line. |
| **`C-space`** | Marks the beginning of a selected region of text. |
| **`C-w`** | Deletes the currently marked text and writes it to the buffer (Cut). |
| **`C-y`** | **Y**anks (pastes) the most recently deleted text at the current cursor location. |
| **`C-_`** | Undoes the previous operation. |

**Searching:**

| Key | Search Action |
| :--- | :--- |
| **`C-s`** | **S**earches forward for a specific pattern. |
| **`C-r`** | Searches **r**everse (backward) for a specific pattern. |

## 4. Learning More: Interactive Tutorial
Like `vim`, `emacs` includes a highly regarded interactive built-in tutorial that walks you through basic operations inside the editor itself. 

Once `emacs` is open, launch the tutorial by typing:
**`C-h t`** *(Press `Ctrl + h`, release, then type `t`)*

## 5. Setting Your Default Editor (`$EDITOR` and `$VISUAL`)
Here is something that surprises many newcomers: certain Linux utilities open a text editor automatically on your behalf. For example, when you run `crontab -e` to edit scheduled tasks, or when `git commit` prompts you for a message, the system launches an editor in your terminal. 

**On many enterprise systems, the default choice is `vi`.** If you aren't expecting it, suddenly finding yourself trapped inside `vi` can lead directly to panic.

Which editor these automated commands launch is controlled by two standard environment variables:
*   **`$VISUAL`** — The primary full-screen visual editor preferred by modern programs.
*   **`$EDITOR`** — The legacy/fallback editor variable checked if `$VISUAL` is unset.

### Changing the Default to `nano`
Most tools check `$VISUAL` first and fall back to `$EDITOR`. To set your default editor to `nano` for your current session, run:
```bash
$ export VISUAL=nano
$ export EDITOR=nano
```

To ensure all future terminal sessions permanently use your preferred editor, append those exact two export lines to the absolute bottom of your `~/.bashrc` file.

> **⚠️ SysAdmin Pro Tip:**
> Even after permanently setting your default to `nano`, knowing the emergency `vi` exit sequence (`Esc`, then `:q!`) remains a mandatory skill. When logged into a remote bare-metal server or a new user account where environment variables haven't been configured, system utilities will instantly fall back to `vi`.