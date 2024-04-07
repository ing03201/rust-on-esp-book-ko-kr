
# OpenOCD

[`probe-rs`][probe-rs]와 마찬가지로, OpenOCD는  `Xtensa` 아키텍처를 지원하지 않습니다. 그러나, Espressif는 Espressif의 칩을 지원하는  [`espressif/openocd-esp32`][espressif-openocd-esp32] 리포지토리에서 OpenOCD의 fork 리포지토리를 유지합니다.

플랫폼에  `openocd-esp32`를 설치하는 방법에 대한 지침은 [Espressif 문서][espressif-documentation]에서 찾을 수 있습니다.

지원되는 모든 Espressif 제품이 있는 GDB는  [`espressif/binutils-gdb`][binutils-repo]에서 얻을 수 있습니다.

일단 설치되면, 올바른 인수로  `openocd`를 실행하는 것만큼 간단합니다. [`USB-JTAG-SERIAL` peripheral 장치][usb-jtag-serial]가 내장된 칩의 경우, 일반적으로 ESP32-C3에서 즉시 작동하는 구성 파일이 있습니다.

```shell
openocd -f board/esp32c3-builtin.cfg
```

다른 구성의 경우, 칩과 인터페이스를 지정해야 할 수 있습니다. 예를 들어, J-Link가 있는 ESP32:

```shell
openocd -f interface/jlink.cfg -f target/esp32.cfg
```

[probe-rs]: ./probe-rs.md
[espressif-openocd-esp32]: https://github.com/espressif/openocd-esp32
[espressif-documentation]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/index.html#setup-of-openocd
[binutils-repo]: https://github.com/espressif/binutils-gdb
[usb-jtag-serial]: index.md#usb-jtag-serial-peripheral

## VS Code Extension

OpenOCD는 Espressif 제품을 디버깅하기 위해  [`cortex-debug`][cortex-debug] 확장을 통해 VS 코드에서 사용할 수 있습니다.

[cortex-debug]: https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug

### Configuration

1. 필요한 경우, 외부 JTAG 어댑터를 연결하세요.
   1. ESP-IDF 프로그래밍 가이드의 다른 JTAG 인터페이스 구성 섹션을 참조하십시오. 예: [Section for ESP32][jtag-interfaces-esp32]
> ⚠️ **Note**: Windows에서는, `USB Serial Converter A 0403 6010 00`드라이버가 WinUSB이어야 합니다.
2. VSCode 설정하기
   1. [Cortex-Debug][cortex-debug]  VS Code 확장 익스텐션 설치하기.
   2. 디버그하려는 프로젝트 트리에  `.vscode/launch.json` 파일을 만드세요.
   3.  `executable`, `svdFile`, `serverpath` 및 `toolchainPrefix`  필드를 업데이트하십시오.

```json
{
  // Use IntelliSense to learn about possible attributes.
  // Hover to view descriptions of existing attributes.
  // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      // more info at: https://github.com/Marus/cortex-debug/blob/master/package.json
      "name": "Attach",
      "type": "cortex-debug",
      "request": "attach", // launch will fail when attempting to download the app into the target
      "cwd": "${workspaceRoot}",
      "executable": "target/xtensa-esp32-none-elf/debug/.....", //!MODIFY
      "servertype": "openocd",
      "interface": "jtag",
      "toolchainPrefix": "xtensa-esp32-elf", //!MODIFY
      "openOCDPreConfigLaunchCommands": ["set ESP_RTOS none"],
      "serverpath": "C:/Espressif/tools/openocd-esp32/v0.11.0-esp32-20220411/openocd-esp32/bin/openocd.exe", //!MODIFY
      "gdbPath": "C:/Espressif/tools/riscv32-esp-elf-gdb/riscv32-esp-elf-gdb/bin/riscv32-esp-elf-gdb.exe", //!MODIFY
      "configFiles": ["board/esp32-wrover-kit-3.3v.cfg"], //!MODIFY
      "overrideAttachCommands": [
        "set remote hardware-watchpoint-limit 2",
        "mon halt",
        "flushregs"
      ],
      "overrideRestartCommands": ["mon reset halt", "flushregs", "c"]
    }
  ]
}
```

[jtag-interfaces-esp32]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/jtag-debugging/configure-other-jtag.html

# 여러 코어들을 디버깅하기

때때로 GDB 또는 VSCode에서 각 코어를 개별적으로 디버깅해야 할 수도 있습니다. 이 경우, `set ESP_RTOS none`을  `set ESP_RTOS hwthread`으로 바꾸십시오. 이것은 각 코어를 GDB의 하드웨어 스레드로 보이게 할 것이다. 이것은 현재 Espressif 공식 문서에 문서화되어 있지 않지만 OpenOCD 문서에 문서화되어 있습니다: https://openocd.org/doc/html/GDB-and-OpenOCD.html
