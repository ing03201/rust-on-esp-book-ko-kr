# `esp-template`의 이해

이제 [no_std 프로젝트를 생성하는 방법][generate-no-std]을 알았으니, 생성된 프로젝트에 포함된 것을 검사하고, 모든 부분을 이해하고, 실행해 봅시다.

[generate-no-std]: ./index.md

## 생성된 프로젝트 검사하기

다음 질답으로 esp-template에서 프로젝트를 만들 때:

-  어떤 MCU가 타겟인가? · `esp32c3`
- 고급 템플릿 옵션을 구성하시겠습니까? · `false`

이 설명을 위해, 우리는 기본값을 사용할 것입니다. 추가 수정을 원한다면, 기본값을 사용하지 않을 때 [추가 프롬프트들][prompts]을 참조하십시오.

다음과 같은 파일 구조를 생성합니다.:

```text
├── .cargo
│   └── config.toml
├── src
│   └── main.rs
├── .gitignore
├── Cargo.toml
├── LICENSE-APACHE
├── LICENSE-MIT
└── rust-toolchain.toml
```

더 나아가기 전에, 이 파일들이 무엇을 위한 것인지 봅시다.

- [`.cargo/config.toml`][config-toml]
  
    - Cargo 구성설정
    
    - 이것은 프로젝트를 올바르게 구축하기 위한 몇 가지 옵션을 정의합니다.
    
    - `runner = "espflash flash --monitor"` -  `cargo run` 으로 코드를 플래시하고 모니터를 할 수 있게합니다.
    
- `src/main.rs`
    - 새로 생성된 프로젝트의 주요 소스 파일
    - For details, see the [Understanding `main.rs`][main-rs] section below
    - 자세한 내용은 아래의 [`main.rs`이해 섹션][main-rs] 을 참조하십시오.

- [`.gitignore`][gitignore]

    - 어떤 폴더와 파일을 무시해야 하는지 `git`에게 알려줍니다.

- [`Cargo.toml`][cargo-toml]

    - 일반적인 카고 매니페스트는 프로젝트의 일부 메타 데이터와 종속성을 선언합니다.
    
- `LICENSE-APACHE`, `LICENSE_MIT`

    - 그것들은 러스트 생태계에서 사용되는 가장 일반적인 라이선스이다.

      다른 라이선스를 사용하려면, 이 파일을 삭제하고 `Cargo.toml`에서 라이선스를 변경할 수 있습니다.

- [`rust-toolchain.toml`][rust-toolchain-toml]
    
    - 사용할 Rust 툴체인을 정의합니다.
      - 툴체인은 당신의 목표에 따라 `nightly` 또는 `esp`가 될 것입니다.

[esp-template]: https://github.com/esp-rs/esp-template
[prompts]: https://github.com/esp-rs/esp-template#esp-template
[main-rs]: #understanding-mainrs
[cargo-toml]: https://doc.rust-lang.org/cargo/reference/manifest.html
[gitignore]: https://git-scm.com/docs/gitignore
[config-toml]: https://doc.rust-lang.org/cargo/reference/config.html
[rust-toolchain-toml]: https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file

### Understanding `main.rs`

```rust,ignore
 1 #![no_std]
 2 #![no_main]
```

- `#![no_std]`
  - 이것은 러스트 컴파일러에게 이 코드가 `libstd`를 사용하지 않는다는 것을 알려줍니다.
- `#![no_main]`
  - `no_main` 속성은 이 프로그램이 인수를 받는 명령줄 응용 프로그램에 맞는 표준 메인 인터페이스를 사용하지 않을 것이라고 말한다. 표준 메인 대신, 우리는 사용자 지정 진입점을 정의하기 위해 `riscv-rt` 크레이트의 항목 속성을 사용할 것입니다. 이 프로그램에서, 우리는 진입점을 `main`으로 명명했지만, 다른 이름이 사용될 수 있었다. 엔트리포인트 함수는 [발산함수][diverging-function]여야 한다. 즉, `fn foo() -> !;`  이 유형은 함수가 절대 반환되지 않는다는 것을 나타냅니다. 이는 프로그램이 절대 종료되지 않는다는 것을 의미합니다.

```rust,ignore
 4 use esp_backtrace as _;
 5 use esp_println::println;
 6 use hal::{clock::ClockControl, peripherals::Peripherals, prelude::*, timer::TimerGroup, Rtc};
```
- `use esp_backtrace as _;`
  - 우리는 베어메탈 환경에 있기 때문에, 코드에서 패닉이 발생하면 실행되는 패닉 핸들러가 필요합니다.
  - 사용할 수 있는 몇 가지 다른 크레이트(예: `panic-halt`)가 있지만 `esp-backtrace`는 백트레이스의 주소를 프린트하는 구현을 제공합니다.  `espflash`/`espmonitor`와 함께 이러한 주소는 소스 코드 위치로 디코딩될 수 있습니다.
- `use esp_println::println;`
  -  `println!` implementation을 제공합니다
- `use hal:{...}`
  - 몇가지 사용할 것들을 `esp-hal`에서 가져옵니다

```rust,ignore
 8 #[entry]
 9 fn main() -> ! {
10    let peripherals = Peripherals::take();
11    let mut system = peripherals.SYSTEM.split();
12    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();
13
14    // Disable the RTC and TIMG watchdog timers
15    let mut rtc = Rtc::new(peripherals.RTC_CNTL);
16    let timer_group0 = TimerGroup::new(
17        peripherals.TIMG0,
18        &clocks,
19        &mut system.peripheral_clock_control,
20    );
21    let mut wdt0 = timer_group0.wdt;
22    let timer_group1 = TimerGroup::new(
23        peripherals.TIMG1,
24        &clocks,
25        &mut system.peripheral_clock_control,
26    );
27    rtc.swd.disable();
28    rtc.rwdt.disable();
29    wdt0.disable();
30    wdt1.disable();
31
32    println!("Hello world!");
33
34    loop {}
35 }
```
 `main` 함수 속 내용들:
- `let peripherals = Peripherals::take().unwrap();`
  - HAL 드라이버는 보통 PAC(peripheral access crate)을 통해 접근하는 주변 장치의 소유권을 갖는다.
  - 여기서 우리는 PAC(peripheral access crate)의 모든 주변 장치를 가져가서 나중에 HAL 드라이버에게 전달합니다.
- `let mut system = peripherals.SYSTEM.split();`
  - 때때로 peripheral 장치(여기서 시스템 peripheral 장치)는 거친 입자이며 HAL 드라이버에 정확히 맞지 않습니다. 그래서 여기서 우리는 시스템 주변 장치를 드라이버에 전달되는 더 작은 조각으로 나눕니다.
- `let clocks = ClockControl::boot_defaults(system.clock_control).freeze();`
  - 여기서 우리는 시스템 clock를 구성합니다 - 이 경우, 기본값을 권장합니다
  - clock을 freeze하여 추후에 바꾸지 못하도록 합니다.
  - 몇몇 드라이버들은 rate와 duration 계산 방법에 대한 clock의 레퍼런스를 필요로 합니다 
- 다음 코드 블록은 부팅 후 워치독을 비활성화하기 위해 일부 주변 장치(즉, RTC와 두 타이머 그룹)를 인스턴스화합니다.
  - 이 코드가 없으면 잠시 후 SoC가 다시 부팅됩니다.
  - 재부팅을 방지하는 또 다른 방법은 워치독에 [전원을 공급](https://docs.rs/esp32c3-hal/0.10.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed)하는 것입니다
- `println!("Hello world!");`
  - "Hello world!" 출력합니다
- `loop {}`
  - 함수는 절대 반환되지 않아야하기 때문에 루프에서 "아무것도하지 않습니다"

[diverging-function]: https://doc.rust-lang.org/beta/rust-by-example/fn/diverging.html
[wtd-feeding]: https://docs.rs/esp32c3-hal/0.10.0/esp32c3_hal/prelude/trait._embedded_hal_watchdog_Watchdog.html#tymethod.feed

## 코드 실행

코드를 빌드하고 실행하는 것은 어렵지 않습니다.

```shell
cargo run
```

이렇게 하면 configuration에 따라 코드가 빌드되고 [`espflash`](https://github.com/esp-rs/espflash/tree/main/espflash)를 실행하여 코드를 보드에 플래시합니다.

우리의 [`runner` configuration][runner-config] 은 [`espflash`][espflash]에 `--monitor` 인수도 있기때문에, 우리는 코드출력을 볼수있습니다.



[`espflash`][espflash] 를 설치했는지 확인하세요,그렇지 않으면 이 단계가 fail 하게됩니다.  [`espflash`][espflash] 설치방법은 다음과 같습니다.:
`cargo install espflash`

이와 유사한 내용을 확인해야 합니다:

```text
[2023-04-17T14:17:08Z INFO ] Serial port: '/dev/ttyACM0'
[2023-04-17T14:17:08Z INFO ] Connecting...
[2023-04-17T14:17:09Z INFO ] Using flash stub
[2023-04-17T14:17:09Z WARN ] Setting baud rate higher than 115,200 can cause issues
Chip type:         esp32c3 (revision v0.3)
Crystal frequency: 40MHz
Flash size:        4MB
Features:          WiFi, BLE
MAC address:       60:55:f9:c0:39:7c
App/part. size:    203,920/4,128,768 bytes, 4.94%
[00:00:00] [========================================]      13/13      0x0
[00:00:00] [========================================]       1/1       0x8000
[00:00:01] [========================================]      64/64      0x10000
[2023-04-17T14:17:11Z INFO ] Flashing has completed!
Commands:
    CTRL+R    Reset chip
    CTRL+C    Exit

...
Hello world!
```

여기에 표시되는 것은 1단계 및 2단계 부트로더의 메시지와 "Hello world" 메시지입니다!

이것은 실제 코드가 동작하는 것입니다

재부팅 단축키는 `CTRL+R` , exit 단축키는 `CTRL+C` 입니다.

프로젝트를 빌드하는 동안 문제가 발생하면 [문제 해결](https://ing03201.github.io/rust-on-esp-book-ko-kr/troubleshooting/index.html) 장을 참조하십시오.

[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
[runner-config]: https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner
[troubleshooting]: ../../troubleshooting/index.md
