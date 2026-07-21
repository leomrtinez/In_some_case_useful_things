# In_some_case_useful_things

A personal collection of configs, snippets, scripts, and development utilities accumulated over time. From Vim tweaks and VS Code settings to shell aliases and random productivity hacks, this repository is my digital toolbox.

## Table of contents

- [VimRC](#vimrc)
- [VS Code – Python script header snippet](#vs-code--python-script-header-snippet)

---

## VimRC

Clone the repo, then move `vimrc` into your Vim config folder:

```bash
git clone <this-repo-url>
cd In_some_case_useful_things
mkdir -p ~/.vim
cp vimrc ~/.vim/vimrc
```

Vim itself only reads `~/.vimrc` (or `~/.config/nvim/init.vim` for Neovim) by default, so you need to tell it to load the file you just copied. Add this single line to `~/.vimrc` (create it if it doesn't exist):

```vim
source ~/.vim/vimrc
```

Then:

1. Open Vim and run `:PlugInstall` to install all plugins.
   - This requires [vim-plug](https://github.com/junegunn/vim-plug) to already be installed (`autoload/plug.vim` in your Vim runtime path). If you don't have it yet, install it first — otherwise `:PlugInstall` won't be recognized:
    ```bash
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    ```

2. To apply changes without restarting Vim, run `:source ~/.vimrc` (or `:source %` if the file you edited is the one currently open).
3. Otherwise, just reopen Vim and the config loads automatically.

**Quick checklist:**
- [ ] `vim-plug` installed
- [ ] `~/.vim/vimrc` in place
- [ ] `~/.vimrc` sources it
- [ ] `:PlugInstall` run at least once

---

## VS Code – Python script header snippet

A user snippet that inserts a standard header (docstring with author/description/date + common imports and Matplotlib config) at the top of any new Python script.

### Setup

1. In VS Code, open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`).
2. Search for **"Configure User Snippets"** and select **python.json**.
3. Paste the snippet below into the file, replacing `TonNom` with your actual name.

```json
{
  "Python Header": {
    "prefix": "pyhead",
    "body": [
      "\"\"\"",
      "Description: ${1:Script description}",
      "",
      "Author: ${2:Your Name}",
      "Date: $CURRENT_DATE-$CURRENT_MONTH-$CURRENT_YEAR",
      "Contact: ${3:Email}",
      "",
      "\"\"\"",
      "",
      "import matplotlib.pyplot as plt",
      "import pandas as pd",
      "import numpy as np",
      "",
      "import pdb",
      "",
      "# Config Matplotlib",
      "plt.style.use('dark_background')",
      "",
      "plt.rcParams.update({",
      "    \"pgf.texsystem\": \"pdflatex\",  # use pdflatex",
      "    \"text.usetex\": True,          # use LaTeX to write all text",
      "    \"font.family\": \"serif\",       # use serif/main font for text elements",
      "    \"font.serif\": [],             # blank entries should cause plots to inherit fonts from the document",
      "    \"axes.labelsize\": 12,         # LaTeX default is 10pt font.",
      "    \"font.size\": 12,",
      "    \"legend.fontsize\": 12,         # Make the legend/label fonts a little smaller",
      "    \"xtick.labelsize\": 12,",
      "    \"ytick.labelsize\": 12,",
      "    \"axes.titlesize\": 12",
      "})",
      "",
      "$0"
    ],
    "description": "En-tête standard pour mes scripts Python"
}
```

### Usage

1. In a new `.py` file, type `pyhead` then press `Tab`.
2. The full header inserts, with the date already filled in.
3. Your name is pre-filled and highlighted — press `Tab` again to jump to the description field, type it, then `Tab` once more.
4. The cursor lands right below the header, ready for your code.

**Note:** `text.usetex: True` requires a working LaTeX installation (e.g. `pdflatex`) on the machine. If you move this to a new environment and get rendering errors, that line is usually the first thing to comment out.