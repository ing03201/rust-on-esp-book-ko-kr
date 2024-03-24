# 개발 접근 방식의 개요

Espressif chips에서 Rust를 사용하는 방법은 다음과 같습니다.:

-  `std` 라이브러리, a.k.a. 스탠다드 라이브러리 사용하기.
-  `core`라이브러리 (`no_std`), a.k.a. 배어메탈 개발 사용하기.

두 접근 방식 모두 장점과 단점이 있으므로, 프로젝트의 필요에 따라 결정을 내려야 합니다. 이 장에는 두 가지 접근 방식에 대한 개요가 포함되어 있습니다.

- [스탠다드 라이브러리 사용하기 (`std`)][rust-esp-book-std]
- [코어 라이브러리 사용하기(`no_std`)][rust-esp-book-no-std]

[The Embedded Rust Book][embedded-rust-book-intro-std]의 다양한 런타임 비교도 참조하십시오.

GitHub의 [esp-rs organization][esp-rs organization]은 Espressif 칩에서 Rust를 실행하는 것과 관련된 여러 저장소의 홈입니다. 필요한 크레이트의 대부분은 여기에 소스 코드를 호스팅하고 있다.

[rust-esp-book-std]: ./using-the-standard-library.md
[rust-esp-book-no-std]: ./using-the-core-library.md
[embedded-rust-book-intro-std]: https://docs.rust-embedded.org/book/intro/no-std.html#a-no_std-rust-environment
[esp-rs organization]: https://github.com/esp-rs/

## 네이밍 컨벤션 레포지토리

[esp-rs organization][esp-rs organization]에서, 우리는 다음과 같은 표현을 사용합니다:

- `esp-`로 시작하는 저장소는 `no_std` 접근 방식에 초점을 맞추고 있다. 예를 들어, `esp-hal`
  - `no_std`는 베어 메탈 위에서 작동하므로, `esp-`는 Espressif chip 이다.
- `esp-idf-`로 시작하는 저장소는 `std` 접근 방식에 초점을 맞추고 있다. 예를 들어, `esp-idf-hal`
  - `std`는 베어 메탈을 제외하고 `esp-idf-`인 [추가 레이어][additional layer]가 필요합니다.

[additional layer]: https://github.com/espressif/esp-idf

## Espressif 제품 지원

> ⚠️ **참고**:
>
> - ✅ - 이 기능은 구현되어있거나 지원됩니다.
> - ⏳ - 그 기능은 개발 중입니다.
> - ❌ - 이 기능은 지원되지 않습니다.

| Chip     | `std` | `no_std` |
| -------- | :---: | :------: |
| ESP32    |   ✅   |    ✅     |
| ESP32-C2 |   ✅   |    ✅     |
| ESP32-C3 |   ✅   |    ✅     |
| ESP32-C6 |   ✅   |    ✅     |
| ESP32-S2 |   ✅   |    ✅     |
| ESP32-S3 |   ✅   |    ✅     |
| ESP32-H2 |   ✅   |    ✅     |
| ESP8266  |   ❌   |    ✅     |

> ⚠️ **참고**: ESP8266 시리즈는 이 책의 범위를 벗어난다. ESP8266 시리즈에 대한 Rust 지원은 제한적이며 Espressif에서 공식적으로 지원되지 않습니다.

특정 상황에서 지원되는 제품은 책 전체에서 _지원되는Espressif 제품들_이라고 불릴 것이다.
