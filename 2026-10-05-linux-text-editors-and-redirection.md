# Linux CLI: Text Editors, Plain Text Configuration & Redirection 

**Date:** 2026-10-05


Today I learned why text editing is the most central, everyday habit in Linux system administration. I explored the critical difference between plain text and formatted text, the hierarchy of available Linux editors, and how to create files directly from the command line without opening an editor at all.

## 1. The Importance of Plain Text 
In Linux, much of the system is configured through plain-text files. The `/etc` directory is full of readable, editable configuration files. Whether you are adjusting a service, writing an automation script, or modifying source code, you are working with plain text.

**Word Processors vs. Text Editors:**
*   **Word Processors:** Applications like a typical office suite add invisible formatting behind the scenes (fonts, margins, styles, layout information). If you edit a system config file in a word processor, that hidden formatting comes along for the ride and will render the file completely unusable.
*   **Text Editors:** Applications that deal strictly in raw characters (similar to Notepad or TextEdit in plain-text mode). A text editor saves exactly the characters you typed and absolutely nothing more, which is mandatory for Linux configuration.

## 2. Types of Linux Text Editors 
Linux text editors are broadly categorized into two main branches: Basic Editors and Advanced Editors[cite: 9].

### Basic Editors[cite: 9]
*   **nano:** Categorized as a basic editor[cite: 9]. It is a straightforward editor that runs right in the terminal. It shows its main commands along the bottom of the screen, meaning there is almost nothing to memorize. It is the ideal first editor.
*   **graphical editors:** Categorized as basic editors[cite: 9]. On the Ubuntu desktop, this is GNOME Text Editor. It is the natural choice when you are in a desktop environment and want a familiar, point-and-click, mouse-driven experience.

### Advanced Editors[cite: 9]
*   **vim:** Categorized as an advanced editor[cite: 9]. It is a classic editor installed on virtually every Linux and UNIX-like system today, without exception. It has a steeper learning curve, but because it is always present, you are likely to land in it unexpectedly, making basic knowledge essential.
*   **emacs:** Categorized as an advanced editor[cite: 9]. Another long-established, highly capable editor with a loyal user base. It is extremely customizable and can do far more than edit text, though it has its own steep learning curve.

### Desktop IDEs
*   **VS Code (run as `code`):** A modern, full-featured editor that runs within a desktop environment rather than a plain terminal. It is really an integrated development environment (IDE) rather than a lightweight editor, but it is highly popular.

**The Golden Rule of Thumb:** Reach for `nano` when you're in the terminal, and a graphical editor like GNOME Text Editor when you're on the desktop. 

## 3. Creating Files Without an Editor (CLI Redirection) 
For a quick file with just a few lines, or when writing scripts that need to generate files automatically on the fly, you can create files directly from the command line using two standard methods.

### Method 1: Using `echo` with Redirection
The `echo` command prints text, which you can redirect directly into a file to build it line by line.

```bash
$ echo line one > myfile
$ echo line two >> myfile
$ echo line three >> myfile
```

**The Redirection Symbols:**

* `>` (Single bracket): Redirects the output into a file and overwrites anything already in that file.
* `>>` (Double bracket): Appends the output to the end of an existing file, adding to it rather than replacing it.

#### 🚨 A Word of Caution about `>`:
The `>` symbol overwrites instantly, with no confirmation and no undo. It is incredibly easy to accidentally type `>` when you meant `>>` and permanently wipe out a file. This is highly dangerous when running commands with `sudo` (e.g., `sudo echo ... > /etc/somefile`), as it can overwrite important configuration with no way back.

### Method 2: Using `cat` with a "Here Document"
This method uses the `cat` command alongside a marker that tells the shell exactly where your text begins and ends.
```Bash
$ cat << EOF > myfile
> line one
> line two
> line three
> EOF
$
```

* **How it works:** You type your content, then finish with the marker word on its own line.
* **The Marker:** The marker does not have to be `EOF`; it can be any word that doesn't appear in your text (like `STOP`), but `EOF` (End of File) is the traditional industry choice.
* **The Prompt:** The leading `>` on the middle lines is just the shell's continuation prompt appearing automatically; you do not type it manually.

*Result:* Both Method 1 and Method 2 produce the exact same result: a file named `myfile` containing exactly three lines of text.