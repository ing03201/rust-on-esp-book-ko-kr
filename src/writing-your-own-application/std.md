# `std` 어플리케이션 만들기

`std` 애플리케이션을 개발하는 방법을 배우고 싶다면, [Ferrous Systems][ferrous-systems]와 함께 개발된 다음 교육 자료를 참조하십시오:

-  [Embedded Rust on Espressif][std-book]
-  [`std-training`][std-repository] 레포지토리

The training is based on [ESP32-C3-DevKit-RUST-1][esp-rust-board]. You can use any other Espressif development board, but code changes and configuration changes might be needed.

교육은 [ESP32-C3-DevKit-RUST-1][esp-rust-board]을 기반으로 합니다. 다른 Espressif 개발 보드를 사용할 수 있지만, 코드 변경과 구성 변경이 필요할 수 있습니다.

* [입문 수준의 예제][intro]:
   * [A basic hardware-check][hardware-check]
   * [An HTTP Client][http-client]
   * [An HTTP Server][http-server]
   * [An MQTT Client][mqtt]
* [고급 수준의 예제][advanced]:
   * 로우레벨 GPIO
   * 일반적인 상황의 인터럽트
   * [I2C Driver][i2c-driver]
   * [I2C Sensor Reading][i2c-sensor-reading]
   * [GPIO/Button Interrupts][button-interrupt]
   * RGB LED 조작

> ⚠️ **참고**: [`esp-idf-hal`][esp-idf-hal]의 예제 폴더 아래에 특정 주변 장치의 사용을 다루는 몇 가지 예가 있습니다. 즉,  [`esp-idf-hal/examples`][esp-idf-hal-examples].

[ferrous-systems]: https://ferrous-systems.com/
[std-book]: https://esp-rs.github.io/std-training/
[std-repository]: https://github.com/esp-rs/std-training
[esp-rust-board]: https://github.com/esp-rs/esp-rust-board
[intro]: https://github.com/esp-rs/std-training/tree/main/intro
[hardware-check]: https://github.com/esp-rs/std-training/tree/main/intro/hardware-check
[http-client]: https://github.com/esp-rs/std-training/tree/main/intro/http-client
[http-server]: https://github.com/esp-rs/std-training/tree/main/intro/http-server
[mqtt]: https://github.com/esp-rs/std-training/tree/main/intro/mqtt
[advanced]: https://github.com/esp-rs/std-training/tree/main/advanced
[i2c-driver]: https://github.com/esp-rs/std-training/tree/main/advanced/i2c-driver
[i2c-sensor-reading]: https://github.com/esp-rs/std-training/tree/main/advanced/i2c-sensor-reading
[button-interrupt]: https://github.com/esp-rs/std-training/tree/main/advanced/button-interrupt
[esp-idf-hal-examples]: https://github.com/esp-rs/esp-idf-hal/tree/master/examples
[esp-idf-hal]: https://github.com/esp-rs/esp-idf-hal
