# `std` 개발 요구사항

대상 아키텍처에 관계없이, [`std`][rust-esp-book-overview-std] 애플리케이션을 구축하는 데 필요한 다음과 같은 도구가 설치되어 있는지 확인하십시오:

- ESP-IDF 전제조건:
  - 윈도우: [`python`][python-website-download] 와 [`git`][git-website-download]
  - 리눅스: [리눅스 ESP-IDF 전제조건][esp-idf-linux].
  - macOS: macOS ESP-IDF prerequisites][esp-idf-macos].
- [`ldproxy`][embuild-github-ldproxy] binary crate: A tool that forwards linker arguments to the actual linker that is also given as an argument to `ldproxy`. Install it by running:
- [`ldproxy`][embuild-github-ldproxy] 바이너리 크레이트: `ldproxy`에 대한 인수로도 주어진 실제 링커에 링커 인수를 전달하는 도구. 실행하여 설치하세요:
  
    ```shell
    cargo install ldproxy
    ```

> ⚠️ **참고**: std 런타임은 [ESP-IDF][esp-idf-github] (Espressif IoT Development Framework)를 호스팅 환경으로 사용하지만 사용자는 설치할 필요가 없습니다. ESP-IDF는 `std` 애플리케이션을 구축할 때 모든 `std` 프로젝트가 사용해야 하는 크레이트인  [`esp-idf-sys`][esp-idf-sys-github]에 의해 자동으로 다운로드되고 설치됩니다.
>

[rust-esp-book-overview-std]: ../overview/using-the-standard-library.md
[python-website-download]: https://www.python.org/downloads/windows/
[git-website-download]: https://git-scm.com/downloads
[embuild-github-ldproxy]: https://github.com/esp-rs/embuild/tree/master/ldproxy
[esp-idf-sys-github]: https://github.com/esp-rs/esp-idf-sys
[esp-idf-github]: https://github.com/espressif/esp-idf
[esp-idf-linux]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/linux-macos-setup.html#for-linux-users
[esp-idf-macos]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/linux-macos-setup.html#for-macos-users

