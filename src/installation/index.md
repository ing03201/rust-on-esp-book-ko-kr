# Setting Up a Development Environment

현재, Espressif SoCs는 `RISC-V` and `Xtensa` 두 가지 아키텍처를 기반으로 하고 있습니다. 

두 아키텍처 모두 `std` 와 `no_std` 접근 방식을 지원합니다.

개발 환경을 설정하려면 다음을 수행합니다:

1. [Rust 설치하기][install-rust]
2. 대상 장치에 대한 설치 요구사항
    - [`RISC-V` targets only][risc-v-targets]
    - [`RISC-V` and `Xtensa` targets][rics-v-xtensa-targets]

대상 아키텍처와 상관없이, `std` 개발을 위해서 [`std` 개발 요구사항][rust-esp-book-std-requirements] 설치를 잊지마세요.

개발환경은 [컨테이너][use-containers]에서 호스팅할 수 있으니 참고하시기 바랍니다..

[install-rust]: ./rust.md
[risc-v-targets]: ./riscv.md
[rics-v-xtensa-targets]: ./riscv-and-xtensa.md
[rust-esp-book-std-requirements]: ./std-requirements.md
[use-containers]: ./using-containers.md
