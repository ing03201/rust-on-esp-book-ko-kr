# 문제 해결

이 장에서는 우리가 시간이 지남에 따라 직면한 특정 질문과 일반적인 문제, 그리고 그들의 해결책과 함께 나열합니다. 이 페이지는 선택된 ESP 생태계와 독립적인 일반적인 문제를 수집합니다. 여기에 나열된 문제를 찾을 수 없다면, 적절한 저장소에서 문제를 열거나 [Matrix room][matrix]에서 문의하십시오.

[matrix]: https://matrix.to/#/#esp-rs:matrix.org

## 잘못된 Rust Toolchain 사용

```text
$ cargo build
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names --target xtensa-esp32-espidf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit status: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-espidf". Run `rustc --print target-list` for a list of built-in targets
```

이전 오류나 유사한 오류가 발생한다면, 아마도 적절한 Rust 툴체인을 사용하지 않을 것입니다. `Xtensa` 대상의 경우, Espressif Rust 포크 툴체인을 사용해야 한다는 것을 기억하세요. 몇 가지 방법이 있습니다:

- 커맨드 라인에서 툴체인 단축키 재정의: `cargo +esp`.

- `RUSTUP_TOOLCHAIN`  환경 변수를 `esp`로 설정하세요.

- [디렉토리 재정의][directory-override] 설정: `rustup 재정의 세트 esp`
  
- 프로젝트에 [`rust-toolchain.toml`][rust-toolchain-toml] 파일을 추가하세요:
  
  ```toml
  [toolchain]
  channel = "esp"
  ```
  
- `esp`를 [디폴트 툴체인][default-toolchain]으로 세팅한다.

툴체인 재정의에 대한 자세한 내용은 The rustup book의 [재정의][overrides-rust-book] 장을 참조하십시오.

[toolchain-override]: https://rust-lang.github.io/rustup/overrides.html#toolchain-override-shorthand
[directory-override]: https://rust-lang.github.io/rustup/overrides.html#directory-overrides
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[default-toolchain]: https://rust-lang.github.io/rustup/overrides.html#default-toolchain
[overrides-rust-book]: https://rust-lang.github.io/rustup/overrides.html#overrides

## Windows

### 긴 Path 네임

Windows를 사용할 때, 긴 경로 이름을 사용하는 경우 새 프로젝트를 구축하는 데 문제가 발생할 수 있습니다. 게다가 - 그리고 `std` 애플리케이션을 빌드하려고 하는 경우 - 프로젝트 경로가 ~ 10자보다 길면 빌드가 어려운 오류와 함께 실패합니다.

문제를 해결하려면 프로젝트 이름을 줄이고 드라이브 루트로 옮겨야 합니다. `C:\myproj`. 또한 Windows `subst` 유틸리티(예: `subst r: <pathToYourProject>`)를 사용하는 동안 프로젝트 위치를 그대로 유지하면서 빌드 중에 짧은 경로를 사용하는 쉬운 솔루션처럼 보일 수 있지만, 짧고 대체된 경로가 Windows API에 의해 실제 (긴) 위치로 확장되기 때문에 *작동하지 않습니다*.

또 다른 대안은 Linux용 Windows 하위 시스템(WSL)을 설치하고, 네이티브 Linux 파일 파티션 내부로 프로젝트를 이동하고, WSL 내부에 빌드하고, WSL 외부에서 컴파일된 MCU ELF 파일만 플래시하는 것입니다.

### Missing ABI

```powershell
  Compiling cc v1.0.69
error: linker `link.exe` not found
  |
  = note: The system cannot find the file specified. (os error 2)

note: the msvc targets depend on the msvc linker but `link.exe` was not found

note: please ensure that VS 2013, VS 2015, VS 2017 or VS 2019 was installed with the Visual C++ option

error: could not compile `compiler_builtins` due to previous error
warning: build failed, waiting for other jobs to finish...
error: build failed
```

이 오류의 이유는 MSVC C++가 누락되어 [컴파일 시간 요구][Compile-time Requirements] 사항을 충족하지 못하기 때문입니다. [Visual Studio 2013(또는 그 이상) 또는 Visual C++ Build Tools 2019][ Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]를 설치하십시오. 비주얼 스튜디오의 경우, "C++ 도구"와 "윈도우 10 SDK" 옵션을 확인하세요. GNU ABI를 사용하는 경우, [MinGW/MSYS2 툴체인][MinGW/MSYS2 toolchain]을 설치하세요.

[Compile-time Requirements]: https://github.com/rust-lang/cc-rs#compile-time-requirements
[Visual Studio 2013 (or later) or the Visual C++ Build Tools 2019]: https://rust-lang.github.io/rustup/installation/windows.html
[MinGW/MSYS2 toolchain]: https://www.msys2.org/
