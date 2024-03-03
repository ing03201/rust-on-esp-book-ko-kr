# `RISC-V` 타겟 only

`RISC-V` 구조 기반 Espressif chips의 어플리케이션을 빌드하려면 다음을 따라주세요

1. `rust-src` [component][rustup-book-components]와 함께 [`nightly`][rustup-book-channel-nightly] 툴체인을 설치해주세요:

    ```shell
    rustup toolchain install nightly --component rust-src
    ```

    위의 명령은 rust 소스 코드를 다운로드합니다. `rust-src`는 std-lib, core-lib and build-config와 같은 항목들을 포함합니다.  
    `rust-src`를 다운로드하는것은 두 가지 이유때문에 중요합니다: 
    - **Determinism(결정론)** - core와 std 라이브러리의 내부를 볼 수 있습니다. 높은 수준의 확실성이 필요한 소프트웨어를 작성해야하는 경우, 사용 중인 라이브러리를 확인하는 것이 좋습니다.  
    - **custom targets 빌드하기** - `rustc`는 새 커스텀 타겟의 컴포넌트를 만들때 `rust-src`를 사용한다. 만약 rust에서 지원을 안하는 triple-target을 타겟으로 할때, `rust-src` 다운로드는 필수입니다.

   For more info on custom targets, read this [Chapter][embedonomicon-creating-a-custom-target] from the [Embedonomicon][embedonomicon-official-book].

2. Set the target:
    - For `no_std` (bare-metal) applications, run:

      ```shell
      rustup target add riscv32imc-unknown-none-elf # For ESP32-C2 and ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # For ESP32-C6 and ESP32-H2
      ```

      This target is currently [Tier 2][rust-lang-book--platform-support-tier2]. Note the different flavors of `riscv32` target in Rust covering different [`RISC-V` extensions][wiki-riscv-standard-extensions].

    - For `std` applications:

      Since this target is currently [Tier 3][rust-lang-book--platform-support-tier3], it doesn't have pre-built objects distributed through `rustup` and, unlike the `no_std` target, **nothing needs to be installed**. Refer to the [*-esp-idf][rust-lang-book--platform-support--esp-idf] section of the rustc book for the correct target for your device.

      - `riscv32imc-esp-espidf` for SoCs which don't support atomics, like ESP32-C2 and ESP32-C3
      - `riscv32imac-esp-espidf` for SoCs which support atomics, like ESP32-C6, ESP32-H2, and ESP32-P4
3. To build `std` projects, you also need to install:
    - [`LLVM`][llvm-website] compiler infrastructure
    - Other [`std` development requirements][rust-esp-book-std-requirements]
    - In your project's file `.cargo/config.toml`, add the unstable Cargo [feature][cargo-book-unstable-features] `-Z build-std`. Our [template projects][rust-esp-book-write-app-generate-project] that are discussed later in this book already include this.

Now you should be able to build and run projects on Espressif's `RISC-V` chips.

[rustup-book-channel-nightly]: https://rust-lang.github.io/rustup/concepts/channels.html#working-with-nightly-rust
[rustup-book-components]: https://rust-lang.github.io/rustup/concepts/components.html
[rust-lang-book--platform-support-tier2]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-2
[wiki-riscv-standard-extensions]: https://en.wikichip.org/wiki/risc-v/standard_extensions
[rust-lang-book--platform-support-tier3]: https://doc.rust-lang.org/nightly/rustc/platform-support.html#tier-3
[rust-lang-book--platform-support--esp-idf]: https://doc.rust-lang.org/nightly/rustc/platform-support/esp-idf.html
[llvm-website]: https://llvm.org/
[cargo-book-unstable-features]: https://doc.rust-lang.org/cargo/reference/unstable.html
[rust-esp-book-write-app-generate-project]: ../writing-your-own-application/generate-project/index.md
[rust-esp-book-std-requirements]: ./std-requirements.md
[embedonomicon-creating-a-custom-target]: https://docs.rust-embedded.org/embedonomicon/custom-target.html
[embedonomicon-official-book]: https://docs.rust-embedded.org/embedonomicon/
