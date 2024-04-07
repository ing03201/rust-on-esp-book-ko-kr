# `espflash`

## `rtc_clk_init: Possibly invalid CONFIG_XTAL_FREQ setting`

이 문제는 26MHz 크리스탈 오실레이터가 있는 ESP32 모듈을 사용하는 사용자에 의해 보고됩니다. 이 문제의 근본 원인은 espflash에 의해 깜박이는 기본 부트로더가 40MHz 크리스탈을 예상한다는 것이다.

### `esp-idf-sys` 기반 프로젝트 빌드하는 경우

26MHz 크리스탈을 사용하도록 [`sdkconfig`](https://github.com/esp-rs/esp-idf-sys/blob/master/BUILD-OPTIONS.md#sdkconfig)가 제대로 설정되어 있는지 확인하세요. 다음과 같은 구성 옵션을 포함해야 합니다:

```
CONFIG_XTAL_FREQ_26=y
```

또한 `espflash`보다 `cargo-espflash`를 사용하는 것을 선호해야 합니다. `cargo-espflash`는 프로젝트와 통합되며 기본 부팅 로더 대신 프로젝트 옆에 내장된 부트로더를 플래시합니다.

`espflash`를 사용하려면, `--bootloader`를 사용하여 적절한 부트로더 이미지를 지정해야 합니다. `target/<MCU의 대상 폴더>/<빌드에 따라 디버그 또는 릴리스>/build/esp-idf-sys-*/build/bootloader/bootloader.bin`에서 부트로더를 찾을 수 있습니다.

### `esp-hal` 기반 프로젝트를 구축하는 경우

HAL([ESP32](https://docs.rs/esp32-hal/latest/esp32_hal/), [ESP32-C2](https://docs.rs/esp32c2-hal/latest/esp32c2_hal/))이 올바른 결정 주파수로 구성되어 있는지 확인하세요. 이렇게 하려면, 기본 기능을 비활성화하고 `xtal-26mhz`를 활성화해야 합니다(다른 기본 기능 외에).

flasing 일때, `--bootloader`를 사용하여 적절한 부트로더 이미지를 지정해야 합니다. 현재 `esp-idf` 기반 프로젝트를 사용하여 이 부트로더를 구축해야 합니다(Rust 또는 C 기반은 동등하게 작동해야 하며, [esp-idf-template](https://github.com/esp-rs/esp-idf-template)로 설정된 프로젝트를 권장합니다).
