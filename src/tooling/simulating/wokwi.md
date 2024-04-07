# Wokwi

[Wokwi][wokwi]는 Espressif Chips에서 Rust 프로젝트(`std` 및 `no_std` 모두) 시뮬레이션을 지원하는 온라인 시뮬레이터입니다. 예시 목록과 새로운 프로젝트를 시작하는 방법은 [wokwi.com/rust][wokwi-rust]를 참조하십시오.

Wokwi는 다른 많은 기능 중에서 Wi-Fi 시뮬레이션, 가상 로직 분석기 및 [GDB debugging][gdb-debugging]을 제공합니다. 자세한 내용은 [Wokwi 문서][wokwi-documentation]를 참조하십시오. ESP 칩의 경우, 현재 지원되는 [시뮬레이션 기능][wokwi-simulation-features] 표가 있습니다.

[wokwi]: https://wokwi.com/
[wokwi-rust]: https://wokwi.com/rust
[gdb-debugging]: https://docs.wokwi.com/gdb-debugging
[wokwi-documentation]: https://docs.wokwi.com/
[wokwi-simulation-features]: https://docs.wokwi.com/guides/esp32#simulation-features

## Wokwi VS Code 확장프로그램 사용하기
Wokwi는 몇 개의 파일만 추가하여 코드 편집기에서 직접 프로젝트를 시뮬레이션할 수 있는 VS 코드 확장 프로그램을 제공합니다. 자세한 내용은 [Wokwi 문서][wokwi-vscode]를 참조하십시오. VS 코드 디버거를 사용하여 코드를 디버깅할 수도 있습니다. [코드 디버깅][wokwi-debugging]을 참조하십시오.

[templates][templates]을 사용하고 기본값을 사용하지 않을 때, 프롬프트가 있습니다 (`Wokwi VS Code 확장으로 Wokwi 시뮬레이션을 지원하는 프로젝트 구성`) Wokwi VS Code 확장자를 사용하는 데 필요한 파일을 생성합니다.

![Wokwi VS Code example](../../assets/wokwi-vscode.png)

[wokwi-vscode]: https://docs.wokwi.com/vscode/getting-started
[wokwi-debugging]: https://docs.wokwi.com/vscode/debugging
[templates]: ./../../writing-your-own-application/generate-project/index.md

## Using `wokwi-server`

[`wokwi-server`][wokwi-server]는 프로젝트의 Wokwi 시뮬레이션을 시작하기 위한 CLI 도구입니다. 즉, 기계나 컨테이너에 프로젝트를 구축하고 결과 바이너리를 시뮬레이션할 수 있습니다.

[`wokwi-server`][wokwi-server]는 또한 칩 자체 이외의 더 많은 하드웨어 부품으로 다른 Wokwi 프로젝트에서 결과 바이너리를 시뮬레이션할 수 있습니다. 자세한 지침은 `wokwi-server` README의 해당 [섹션][wokwi-server-custom]을 참조하십시오.

[wokwi-server]: https://github.com/MabezDev/wokwi-server
[wokwi-server-custom]: https://github.com/MabezDev/wokwi-server#simulating-your-binary-on-a-custom-wokwi-project

## 커스텀 칩들
Wokwi는 Wokwi에서 지원되지 않는 구성 요소의 동작을 프로그래밍할 수 있는 사용자 지정 칩을 생성할 수 있습니다. 자세한 내용은 공식 [Wokwi 문서][wokwi-custom-chip]를 참조하십시오.

커스텀 칩은 Rust로도 쓸 수 있습니다! 자세한 내용은 [Wokwi Custom Chip API][rust-chip-api]를 참조하십시오. 예를 들어, Rust의 커스텀 [inverter chip][custom-chip-example].

[wokwi-custom-chip]: https://docs.wokwi.com/chips-api/getting-started
[rust-chip-api]: https://github.com/wokwi/wokwi_chip_ll
[custom-chip-example]: https://github.com/wokwi/rust_chip_inverter
