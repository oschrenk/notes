# Ledger
```
brew install hledger

brew install ghc
stack config set system-ghc --global true
stack install --local-bin-path /usr/local/bin hledger-web-1.1
```

## Sync with ABN Amro

```
pip install ledger-autosync
```

*Download transactions*
https://github.com/msturm/abncsv2qif

*Convert*

*Sync*
```
ledger-autosync download.ofx
```