# Containers 사용하기

로컬 시스템에 직접 설치하는 대신, 컨테이너 내에서 개발 환경을 호스팅할 수 있습니다. Espressif는 `RISC-V`와 `Xtensa` 대상 아키텍처를 모두 지원하고 `std`와 `no_std` 개발을 모두 가능하게 하는 [`idf-rust`][idf-rust] 이미지를 제공합니다.

`linux/arm64` 및 `linux/amd64` 플랫폼에 대한 수많은 태그를 찾을 수 있습니다.

각 Rust 릴리스에 대해, 우리는 다음과 같은 명명 규칙으로 태그를 생성합니다:

- `<chip>_<rust-toolchain-version>`
  - 예를 들어, `esp32_1.64.0.0`에는 `std`를 개발하기 위한 생태계와  `1.64.0.0` `Xtensa` Rust 툴체인이 있는  `ESP32`용 `no_std` 애플리케이션이 포함되어 있습니다.

특별 케이스가 있다.:

- `<chip>`은 모든 Espressif 대상과의 호환성을 나타내는 `all`이 될 수 있습니다.
- `<rust-toolchain-version>`은 `Xtensa` Rust 툴체인의 최신 릴리스를 의미하는 `latest`가 될 수 있습니다.

운영 체제에 따라 [Docker][docker], [Podman][podman], 또는 [Lima][lima]와 같은 컨테이너 런타임을 선택할 수 있습니다.

[docker]: https://www.docker.com/
[podman]: https://podman.io/
[lima]: https://github.com/lima-vm/lima
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
