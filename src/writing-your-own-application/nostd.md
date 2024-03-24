# `no_std` 어플리케이션 만들기
`no_std` 애플리케이션을 개발하는 방법을 배우고 싶다면, 다음 교육 자료를 참조하십시오:

- [Embedded Rust (`no_std`) on Espressif][no-std-book]
- [`no_std-training`][no-std-repository] 레포지토리

교육은 [ESP32-C3-DevKit-RUST-1][esp-rust-board]을 기반으로 합니다. 다른 Espressif 개발 보드를 사용할 수 있지만 코드 변경 및 구성 변경이 필요할 수 있습니다.

* 입문 수준의 예시:
   * [A basic hello-world][hello-world]
   * [A panic example][panic]
   * [A blinky example][blinky]
   * [A button example][button]
   * [A button with an interrupt example][button-interrupt]

> ⚠️ **참고**: 모든 SoC [`esp-hal`][esp-hal]의 예제 폴더 아래에 특정 주변 장치의 사용을 다루는 몇 가지 예가 있습니다. 예: [`esp32c3-hal/examples`][esp32c3-hal-examples]

[no-std-book]: https://esp-rs.github.io/no_std-training/
[no-std-repository]: https://github.com/esp-rs/no_std-training
[esp-rust-board]: https://github.com/esp-rs/esp-rust-board
[hello-world]: https://github.com/esp-rs/no_std-training/tree/main/intro/hello-world
[panic]: https://github.com/esp-rs/no_std-training/tree/main/intro/panic
[blinky]: https://github.com/esp-rs/no_std-training/tree/main/intro/blinky
[button]: https://github.com/esp-rs/no_std-training/tree/main/intro/button
[button-interrupt]: https://github.com/esp-rs/no_std-training/tree/main/intro/button-interrupt
[esp-hal]: https://github.com/esp-rs/esp-hal
[esp32c3-hal-examples]: https://github.com/esp-rs/esp-hal/tree/main/esp32c3-hal/examples
