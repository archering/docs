---
title: wasm-bindgen 导入 导出
updated: 2022-10-02 14:11:11Z
created: 2022-10-02 14:10:49Z
latitude: 34.34157500
longitude: 108.93977000
altitude: 0.0000
---

Import JavaScript things into Rust and export Rust things to JavaScript.

```
use wasm_bindgen::prelude::*;

// Import the `window.alert` function from the Web.
#[wasm_bindgen]
extern "C" {
    fn alert(s: &str);
}

// Export a `greet` function from Rust to JavaScript, that alerts a
// hello message.
#[wasm_bindgen]
pub fn greet(name: &str) {
    alert(&format!("Hello, {}!", name));
}
```