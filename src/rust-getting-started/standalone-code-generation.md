# Standalone code generation

Even with a [choice between the windows and windows-sys crates](windows-or-windows-sys.md), some developers may prefer to use completely standalone bindings. The [windows-bindgen](https://crates.io/crates/windows-bindgen) crate lets you generate entirely standalone bindings for Windows APIs with a single function call that you can run from a test to automate the generation of bindings. This can help to reduce your dependencies while continuing to provide a sustainable path forward for any future API requirements you might have, or just to refresh your bindings from time to time to pick up any bug fixes automatically from Microsoft.

Start by adding the following to your Cargo.toml file:

```toml
[dependencies.windows-targets]
version = "0.48"

[dev-dependencies.windows-bindgen]
version = "0.48"
```

The `windows-bindgen` crate is only needed for generating bindings and is thus a dev dependency only. The [windows-targets](https://crates.io/crates/windows-targets) crate is a dependency shared by the `windows` and `windows-sys` crates and only contains import libs for supported targets. This will ensure that you can link against any Windows API functions you may need. 

Write a test to generate bindings as follows:

```rust,no_run
#[test]
fn gen_bindings() {
    let apis = [
        "Windows.Win32.System.SystemInformation.GetTickCount",
    ];

    let bindings = windows_bindgen::standalone(&apis);
    std::fs::write("src/bindings.rs", bindings).unwrap();
}
```

Make use of any Windows APIs as needed.

```rust,no_run,ignore
mod bindings;
use bindings::*;

fn main() {
    unsafe {
        println!("{}", GetTickCount());
    }
}
```