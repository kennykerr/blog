# Macro and Function snippets for Rust

With [windows-rs](https://github.com/microsoft/windows-rs) gaining popularity and use, some of the macros and header-only functions from the C++ SDK have become [requested (windows-rs#921)](https://github.com/microsoft/windows-rs/issues/921) [recurringly (windows-rs#2273)](https://github.com/microsoft/windows-rs/issues/2273). Eventually discussion picked up about [creating a home (windows-rs#2798)](https://github.com/microsoft/windows-rs/issues/2798) for either macro-equivalent crates, or a collection of reusable snippets for common operations. Due to there being no definitive list of all macros, and which ones are even being actively missed when using the Rust crates, it was decided to first create and curate this list here before deciding on more definitive solutions.

Macros and functions known to be missing that have already been requested will be tracked in this section, and the snippets will be grouped in chapters. If there are other utilities you feel are missing, feel free to propose adding them either to these snippets, to existing examples, or to a crate that can help extend `windows-rs`!
- `GET_X_LPARAM` (#921)
- `GET_Y_LPARAM` (#921)
- `HIBYTE` (#921)
- `HIWORD` (#921)
- `LOBYTE` (#921)
- `LOWORD` (#921)
- `MAKELONG` (#921)
- `MAKEWORD` (#921)
- `InitPropVariant*` helpers (#976)
- `SendMessage` macros (#921)
- The complete set of WMI (MI_xxx) functions (#1572)
- And apparently many more, to the point [people joked about `windows-rsEx` crates](https://github.com/microsoft/windows-rs/issues/1984#issuecomment-1226790825) ...

The other pages in this section is a (still growing) collection of snippets implementing some of these macros. As time goed by, it might become worth investing time in spinning up dedicated crates for these, but we're not yet at that point and it would be great to see the community pick up and maintain collections providing these as well.
