# `esp-idf-hal` based projects

## Environment Variable `LIBCLANG_PATH` Not Set

```text
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```

We need `libclang` for [`bindgen`] to generate the Rust bindings to the ESP-IDF C headers.
Make sure you have sourced the export file generated by `espup`, see [Set up the environment variables][set-up-the-environment-variables].

[`bindgen`]: https://github.com/rust-lang/rust-bindgen
[set-up-the-environment-variables]: ./../installation/riscv-and-xtensa.md#3-set-up-the-environment-variables

## Missing `ldproxy`

```shell
error: linker `ldproxy` not found
  |
  = note: No such file or directory (os error 2)
```

If you are trying to build a `std` application [`ldproxy`][ldproxy] must be installed. See [`std` Development Requirements][rust-esp-book-std-requirements]

```shell
cargo install ldproxy
```

[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[rust-esp-book-std-requirements]: ./../installation/std-requirements.md

## `sdkconfig.defaults` File is Updated but it Doesn't Appear to Have Had Any Effect

You must clean your project and rebuild for changes in the `sdkconfig.defaults` to take effect:

```shell
cargo clean
cargo build
```

## The Documentation for the Crates Mentioned on This Page is out of Date or Missing

Due to the [resource limits] imposed by [docs.rs], internet access is blocked while building documentation. For this reason, we are unable to build the documentation for `esp-idf-sys` or any crate depending on it.

Instead, we are building the documentation and hosting it ourselves on GitHub Pages:

- [`esp-idf-hal` Documentation]
- [`esp-idf-svc` Documentation]
- [`esp-idf-sys` Documentation]

[resource limits]: https://docs.rs/about/builds#hitting-resource-limits
[docs.rs]: https://docs.rs
[`esp-idf-hal` documentation]: https://esp-rs.github.io/esp-idf-hal/esp_idf_hal/
[`esp-idf-svc` documentation]: https://esp-rs.github.io/esp-idf-svc/esp_idf_svc/
[`esp-idf-sys` documentation]: https://esp-rs.github.io/esp-idf-sys/esp_idf_sys/

## A Stack Overflow in Task `main` has Been Detected

If the second-stage bootloader reports this error, you likely need to increase the stack size for the main task. This can be accomplished by adding the following to the `sdkconfig.defaults` file:

```text
CONFIG_ESP_MAIN_TASK_STACK_SIZE=7000
```

In this example, we are allocating 7 kB for the main task's stack.

## How to Disable Watchdog Timer(s)?

Add to your `sdkconfig.defaults` file:

```text
CONFIG_INT_WDT=n
CONFIG_ESP_TASK_WDT=n
```

Recall that you must clean your project before rebuilding when modifying these configuration files.
