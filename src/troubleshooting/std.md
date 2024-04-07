# `esp-idf-hal` 기반 프로젝트들

## `LIBCLANG_PATH` 환경변수가 정의되지않았을때

```text
thread 'main' panicked at 'Unable to find libclang: "couldn't find any valid shared libraries matching: ['libclang.so', 'libclang-*.so', 'libclang.so.*', 'libclang-*.so.*'], set the `LIBCLANG_PATH` environment variable to a path where one of these files can be found (invalid: [])"', /home/esp/.cargo/registry/src/github.com-1ecc6299db9ec823/bindgen-0.60.1/src/lib.rs:2172:31
```



우리는 [`bindgen`][`bindgen`]이 ESP-IDF C 헤더에 Rust 바인딩을 생성하기 위해  `libclang`이 필요합니다. `Espup`에서 생성된 내보내기 파일을 소싱했는지 확인하십시오. [환경 변수 설정][set-up-the-environment-variables]을 참조하십시오.

[`bindgen`]: https://github.com/rust-lang/rust-bindgen
[set-up-the-environment-variables]: ./../installation/riscv-and-xtensa.md#3-set-up-the-environment-variables

## `ldproxy`가 보이지않을때

```shell
error: linker `ldproxy` not found
  |
  = note: No such file or directory (os error 2)
```

`std` 애플리케이션을 구축하려는 경우 [ldproxy][ldproxy]를 설치해야 합니다.[ `std` 개발 요구 사항][rust-esp-book-std-requirements] 보기

```shell
cargo install ldproxy
```

[ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[rust-esp-book-std-requirements]: ./../installation/std-requirements.md

## `sdkconfig.defaults`파일이 업데이트되었지만 아무런 영향을 미치지 않는 것으로 나타남

적용하려면 `sdkconfig.defaults`의 변경 사항을 위해 프로젝트를 정리하고 재구성해야 합니다.

```shell
cargo clean
cargo build
```

## 이 페이지에 언급된 Crate에 대한 문서가 오래되었거나 누락되었습니다

[docs.rs][docs.rs]에 의해 부과된 [자원 제한][resource limits]으로 인해, 문서를 작성하는 동안 인터넷 접속이 차단됩니다. 이러한 이유로, 우리는 `esp-idf-sys` 또는 그것에 따라 어떤 crate에도 대한 문서를 만들 수 없습니다.

대신, 우리는 문서를 만들고 GitHub 페이지에서 직접 호스팅하고 있습니다:

- [`esp-idf-hal` 문서][`esp-idf-hal` documentation]
- [`esp-idf-svc` 문서][`esp-idf-svc` documentation]
- [`esp-idf-sys` 문서][`esp-idf-sys` documentation]

[resource limits]: https://docs.rs/about/builds#hitting-resource-limits
[docs.rs]: https://docs.rs
[`esp-idf-hal` documentation]: https://esp-rs.github.io/esp-idf-hal/esp_idf_hal/
[`esp-idf-svc` documentation]: https://esp-rs.github.io/esp-idf-svc/esp_idf_svc/
[`esp-idf-sys` documentation]: https://esp-rs.github.io/esp-idf-sys/esp_idf_sys/

## 작업 `main`의 스택 오버플로가 감지되었습니다

2단계 부트로더가 이 오류를 보고한다면, 주요 작업에 대한 스택 크기를 늘려야 할 것입니다. 이것은 `sdkconfig.defaults` 파일에 다음을 추가하여 수행할 수 있습니다:

```text
CONFIG_ESP_MAIN_TASK_STACK_SIZE=7000
```

이 예에서, 우리는 주요 작업의 스택에 7kB를 할당하고 있다.

## 워치독 타이머를 비활성화하는 방법?

`sdkconfig.defaults` 파일에 추가하세요:

```text
CONFIG_INT_WDT=n
CONFIG_ESP_TASK_WDT=n
```

이러한 구성 파일을 수정할 때 재구축하기 전에 프로젝트를 정리해야 한다는 것을 기억하세요.
