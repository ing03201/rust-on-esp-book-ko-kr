# `espflash`

`espflash`는 Espressif SoCs 와 modules을 위한 [esptool.py][esptool]를 기반으로 하는 시리얼 플래시 usb 유틸리티입니다.

[`espflash`][espflash] 저장소에는 두 개의 crate들, `cargo-espflash` 와 `espflash`가 포함되어 있습니다. 이 crate 대한 자세한 내용은 아래의 각 섹션을 참조하십시오.

[esptool]: https://github.com/espressif/esptool
[espflash]: https://github.com/esp-rs/espflash

> ⚠️ **Note**: 아래에 표시된  `espflash` 및  `cargo-espflash` 명령은 버전 `2.0` 이상이 사용되었다고 가정합니다.

## `cargo-espflash`

교차 컴파일과 플래싱을 처리하는 `cargo`에 대한 하위 명령을 제공합니다.

`cargo-espflash`를 설치하려면, [필요한 종속성][cargo-espflash-dependencies]이 설치되어 있는지 확인한 다음, 다음 명령을 실행하십시오:

```shell
cargo install cargo-espflash
```

이 명령은 Cargo 프로젝트, 즉 `Cargo.toml` 파일이 포함된 디렉토리 내에서 실행되어야 합니다. 예를 들어, 'blinky'라는 예제를 만들려면, 결과 바이너리를 장치에 플래시한 다음, 시리얼 모니터를 시작하십시오:

```shell
cargo espflash flash --example=blinky --monitor
```

자세한 내용은  [`cargo-espflash`][cargo-espflash] README를 참조하십시오.

[cargo-espflash]: https://github.com/esp-rs/espflash/blob/master/cargo-espflash/README.md
[cargo-espflash-dependencies]: https://github.com/esp-rs/espflash/blob/main/cargo-espflash/README.md#installation

## `espflash`

ELF 파일을 장치에 플래시하는 독립형 명령줄 애플리케이션을 제공합니다.

`espflash`를 설치하려면, [필요한 종속성][cargo-espflash-dependencies]이 설치되어 있는지 확인한 다음, 다음 명령을 실행하십시오:

```shell
cargo install espflash
```

이미 다른 방법으로 ELF 바이너리를 구축했다고 가정하면, 특히  `espflash`를 사용하여 장치에 다운로드하고 시리얼 포트를 모니터링할 수 있습니다. 예를 들어, `idf.py`를 사용하여 [ESP-IDF][esp-idf]에서 `getting-started/blinky`예제를 구축했다면, 다음과 같은 것을 실행할 수 있습니다:

```shell
espflash flash build/blinky --monitor
```

자세한 내용은  [`espflash` README][espflash-readme]를 참조하십시오.

`espflash`는 프로젝트의  `.cargo/config.toml` 파일에 다음을 추가하여 Cargo runner로 사용할 수 있습니다.

```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "espflash flash --monitor"
```
이 구성을 사용하면  `cargo run`을 사용하여 애플리케이션을 플래시하고 모니터링할 수 있습니다.

[esp-idf]: https://github.com/espressif/esp-idf
[espflash-readme]: https://github.com/esp-rs/espflash/blob/master/espflash/README.md
[espflash-dependencies]:https://github.com/esp-rs/espflash/blob/main/espflash/README.md#installation
