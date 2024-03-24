# `esp-idf-template`의 이해

이제 우리는 [`std` 프로젝트를 생성하는 방법][generate-std]을 알았으니, 생성된 프로젝트에 무엇이 포함되어 있는지 검사하고 그것의 모든 부분을 이해하려고 노력합시다.

[generate-std]: ./index.md

## 생성된 프로젝트 점검

다음과 같은 답변으로  [`esp-idf-template`][esp-idf-template]에서 프로젝트를 만들 때:

- 어떤 MCU를 타겟으로 하시나요? · `esp32c3`
- 고급 템플릿 옵션들을 설정하겠습니까? · `false`

이 설명을 위해, 우리는 디폴트값을 사용할 것입니다. 추가 수정을 원한다면, 디폴트 값을 사용하지 않을 때 [additional prompts][prompts]를 참조하십시오.

다음과 같은 파일 구조를 생성해야 합니다:

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .gitignore
├── build.rs
├── Cargo.toml
├── rust-toolchain.toml
└── sdkconfig.defaults
```

더 나아가기 전에, 이 파일들이 무엇을 위한 것인지 봅시다.

- [`.cargo/config.toml`][config-toml]
    - Cargo 구성설정
    - 타겟 포함
    - `runner = "espflash flash --monitor"` -  `cargo run` 으로 코드를 플래시하고 모니터를 할 수 있게합니다.
    - 사용할 링커를 포함합니다. 위 케이스는 [`ldproxy`][ldproxy]를 포함합니다.
    - 활성화된 불안정한 `build-std` Cargo 기능이 포함되어 있습니다.
    - 프로젝트가 사용할 ESP-IDF 버전을 [`esp-idf-sys`][esp-idf-sys]에 알려주는 `ESP-IDF-VERSION` 환경 변수를 포함합니다.
- `src/main.rs`
    - 새로 생성된 프로젝트의 주요 소스 파일
    - 자세한 내용은 아래의 [ `main.rs`의 이해][main-rs] 섹션을 참조하십시오.
- [`.gitignore`][gitignore]
    - 어떤 폴더와 파일을 무시해야 하는지 `git`에게 알려줍니다.
- [`build.rs`][build-rs]
    - `ldproxy`에 대한 링커 인수를 전파합니다.
- [`Cargo.toml`][cargo-toml]
    - 프로젝트의 일부 메타 데이터와 종속성을 선언하는 일반적인 Cargo 매니페스트
- [`rust-toolchain.toml`][rust-toolchain-toml]
    - 사용할 Rust 툴체인을 정의합니다.
      - 툴체인은 당신의 목표에 따라 `nightly` 또는 `esp` 될 것입니다.
- [`sdkconfig.defaults`][sdkconfig-defaults]
    - ESP-IDF 기본값에서 재정의된 값을 포함합니다.

[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[prompts]: https://github.com/esp-rs/esp-idf-template#generate-the-project
[main-rs]:#mainrs의-이해
[config-toml]: https://doc.rust-lang.org/cargo/reference/config.html
[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys]: https://github.com/esp-rs/esp-idf-sys
[gitignore]: https://git-scm.com/docs/gitignore
[build-rs]: https://doc.rust-lang.org/cargo/reference/build-scripts.html
[cargo-toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file
[sdkconfig-defaults]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/build-system.html#custom-sdkconfig-defaults

###  `main.rs`의 이해

```rust,ignore
1 use esp_idf_sys as _; // If using the `binstart` feature of `esp-idf-sys`, always keep this module imported
2
3 fn main() {
4     // It is necessary to call this function once. Otherwise some patches to the runtime
5     // implemented by esp-idf-sys might not link properly. See https://github.com/esp-rs/esp-idf-template/issues/71
6     esp_idf_sys::link_patches();
7     println!("Hello, world!");
8 }
```

첫 번째 줄은 루트 크레이트가 주 함수를 정의하는 바이너리 상자일 때 ESP-IDF 진입점이 정의된 것을 가져오기입니다.

그런 다음, 우리는 몇 줄이 있는 일반적인 주요 기능을 가지고 있습니다:

- Rust에서 구현된 ESP-IDF에 대한 몇 가지 패치가 최종 실행 파일과 연결되도록 하는 `esp_idf_sys::link_patches` 함수에 대한 호출

  우리는 콘솔에 유명한 "Hello, world!"를 인쇄합니다.

## 코드 실행하기

코드를 빌드하고 실행하는 것은 어렵지 않습니다.

```shell
cargo run
```

이렇게 하면 configuration에 따라 코드가 빌드되고 [`espflash`](https://github.com/esp-rs/espflash/tree/main/espflash)를 실행하여 코드를 보드에 플래시합니다.

우리의 [`runner` configuration](https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner) 은 [`espflash`](https://github.com/esp-rs/espflash/tree/main/espflash)에 `--monitor` 인수도 있기때문에, 우리는 코드출력을 볼수있습니다.

[`espflash`](https://github.com/esp-rs/espflash/tree/main/espflash) 를 설치했는지 확인하세요,그렇지 않으면 이 단계가 fail 하게됩니다. [`espflash`](https://github.com/esp-rs/espflash/tree/main/espflash) 설치방법은 다음과 같습니다.: `cargo install espflash`

이와 유사한 내용을 확인해야 합니다:

```text
[2023-04-18T08:05:09Z INFO ] Connecting...
[2023-04-18T08:05:10Z INFO ] Using flash stub
[2023-04-18T08:05:10Z WARN ] Setting baud rate higher than 115,200 can cause issues
Chip type:         esp32c3 (revision v0.3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi, BLE
MAC address:       60:55:f9:c0:39:7c
App/part. size:    478,416/4,128,768 bytes, 11.59%
[00:00:00] [========================================]      13/13      0x0
[00:00:00] [========================================]       1/1       0x8000
[00:00:04] [========================================]     227/227     0x10000
[2023-04-18T08:05:15Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

...
I (344) cpu_start: Starting scheduler.
Hello, world!
```

여기에 표시되는 것은 1단계 및 2단계 부트로더의 메시지와 "Hello world" 메시지입니다!

이것은 실제 코드가 동작하는 것입니다

재부팅 단축키는 `CTRL+R` , exit 단축키는 `CTRL+C` 입니다.

프로젝트를 빌드하는 동안 문제가 발생하면 [문제 해결](https://ing03201.github.io/rust-on-esp-book-ko-kr/troubleshooting/index.html) 장을 참조하십시오.

[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
[runner-config]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[troubleshooting]: ../../troubleshooting/index.md
