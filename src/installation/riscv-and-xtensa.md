# `RISC-V` 와 `Xtensa` 타겟

[`espup`][espup-github] 는 `Xtensa` 및 `RISC-V` 아키텍처를 위한 Rust 어플리케이션을 개발하는 데 필요한 구성 요소의 설치 및 유지 관리를 단순화하는 도구입니다..

### 1. `espup`설치

 `espup`를 설치하려면, 아래 명령어를 터미널에 실행하세요:
```shell
cargo install espup
```

미리 컴파일된 [릴리스 바이너리][release-binaries]를 직접 다운로드하거나  [`cargo-binstall`][cargo-binstall]을 사용할 수도 있습니다.

[espup-github]: https://github.com/esp-rs/espup
[release-binaries]: https://github.com/esp-rs/espup/releases
[cargo-binstall]: https://github.com/cargo-bins/cargo-binstall

### 2. 필수 툴체인 설치

다음을 실행하여 지원되는 모든 Espressif 대상에 대한 Rust 애플리케이션을 개발하는 데 필요한 모든 도구를 설치하십시오:
```shell
espup install
```

> ⚠️ **참고**: `std` 애플리케이션은 [`std` 개발 요구사항][rust-esp-book-std-requirements]에서 다루는 추가 소프트웨어를 설치해야 합니다.

[rust-esp-book-std-requirements]: ./std-requirements.md

### 3. 환경 변수 설정하기
`espup`은 프로젝트를 구축하는 데 필요한 몇 가지 환경 변수가 포함된 내보내기 파일을 만들 것이다.

Windows (`%USERPROFILE%\export-esp.ps1`)
  - 이 파일을 실행할 **필요가 없습니다**. 수정된 환경변수를 보여주는 용도로 만든것입니다.

유닉스 기반 운영체제 - (`$HOME/export-esp.sh`). 파일을 sourcing하기위한 다른 방법들이 있습니다:
- 매 터미널에서 이 파일을 source하기:
   1. 해당 파일을 source하기: `. $HOME/export-esp.sh`

   이 방식은 매 새 쉘을 열때마ㅏ 실행해줘야합니다.
   
- `export-esp.sh`를 실행하는 alias 만들기:
  
   1. 쉘 프로파일 (`.profile`, `.bashrc`, `.zprofile`, 등.)에 다음 명령어를 복사 붙여넣기 하세요: `alias get_esprs='. $HOME/export-esp.sh'`
   2. 터미널 세션을 재시작하시거나 `source [프로파일 경로]` 명령어를 실행하세요, 예를 들면, `source ~/.bashrc`.
   
   이 방식은 매 쉘마다 sourcing을 안해도됩니다, `export-esp.sh`스크립트는 새로운 쉘이 실행될때 자동으로 실행됩니다.

###  `espup`는 무엇을 설치하나요?

Espressif 대상을 지원하기위해, `espup` 다음과 같은 도구를 설치합니다.

- Espressif 대상 지원을 위한 Espressif Rust fork 
- `RISC-V` 대상을 위한 `nightly` 툴체인
-  `Xtensa`대상을 위한 `LLVM` [fork][llvm-github-fork]
- final binary를 링크하는 [GCC 툴체인][gcc-toolchain-github-fork] 

fork 컴파일러는 표준 Rust 컴파일러와 공존할 수 있으며, 둘 다 시스템에 설치할 수 있습니다. fork 컴파일러는 [오버로딩 메소드][rustup-overrides]를 사용할 때 호출됩니다.

> ⚠️ **참고**: 우리는 fork를 업스트림하기위해 노력을 하고있습니다
> 1.  `LLVM` fork에 변화는 이미 진행중입니다, [tracking issue][llvm-github-fork-upstream issue]를 확인하세요.
> 2. Rust 컴파일러 Fork들에 대해, LLVM 변경이 수락되면, 우리는 Rust 컴파일러 변경을 진행할 것입니다.

오류가 발생하면, [문제 해결][troubleshooting] 장을 확인하세요.

[llvm-github-fork]: https://github.com/espressif/llvm-project
[gcc-toolchain-github-fork]: https://github.com/espressif/crosstool-NG/
[rustup-overrides]: https://rust-lang.github.io/rustup/overrides.html
[llvm-github-fork-upstream issue]: https://github.com/espressif/llvm-project/issues/4
[troubleshooting]: ../troubleshooting/index.md

### `Xtensa` Targets를 위한 다른 설치 방법

- [`rust-build`][rust-build] 설치 스크립트 사용. 이것은 과거에 권장되는 방법이었지만, 이제 설치 스크립트는 기능이 frozen되었다. 모든 새로운 기능은  `espup`에만 포함될 것입니다. 원본 Repository [README][https://github.com/esp-rs/rust-build.git]를 참조하십시오.
- 소스에서 `Xtensa`  Rust 컴파일러를 구축하세요. 이 과정은 계산 비용이 많이 들고 시스템에 따라 완료하는 데 한 시간 이상이 걸릴 수 있습니다. 이 접근 방식을 취할 주요 이유가 없다면 권장되지 않습니다. 여기 소스에서 빌드할 리포지토리가 있습니다: [`esp-rs/rust` repository][esp-rs-rust].

[rust-build]: https://github.com/esp-rs/rust-build#download-installer-in-bash
[esp-rs-rust]: https://github.com/esp-rs/rust
