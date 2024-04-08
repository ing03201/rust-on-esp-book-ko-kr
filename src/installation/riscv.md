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

   커스텀 타겟에 대한 자세한 내용은 [Embedonomicon][embedonomicon-official-book]의 이 [챕터][embedonomicon-creating-a-custom-target]를 읽으세요

2. 타겟을 설정합니다:
    - `no_std` (베어 메탈) 어플리케이션의 경우 다음을 실행합니다:

      ```shell
      rustup target add riscv32imc-unknown-none-elf # For ESP32-C2 and ESP32-C3
      rustup target add riscv32imac-unknown-none-elf # For ESP32-C6 and ESP32-H2
      ```

      이 대상은 현재 [Tier 2][rust-lang-book--platform-support-tier2]입니다. Rust에서 다양한 [`RISC-V` extensions][wiki-riscv-standard-extensions]을 포함하는 `riscv32` 대상의 다양한 맛을 주목하십시오.

    -  `std` 어플리케이션의 경우:

      이 대상은 현재 [Tier 3][rust-lang-book--platform-support-tier3]이므로 `rustup`을 통해 배포되는 사전 빌드된 개체가 없으며 `no_std` 대상과 달리 **설치할 필요가 없습니다**. 장치에 대한 올바른 대상은 러스트북의  [*-esp-idf][rust-lang-book--platform-support--esp-idf] 섹션을 참조하십시오.

      - `riscv32imc-esp-espidf` 는 ESP32-C2 및 ESP32-C3 와 같은 atomics을 지원하지않는 SoC 용입니다.
      - `riscv32imac-esp-espidf` 는 atomics를 지원하는 SoC(ESP32-C6, ESP32-H2, and ESP32-P4 등)용입니다.
3.  `std` 프로젝트들을 빌드하려면 다음을 설치하여야합니다:
    - [`LLVM`][llvm-website] 컴파일러 인프라
    - 다른 [`std` 개발 요구사항들][rust-esp-book-std-requirements]
    - 프로젝트의 파일  `.cargo/config.toml` 에서 불안정한 Cargo [feature][cargo-book-unstable-features] `-Z build-std`를 추가합니다.  이 책 뒷부분에서 설명하는 [템플릿 프로젝트][rust-esp-book-write-app-generate-project]에는 이미 이 기능이 포함되어있습니다

이제 Espressif의 `RISC-V` 칩에 프로젝트를 구축하고 실행할 수 있을 것입니다.

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
