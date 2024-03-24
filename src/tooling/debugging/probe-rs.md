# `probe-rs`

[`probe-rs`][probe-rs] 프로젝트는 다양한 디버그 프로브를 사용하여 임베디드 MCU와 상호 작용하는 도구 세트입니다. 그것은 [OpenOCD][openocd], [pyOCD][pyocd], [Segger tools][segger-tools] 등과 유사하다. 다음을 포함하되 이에 국한되지 않는 도구 모음과 함께  `ARM` & `RISC-V` 아키텍처에 대한 지원이 있습니다.

- 디버거
  - GDB 지원.
  - 대화형 디버깅을 위한 CLI.
  - VS Code 익스텐션.

- [Real Time Transfer (RTT)][rtt]
  - [IDF의 `app_trace` 구성 요소][app-trace-idf]와 유사하다.

- [Flash-algorithm][https://github.com/probe-rs/flash-algorithm.git]

[`probe-rs`][probe-rs] 웹사이트의 [설치][prober-rs-installation] 및 [셋업][prober-rs-setup] 지침을 따르세요.

 [`USB-JTAG-SERIAL` peripheral][usb-jtag-serial]  장치가 포함된 Espressif 제품은 외부 하드웨어 없이 `probe-rs`를 사용할 수 있습니다.

[probe-rs]: https://probe.rs/
[openocd]: https://openocd.org/
[pyocd]: https://pyocd.io/
[segger-tools]: https://www.segger.com/
[app-trace-idf]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/app_trace.html
[rtt]: https://wiki.segger.com/RTT
[prober-rs-installation]: https://probe.rs/docs/getting-started/installation/
[prober-rs-setup]: https://probe.rs/docs/getting-started/probe-setup/
[usb-jtag-serial]: index.md#usb-jtag-serial-peripheral

## `probe-rs`으로 플래싱하기

`probe-rs`는 [ESP-IDF image format][idf-image]을 지원하기 때문에 대상에 어플리케이션을 플래시하는 데 사용할 수 있습니다.

  - ESP32-C3 플래싱하는 명령어 예제: `probe-rs run --chip esp32c3`

flash 명령은 프로젝트의 `.cargo/config.toml`파일에 다음을 추가하여 사용자 지정 Cargo runner로 설정할 수 있습니다.

```toml
[target.'cfg(any(target_arch = "riscv32", target_arch = "xtensa"))']
runner = "probe-rs run --chip esp32c3"
```

이 구성을 사용하면 `cargo run`을 사용하여 애플리케이션을 플래시하고 모니터링할 수 있습니다.

[idf-image]: https://docs.espressif.com/projects/esptool/en/latest/esp32c3/advanced-topics/firmware-image-format.html

## VS Code 익스텐션

VS 코드에는 `probe-rs` 확장이 있습니다. 설치, 구성 및 사용 방법에 대한 자세한 내용은 `probe-rs` [VS Code 문서][probe-rs-vscode]를 참조하십시오.

### `launch.json` 예제

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "probe-rs-debug",
            "request": "launch",
            "name": "Launch",
            "cwd": "${workspaceFolder}",
            "chip": "esp32c3", //!MODIFY
            "flashingConfig": {
                "flashingEnabled": true,
                "resetAfterFlashing": true,
                "haltAfterReset": true,
                "formatOptions": {
                    "format": "idf" //!MODIFY (or remove). Valid values are: 'elf'(default), 'idf'
                }
            },
            "coreConfigs": [
                {
                    "coreIndex": 0,
                    "programBinary": "target/riscv32imc-unknown-none-elf/debug/${workspaceFolderBasename}", //!MODIFY
                    "rttEnabled": true,
                    "rttChannelFormats": [
                        {
                            "channelNumber": 0,
                            "dataFormat": "String",
                            "showTimestamp": true,
                        }
                    ]
                }
            ]
        },
        {
            "type": "probe-rs-debug",
            "request": "attach",
            "name": "Attach",
            "cwd": "${workspaceFolder}",
            "chip": "esp32c3", //!MODIFY
            "coreConfigs": [
                {
                    "coreIndex": 0,
                    "programBinary": "target/riscv32imc-unknown-none-elf/debug/${workspaceFolderBasename}", //!MODIFY
                    "rttEnabled": true,
                    "rttChannelFormats": [
                        {
                            "channelNumber": 0,
                            "dataFormat": "String",
                            "showTimestamp": true,
                        }
                    ]
                }
            ]
        }
    ]
}
```

> ⚠️ **참고**: 예제 `launch.json` 은 `rtt`를 사용하며, [`esp-println`][esp-println] 및  [`esp-backtrace`][esp-backtrace]와 같은 일부 상자에서 이러한 기능을 활성화해야 할 수 있습니다.  `esp-println` 및 `esp-backtrace`를 사용하는 ESP32-C3 `no_std` 프로젝트 예제:
>
> ```toml
> esp-backtrace = { version = "0.9.0", features = ["esp32c3", "panic-handler", "exception-handler", "print-rtt"] }
> esp-println = { version = "0.7.0", features = ["esp32c3", "rtt"], default-features = flase }
> ```

`Launch` 구성은 장치를 플래시하고 디버깅 프로세스를 시작하는 반면 `Attach`는 이미 실행 중인 장치의 응용 프로그램에서 디버깅을 시작합니다. 자세한 내용은 [launch와 attach의 차이점][vscode-configs] 에 대한 VS Code 문서를 참조하십시오.


[probe-rs-vscode]: https://probe.rs/docs/tools/debugger/
[esp-println]: https://github.com/esp-rs/esp-println
[esp-backtrace]: https://github.com/esp-rs/esp-backtrace?tab=readme-ov-file#features
[vscode-configs]: https://code.visualstudio.com/docs/editor/debugging#_launch-versus-attach-configurations

## `cargo-flash` 와 `cargo-embed`

`probe-rs`는 이 두 가지 도구와 함께 제공됩니다:

- [`cargo-flash`][cargo-flash]: 빌드된 바이너리를 타겟에 다운로드하고 실행하는 플래시 도구.
- [`cargo-embed`][cargo-embed]: RTT 터미널이나 GDB 서버를 열 수 있는 `cargo-flash`의 슈퍼셋. [configuration file][cargo-embed-config]은 동작을 정의하는 데 사용할 수 있다.

[cargo-flash]: https://probe.rs/docs/tools/cargo-flash/
[cargo-embed]: https://probe.rs/docs/tools/cargo-embed/
[cargo-embed-config]: https://probe.rs/docs/tools/cargo-embed/#configuration

## GDB Integration

`probe-rs`에는 일반적인 도구로 일반적인 워크플로우에 통합할 수 있는 GDB 스텁이 포함되어 있습니다.  `probe-rs gdb` 명령은 기본적으로 포트 `1337`에서 GDB 서버를 실행합니다.

지원되는 모든 Espressif 제품이 있는 GDB는 [`espressif/binutils-gdb`][binutils-repo]에서 얻을 수 있습니다.

[binutils-repo]: https://github.com/espressif/binutils-gdb
