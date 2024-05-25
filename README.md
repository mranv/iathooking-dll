# WinAPI Hooking Library

This project demonstrates the use of the Windows API to perform function hooking, process memory reading/writing, and parsing of PE (Portable Executable) headers to locate and manipulate import and export tables.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Dependencies](#dependencies)
- [Setup](#setup)
- [Usage](#usage)
- [Functions](#functions)
- [Structures](#structures)
- [Example](#example)

## Overview

This library provides functionality for hooking Windows API functions, reading and writing process memory, and parsing PE headers to locate import and export functions. It demonstrates how to hook the `NtQuerySystemInformation` function and modify its behavior.

## Features

- Read strings from process memory.
- Hook a function and redirect it to a custom implementation.
- Parse PE headers to retrieve import and export functions.
- Manipulate import and export tables.

## Dependencies

- [winapi](https://docs.rs/winapi/0.3.9/winapi/)
- [ntapi](https://docs.rs/ntapi/0.3.6/ntapi/)

## Setup

Add the following dependencies to your `Cargo.toml` file:

```toml
[dependencies]
winapi = "0.3.9"
ntapi = "0.3.6"
```

## Usage

### Read String from Memory

This function reads a string from a specified memory location in a process.

```rust
pub fn ReadStringFromMemory(prochandle: *mut c_void, base: *const c_void) -> String
```

### Hook a Function

This function demonstrates hooking the `NtQuerySystemInformation` function and modifying its behavior.

```rust
#[no_mangle]
pub unsafe extern "C" fn test2(
    sysinfo: SYSTEM_INFORMATION_CLASS,
    baseaddress: *mut c_void,
    infolength: u32,
    outinfolength: *mut u32
) -> i32
```

### Parse Exports

This function parses the export table of a 64-bit PE image.

```rust
pub fn ParseExports64(prochandle: *mut c_void, baseaddress: *mut c_void) -> HashMap<String, i32>
```

### Parse Imports

This function parses the import table of a 64-bit PE image.

```rust
pub fn ParseImports64(prochandle: *mut c_void, baseaddress: *mut c_void) -> HashMap<String, Vec<HashMap<String, HashMap<String, i64>>>>
```

### Fill Structure from Array

This function copies data from an array to a structure.

```rust
pub fn FillStructureFromArray<T, U>(base: &mut T, arr: &[U]) -> usize
```

### Fill Structure from Memory

This function reads data from memory and fills a structure with it.

```rust
pub fn FillStructureFromMemory<T>(
    dest: &mut T,
    src: *const c_void,
    prochandle: *mut c_void,
) -> usize
```

## Functions

### `ReadStringFromMemory`

Reads a null-terminated string from a specific memory address in a process.

### `messageboxclone`

A hooked version of `MessageBoxA` that modifies the displayed message if it matches "hello world".

### `test2`

A hooked version of `NtQuerySystemInformation` that optionally modifies its behavior.

### `FillStructureFromArray`

Copies data from an array to a structure.

### `FillStructureFromMemory`

Copies data from memory to a structure.

### `ParseExports64`

Parses the export table of a 64-bit PE image.

### `ParseImports64`

Parses the import table of a 64-bit PE image.

### `DllMain`

The entry point for the DLL, responsible for setting up the hook.

## Structures

### `IMAGE_SECTION_HEADER`

Represents a section header in the PE file.

### `IMAGE_IMPORT_DESCRIPTOR`

Represents an import descriptor in the PE file.

### `IMAGE_EXPORT_DIRECTORY`

Represents an export directory in the PE file.

### `IMAGE_OPTIONAL_HEADER64`

Represents the optional header in a 64-bit PE file.

### `IMAGE_FILE_HEADER`

Represents the file header in a PE file.

### `IMAGE_DATA_DIRECTORY`

Represents a data directory in the PE file.

### `IMAGE_NT_HEADERS32`

Represents the NT headers in a 32-bit PE file.

### `IMAGE_NT_HEADERS64`

Represents the NT headers in a 64-bit PE file.

### `IMAGE_DOS_HEADER`

Represents the DOS header in a PE file.

## Example

The following example demonstrates how to hook the `NtQuerySystemInformation` function and modify its behavior.

```rust
#[no_mangle]
pub unsafe extern "stdcall" fn DllMain(
    handle: HINSTANCE,
    reason: u32,
    reserved: *mut c_void
) -> u32 {
    if reason == 1 {
        // Code to set up the hook
    }
    1
}
```

In this example, `DllMain` sets up the hook by modifying the import table to redirect calls to `NtQuerySystemInformation` to a custom function `test2`.

For a complete example and more details, refer to the source code provided in this repository.

## License

This project is licensed under the MIT License. See the LICENSE file for details.
