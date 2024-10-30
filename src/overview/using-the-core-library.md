# 코어 라이브러리 사용하기 (`no_std`)

임베디드 Rust 개발자들에게는 `no_std` 를 사용하는것이 더 친숙할수 있습니다. `std` (Rust [`스탠다드`][rust-lib-std] 라이브러리) 를 사용하지않고 부분집합인 [`core`][rust-lib-core] 라이브러리를 사용합니다. [The Embedded Rust Book][embedded-rust-book] 에는 훌륭한 [세션][embedded-rust-book-no-std]이 있습니다.

여기서 주의할 점은  `no_std` 은 Rust `core` 라이브러리를 사용한다는 것입니다. Rust `standard` 의 일부이기때문에 `no_std` crate는 `std` 환경에서 컴파일 할 수 있습니다. 하지만, 역은 성립하지않습니다: `std` crate는 `no_std` 환경에서 컴파일 할 수 없습니다. 이 정보는 어떤 라이브러리를 선택할지 결정할 때 기억할 가치가 있습니다.

[embedded-rust-book]: https://docs.rust-embedded.org/
[embedded-rust-book-no-std]: https://docs.rust-embedded.org/book/intro/no-std.html
[rust-lib-core]: https://doc.rust-lang.org/core/index.html
[rust-lib-std]: https://doc.rust-lang.org/std/index.html

## 현재 지원사항들

아래 표는 다양한 Espressif  제품에 대한 현재 `no_std` 지원에 대해 설명합니다.

|          | [HAL][esp-hal] | [Wi-Fi/BLE/ESP-NOW][esp-wifi] | [Backtrace][esp-backtrace] | [Storage][esp-storage] |
| -------- | :------------: | :---------------------------: | :------------------------: | :--------------------: |
| ESP32    |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C3 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-C6 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-H2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-S2 |       ✅        |               ✅               |             ✅              |           ✅            |
| ESP32-S3 |       ✅        |               ✅               |             ✅              |           ✅            |

> ⚠️ **Note**:
>
> - ✅ Wi-Fi/BLE/ESP-NOW에서 대상이 적어도 나열된 기술 중 하나를 지원하는 것을 의미합니다. 자세한 내용은 esp-wifi repository의  [Current support][esp-wifi-current-support] 표를 참조하십시오.
> - [ESP8266 HAL][esp8266-hal]은 유지보수 모드이며 이 칩에 대해서는 더 이상 개발이 이루어지지 않습니다.

[esp-hal]: https://github.com/esp-rs/esp-hal/tree/main/esp-hal "하드웨어 추상계층"
[esp-wifi]: https://github.com/esp-rs/esp-hal/tree/main/esp-wifi "Wi-Fi, BLE and ESP-NOW 지원"
[esp-backtrace]: https://github.com/esp-rs/esp-hal/tree/main/esp-backtrace "Exception 과 panic 핸들러"
[esp-storage]: https://github.com/esp-rs/esp-hal/tree/main/esp-storage "암호화되지 않은 플래시 메모리에 액세스할 수 있는 임베디드 스토리지 traits"
[esp-wifi-current-support]: https://github.com/esp-rs/esp-hal/tree/main/esp-wifi#current-support
[esp8266-hal]: https://github.com/esp-rs/esp8266-hal "ESP8266 하드웨어 추상 계층"

### `esp-rs` 관련 Crates 

| Repository                       | Description                                                  |
| -------------------------------- | ------------------------------------------------------------ |
| [`esp-hal`][esp-hal]             | 하드웨어 추상계층                                            |
| [`esp-pacs`][esp-pacs]           | Peripheral 접근crates                                        |
| [`esp-wifi`][esp-wifi]           | Wi-Fi, BLE and ESP-NOW 지원                                  |
| [`esp-alloc`][esp-alloc]         | 간단한 Heap 할당자                                           |
| [`esp-println`][esp-println]     | `print!`,  `println!`                                        |
| [`esp-backtrace`][esp-backtrace] | 예외상황과 패닉 핸들러                                       |
| [`esp-storage`][esp-storage]     | 암호화되지 않은 플래시 메모리에 액세스할 수 있는 임베디드 스토리지 traits |

### Core Library(`no_std`)를 권장하는 예시

- 작은 메모리 사용량: 임베디드 시스템의 리소스가 제한되어있고 메모리 사용량이 적어야 하는 경우 `std` 기능을 사용하면 컴파일 되는 최종 바이너리 크기와 컴파일 시간이 많이 들기 때문에, bare-matal을 사용할 가능성이 높습니다.
- 하드웨어 직접 제어: 임베디드 시스템에서 낮은 수준의 장치 드라이버나 특수 하드웨어 기능에 대한 액세스와 같이 하드웨어에 대한 보다 직접적인 제어가 필요한 경우 `std` 가 하드웨어와 직접 상호 작용하기 어려울 수 있는 추상화를 추가하기 때문에 베어메탈을 사용하는 것이 좋습니다
- 시간이 중요한 어플리케이션: 임베디드 시스템이 실시간적 퍼포먼스와 빠른 응답속도를 요구한다면, `std` 는 예측할 수 없는 지연과 오버헤드를 초래할 수 있으므로 `no_std`를 사용하는 것이 좋습니다.
- 커스텀 요구사항들: 베어메탈을 사용하면 애플리케이션의 동작을 보다 맞춤화하고 세밀하게 제어할 수 있으므로 전문화된 환경이나 비표준 환경에서 유용하게 사용할 수 있습니다.

[esp-pacs]: https://github.com/esp-rs/esp-pacs "Peripheral access crates"
[esp-alloc]: https://github.com/esp-rs/esp-hal/tree/main/esp-alloc "Simple heap allocator"
[esp-println]: https://github.com/esp-rs/esp-hal/tree/main/esp-println "print!, println!"
