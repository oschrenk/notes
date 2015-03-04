# Powerline #

- [powerline](https://github.com/powerline/powerline)

## Patching fonts

Install fontforge with python bindings:

    brew install fontforge --with-python

Install powerline-fontpatcher:

    mkdir -p ~/.local/src
    git clone git@github.com:powerline/fontpatcher.git ~/.local/src/powerline-fontpatcher
    cd ~/.local/src/powerline-fontpatcher
    python setup.py install

Download, patch & install font:

    mkdir -p ~/.local/src/fonts
    git clone https://github.com/oschrenk/mplus-outline-fonts ~/.local/src/fonts/mplus-outline-fonts l
    cd ~/Downloads
    find ~/.local/src/fonts/mplus-outline-fonts/ -name "mplus-[12]m*" -exec scripts/powerline-fontpatcher --source-font=~//Users/oliver/.local/src/powerline-fontpatcher/fonts/powerline-symbols.sfd '{}' \;

