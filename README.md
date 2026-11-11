# Phone

The Arche phone's dialer, written in [Arche](https://github.com/arche-family/arche-lang): a WebAssembly component of the Arche phone's world, built into the phone as one of its default apps and updated from its repository. That world is Nimadir's mini-app world and what a phone adds to it; its WIT is in `wit/` (from arche-os's `phone/wit`), and `world` names it.

It works like Android's, in three tabs:

- Keypad: the number typed, `+` for a country's code, `⌫` for a key less, and Call. The phone's call screen takes the call from there, as it shows one that comes.
- Recents: the calls the phone had, the latest first, each by its contact's name or its number, whether it came, was made or was missed, and when. A tap calls it back.
- Contacts: the phone's contacts; a tap calls one.

`manifest` says what it may do: make calls and read the calls the phone had (`telephony`), and read the contacts (`contacts`).

## Build

With [arche-lang](https://github.com/arche-family/arche-lang) checked out next to this repository and built (`make -C stage0 && make -C compiler && make -C wasm`):

```sh
../arche-lang/wasm/nimadir-app.sh . phone.component.wasm
```

## Test

`headless` holds a headless run's input (`+`, `1` and Call on the keypad, then the other tabs) and the functions the phone's world adds, and `expected.txt` what arche-wasm's interpreter said to them, as Nimadir's shell does not run the phone's world. `app-check.sh` runs the app with arche-wasm, by the interpreter and by the JIT, and compares what it says with `expected.txt`; CI runs it on every push. Calls on the phone itself, made, answered and missed, through its modem, are tried by arche-os's `tests/phone.sh`.

```sh
../arche-lang/wasm/app-check.sh .
NIMADIR=~/nimadir ../arche-lang/wasm/app-expected.sh .   # expected.txt again, from arche-wasm
```
