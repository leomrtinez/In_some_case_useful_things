 In_some_case_useful_things
 
A personal collection of configs, snippets, scripts, and development utilities accumulated over time. From Vim tweaks and VS Code settings to shell aliases and random productivity hacks, this repository is my digital toolbox — and my quick-reinstall kit whenever I switch machines.
 
## Table of contents
 
- [Repo structure](#repo-structure)
- [Prerequisites](#prerequisites)
- [Homebrew (macOS packages)](#homebrew-macos-packages)
- [Shell (zsh)](#shell-zsh)
- [Git](#git)
- [Vim](#vim)
- [VS Code](#vs-code)
- [Python environment (conda)](#python-environment-conda)
- [SSH config](#ssh-config)

 
## Repo structure
 
```
In_some_case_useful_things/
├── README.md
├── brew/
│   └── Brewfile
├── shell/
│   ├── zshrc
│   └── gitconfig
├── vim/
│   └── vimrc
├── vscode/
│   ├── settings.json
│   ├── python.json
│   └── extensions.txt
├── python/
│   └── environment.yml
└── ssh/
    └── config
```
 

 
## Prerequisites
 
Before going through the sections below, make sure you have:
 
- A terminal open and this repo cloned locally:
```bash
  git clone <this-repo-url>
  cd In_some_case_useful_things
```
- Admin rights on the machine (needed for Homebrew and some installs).
Everything below assumes **macOS**.
 

 
## Homebrew (macOS packages)

### Install Homebrew (if not already installed)
 
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
 
### Restore packages from this repo
 
```bash
brew bundle install --file=brew/Brewfile
```
 
### Update the Brewfile (do this on your current machine before switching)
 
```bash
brew bundle dump --file=brew/Brewfile --force
```
 
Open `brew/Brewfile` afterwards and remove anything too specific to that machine / a one-off project before committing.
 
**Checklist:**
- [ ] Homebrew installed
- [ ] `brew bundle install` run
- [ ] Brewfile reviewed/cleaned before committing


## Shell (zsh)
 
macOS uses zsh by default since Catalina. Check with:
 
```bash
echo $SHELL
```
 
### Restore
 
```bash
cp shell/zshrc ~/.zshrc
```
 
If you use **Oh My Zsh** or a theme/plugin manager, install it *before* copying `.zshrc`, since the file likely references paths like `~/.oh-my-zsh/...`:
 
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
 
Then restart the terminal or run:
 
```bash
source ~/.zshrc
```

### Update (on your current machine)
 
```bash
cp ~/.zshrc shell/zshrc
```
 
**Checklist:**
- [ ] Oh My Zsh (or equivalent) installed first, if used
- [ ] `~/.zshrc` restored
- [ ] Terminal restarted / `source ~/.zshrc` run

 
## Git
 
### Restore
 
```bash
cp shell/gitconfig ~/.gitconfig
```
 
### Update (on your current machine)
 
```bash
cp ~/.gitconfig shell/gitconfig
```
 
⚠️ Before committing, check there's no token or credential in it:
 
```bash
cat ~/.gitconfig
```
 
If there's a `[credential]` section with a token in plain text, strip it out before it goes in the repo.
 

⚠️ **`[safe]` section:** entries under `safe.directory` are absolute paths (e.g. `/Users/tonnom/project`) tied to your current username and machine — they will never be valid as-is on a new machine. Don't copy this section over; rebuild it manually instead:
 
```bash
git config --global --unset-all safe.directory   # clean slate
git config --global --add safe.directory /path/to/active/project
```
 
⚠️ **`[filter "lfs"]` section:** if present, this means [Git LFS](https://git-lfs.github.com/) is used for large files (rasters, datasets, etc.) and is marked `required = true` — meaning Git will refuse to work with LFS-tracked files if `git-lfs` isn't installed first. Make sure `git-lfs` is in `brew/Brewfile`, then run once per machine:
 
```bash
brew install git-lfs   # if not already handled by the Brewfile step
git lfs install
```
 
## Vim
 
### Restore
 
```bash
mkdir -p ~/.vim
cp vim/vimrc ~/.vim/vimrc
```
 
Vim only reads `~/.vimrc` by default, so point it to the file above. Add this line to `~/.vimrc` (create it if it doesn't exist):
 
```bash
echo "source ~/.vim/vimrc" >> ~/.vimrc
```
 
Then:
 
1. Install [vim-plug](https://github.com/junegunn/vim-plug) if it isn't already installed (`autoload/plug.vim` in your Vim runtime path) — `:PlugInstall` won't work without it: 
    ```bash
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    ```
2. Open Vim and run `:PlugInstall` to install all plugins.
3. Run `:source ~/.vimrc` (or reopen Vim) to apply changes.
### Update (on your current machine)
 
```bash
cp ~/.vim/vimrc vim/vimrc
```
 
**Checklist:**
- [ ] `vim-plug` installed
- [ ] `~/.vim/vimrc` in place
- [ ] `~/.vimrc` sources it
- [ ] `:PlugInstall` run at least once

 
## VS Code
 
### Settings
 
```bash
cp vscode/settings.json ~/Library/Application\ Support/Code/User/settings.json
```
 
### Python header snippet
 
```bash
mkdir -p ~/Library/Application\ Support/Code/User/snippets
cp vscode/python.json ~/Library/Application\ Support/Code/User/snippets/python.json
```
 
**Usage:** in a new `.py` file, type `pyhead` then `Tab` — inserts a standard header (author/description/date docstring + common imports + Matplotlib config). Date fills in automatically; name and description are editable via `Tab`.
 
### Extensions
 
```bash
cat vscode/extensions.txt | xargs -n 1 code --install-extension
```
 
### Update (on your current machine)
 
```bash
cp ~/Library/Application\ Support/Code/User/settings.json vscode/settings.json
cp ~/Library/Application\ Support/Code/User/snippets/python.json vscode/python.json
code --list-extensions > vscode/extensions.txt
```
 
**Note:** the Python header snippet uses `text.usetex: True`, which requires a working LaTeX installation (e.g. `pdflatex`). If you get rendering errors on a new machine, that's usually the first setting to comment out.
 
 
## Python environment (conda)
 
### Install Miniconda (if not already installed)
 
Check your architecture first:
 
```bash
uname -m
```
 
- `arm64` → Apple Silicon (M1/M2/M3/M4)
- `x86_64` → Intel
Then run:
 
```bash
ARCH=$(uname -m)
if [ "$ARCH" = "arm64" ]; then
    URL="https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh"
else
    URL="https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh"
fi
 
curl -o ~/miniconda.sh "$URL"
bash ~/miniconda.sh -b -p "$HOME/miniconda3"
"$HOME/miniconda3/bin/conda" init zsh
rm ~/miniconda.sh
source ~/.zshrc
conda --version
```
 
(replace `zsh` with `bash` if that's your shell — see the [Shell](#shell-zsh) section above)
 
⚠️ **Ordering matters with the Shell section:** `conda init` appends a block to `~/.zshrc` (between `# >>> conda initialize >>>` and `# <<< conda initialize <<<`) that points to `~/miniconda3`. If you restore `shell/zshrc` from this repo *before* installing Miniconda on the new machine, that block will reference a path that doesn't exist yet. Either:
- install Miniconda **first**, then restore `shell/zshrc` afterward and merge the two conda blocks by hand, or
- restore `shell/zshrc` first, then re-run `conda init zsh` afterward to regenerate the block correctly.
### Restore environment
 
```bash
conda env create -f python/environment.yml
conda activate <env_name>
```
 
### Update (on your current machine)
 
Prefer the `--from-history` export — it only lists packages you explicitly installed (not every sub-dependency), which is more portable across OSes:
 
```bash
conda env export --from-history > python/environment.yml
```
 
Remove any leftover `prefix:` line at the bottom of the file — it's a local path that means nothing on another machine.
 
**Checklist:**
- [ ] Miniconda installed (`conda --version` works)
- [ ] `conda init` block present in `~/.zshrc`, no path mismatch
- [ ] Environment created from `python/environment.yml`
- [ ] `conda activate <env_name>` works
 
## SSH config
 
### Restore
 
```bash
mkdir -p ~/.ssh
cp ssh/config ~/.ssh/config
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
```
 
### Update (on your current machine)
 
```bash
cp ~/.ssh/config ssh/config
```
 
⚠️ **Only copy `config`, never private keys** (`id_rsa`, `id_ed25519`, or any file without a `.pub` extension). If this repo were ever exposed, those keys would grant access to every server they're authorized on.
 
Private keys must be regenerated on the new machine (`ssh-keygen`) and re-added to the relevant services (GitHub, servers, etc.), or restored from a separate secure password manager — never from this repo.
 
**Checklist:**
- [ ] `~/.ssh/config` restored
- [ ] Permissions set (`700` on folder, `600` on config)
- [ ] Keys regenerated / restored from secure storage separately




