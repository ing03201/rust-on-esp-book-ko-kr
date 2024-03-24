# 디버깅

이 장에서 다룰 다른 도구를 사용하여 러스트 응용 프로그램을 디버깅하는 것도 가능합니다.

모든 디버깅 방법에서 어떤 칩이 지원되는지 보려면 아래 표를 참조하십시오:

|              | **probe-rs** | **OpenOCD** |
| :----------: | :----------: | :---------: |
|  **ESP32**   |      ⏳       |      ✅      |
| **ESP32-C2** |      ✅       |      ✅      |
| **ESP32-C3** |      ✅       |      ✅      |
| **ESP32-C6** |      ✅       |      ✅      |
| **ESP32-H2** |      ✅       |      ✅      |
| **ESP32-S2** |      ✅       |      ✅      |
| **ESP32-S3** |      ✅       |      ✅      |

## `USB-JTAG-SERIAL` Peripheral

우리의 최근 제품 중 일부는 외부 하드웨어 디버거 없이 디버깅할 수 있는  `USB-JTAG-SERIAL`주변 장치를 포함합니다. 인터페이스 구성에 대한 자세한 정보는 이 주변 장치를 지원하는 칩의 공식 문서에서 찾을 수 있습니다:

- [ESP32-C3][esp32c3-docs]

    - 내장 JTAG 인터페이스의 가용성은 ESP32-C3 개정판에 따라 다릅니다:
      - 0.3보다 오래된 개정판에는 JTAG 인터페이스가 **내장되어 있지 않습니다**.
      - 개정판 0.3(및 그 이상)에는 JTAG 인터페이스가 **내장되어 있으며**, 디버깅하기 위해 외부 장치를 연결할 필요가 없습니다.


    ESP32-C3 개정판을 찾으려면, 다음을 실행하세요:
    ```shell
    cargo espflash board-info
    # or
    espflash board-info
    ```
- [ESP32-C6][esp32c6-docs]
- [ESP32-H2][esp32h2-docs]
- [ESP32-S3][esp32s3-docs]

[esp32c3-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32c6-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32c6/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32h2-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32h2/api-guides/jtag-debugging/configure-builtin-jtag.html
[esp32s3-docs]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/jtag-debugging/configure-builtin-jtag.html

