# QEMU

Espressif는 Espressif 칩에서 작동하도록 필요한 패치와 함께 [espressif/QEMU][espressif-qemu]에서 QEMU 포크를 유지합니다. QEMU를 구축하고 프로젝트를 에뮬레이트하는 방법에 대한 지침은 [QEMU wiki][qemu-wiki]를 참조하십시오.

QEMU를 구축하면, `qemu-system-xtensa` 파일이 있어야 합니다.

[espressif-qemu]: https://github.com/espressif/qemu
[qemu-wiki]: https://github.com/espressif/qemu/wiki

## 프로젝트를 QEMU에서 실행하기

> ⚠️ **Note**: 현재 ESP32만 지원되므로, `xtensa-esp32-espidf` 대상을 위해 컴파일하고 있는지 확인하세요.

QEMU에서 프로젝트를 실행하려면 부트로더와 파티션 테이블이 병합된 펌웨어/이미지가 필요합니다. 우리는 그것을 생성하기 위해 [`cargo-espflash`][cargo-espflash]를 사용할 수 있습니다:

```shell
cargo espflash save-image --chip esp32 --merge <OUTFILE> --release
```

[`espflash`][espflash]를 사용하는 것을 선호한다면, 먼저 프로젝트를 구축한 다음 이미지를 생성하여 동일한 결과를 얻을 수 있습니다.

```shell
cargo build --release
espflash save-image --merge ESP32 target/xtensa-esp32-espidf/release/<NAME> <OUTFILE>
```

이제, QEMU에서 이미지를 실행하세요:
```shell
/path/to/qemu-system-xtensa -nographic -machine esp32 -drive file=<OUTFILE>,if=mtd,format=raw
```

[cargo-espflash]: https://github.com/esp-rs/espflash/tree/main/cargo-espflash
[espflash]: https://github.com/esp-rs/espflash/tree/main/espflash
