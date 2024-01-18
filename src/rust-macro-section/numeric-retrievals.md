# Getting or combining subslices of bytes

Some of the macros defined in the Win32 SDK are used to take smaller slices of bytes from larger ones. For example `LOWORD` and `HIWORD` to get the most and least significant `WORD`s from 32-bit integer types serving as packed data. Helpers like these are luckily rather trivial to implement and only require some bitmasking.

## Getting window `LPARAM` values

```rs
/// Get the low order word as a signed 32-bit integer.
#[inline(always)]
pub fn get_x_lparam(lp: windows::Win32::Foundation::LPARAM) -> i32 {
    lp.0 as i32
}

/// Get the high order word as a signed 32-bit integer.
#[inline(always)]
pub fn get_y_lparam(lp: windows::Win32::Foundation::LPARAM) -> i32 {
    // the sign bit is duplicated for right shifts on signed ints
    (lp.0 >> 16) as i32
}
```

## Getting high- and low-order values

Luckily the `DWORD` type in the C++ headers is defined as an unsigned 32-bit integer, and the `WORD` is just an unsigned 16-bit integer. This means we can do some cheap optimizations here without breaking correctness guarantees. The same applies to the `BYTE` type (unsigned char) which is just an `u8` in Rust!
Additionally, some functions require two `DWORD` arguments for high and low order bytes. These are internally reconstrcuted into a single unsigned 64-bit integer. So these utilities will also include a function for splitting a single `u64` for convenience.

```rs
/// Get the low order word as u16
#[inline(always)]
pub fn loword(dw: u32) -> u16 {
    dw as u16 // truncate the first 16 bits
}

/// Get the high order word as u16
#[inline(always)]
pub fn hiword(dw: u32) -> u16 {
    (dw >> 16) as u16 // truncate the zero-padding
}

/// Get the low order byte from a word (u16)
#[inline(always)]
pub fn lobyte(word: u16) -> u8 {
    word as u8
}

/// Get the high order byte from a word (u16)
#[inline(always)]
pub fn hibyte(word: u16) -> u8 {
    (word >> 8) as u8
}

/// Get the low order double word from a u64
#[inline(always)]
pub fn lodword(longlong: u64) -> u32 {
    longlong as u32
}

/// Get the high order double word from a u64
#[inline(always)]
pub fn hidword(longlong: u64) -> u32 {
    (longlong >> 32) as u32
}
```

Likewise, the headers contain some macros from converting smaller units back into their doubly lengthy counterparts, used in e.g. packing values into an `LPARAM`. For consistency's sake and because CsWin32 also implements several of them, they'll be provided here as well. Feel free to inform us of cases where these are useful!

```rs
/// Turn two bytes into a word
#[inline(always)]
pub fn makeword(low: u8, high: u8) -> u16 {
    (low as u16) | ((high as u16) << 8)
}

/// Turn two words into a dword/long
/// docs are somewhat weird, the macro is named "MAKELONG" but returns a DWORD?
pub fn makelong(low: u16, high: u16) -> u32 {
    (low as u32) | ((high as u32) << 16)
}

/// Not part of the standard macros, but a logical step to accommodate 64-bit
/// Named after the underlying C datatype, in Win32 terms this would be a QWORD
pub fn makelonglong(low: u32, high: u32) -> u64 {
    (low as u64) | ((high as u64) << 32)
}
```
