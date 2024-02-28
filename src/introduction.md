<p style="text-align:center;"><img src="./assets/esp-logo-black.svg" width="50%"></p>

# 소개

이 책의 목표는 [Espressif][espressif] 장치에서  [Rust 프로그래밍 언어][rust]의 포괄적인 사용 가이드를 제공하는 것입니다.

[Espressif][espressif] 장치의  [Rust 프로그래밍 언어][rust]의 지원이 현재 빠르게 진행 중에 있습니다. 이 때문에 이 문서의 일부가 빠르게 변경될수 있으며 오래되어있을수있습니다.

Rust on ESP 관련 툴과 라이브러리는 Gihtub의 [esp-rs organization][esp-rs]를 참고하세요. 이 organization은 Espressif  직원 뿐만 아니라 커뮤니티 구성원도 관리합니다

모든 기술적 질문과 이슈에 대해 자유롭게 [`esp-rs` community on Matrix][matrix] 에 참여하세요! 이 커뮤니티는 모두에게 열려있습니다!

[rust]: https://www.rust-lang.org/
[espressif]: https://espressif.com/
[esp-rs]: https://github.com/esp-rs/
[matrix]: https://matrix.to/#/#esp-rs:matrix.org



## 이 책의 독자층

이 책은 Rust에 대한 경험이 있는 사람들을 대상으로 하며, 임베디드 개발 및 전자제품에 대한 초보적인 지식을 전제로 합니다. 위 경험이 없는 사람들에게는 먼저 [가정 및 전제조건][전제조건]과 [리소스][리소스]섹션을 읽어보고 최신 정보를 얻도록 권장합니다

[prerequisites]: #가정-및-전제조건
[resources]: #리소스



### 가정 및 전제조건

- Rust 프로그래밍 언어를 편하게 사용할 수 있으며 데스크톱 환경에서 응용 프로그램을 작성하고 실행했습니다.
- 이 책은 Rust [2021년 에디션][rust-2021]을 기준으로 하므로 코드 관용구를 숙지해야합니다. 
- C또는 C++ 등과 같은 언어로 임베디드 시스템을 개발하는데 편안하고 다음 개념에 익숙해야합니다
  - cross-compilation
  - `UART`, `SPI`, `I2C`, 등과 같은 일반적인 디지털 인터페이스.
  - Memory-mapped peripherals
  - Interrupts

[rust-2021]: https://doc.rust-lang.org/edition-guide/rust-2021/index.html



### 리소스

위에서 언급한 내용에 대해 잘 모르거나 경험이 부족한 경우 또는 이 책에서 언급한 특정 주제에 대해 더 많은 정보를 원하시면 도움이 될 수 있습니다.

| Resource                                               | Description                                                  |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| [The Rust Programming Language][rust-book]             | Rust에 대해 잘 모르신다면 이 책을 먼저 읽으시는것이 좋습니다. |
| [The Embedded Rust Book][embedded-rust-book]           | 여기서 Rust's Embedded Working Group에서 제공하는 여러 리소스를 찾을 수 있습니다. |
| [The Embedonomicon][embedonomicon]                     | Rust 임베디드 프로그래밍에서 중요한 세부내용들               |
| [Embedded Rust (std) on Espressif][std-training]       | Espressif SoCs를 위한 `std` 사용 가이드                      |
| [Embedded Rust (no_std) on Espressif][no_std-training] | Espressif SoCs를 위한 `no_std` 사용 가이드                   |

[rust-book]: https://doc.rust-lang.org/book/
[embedded-rust-book]: https://docs.rust-embedded.org/book/index.html
[embedonomicon]: https://docs.rust-embedded.org/embedonomicon/
[std-training]: https://esp-rs.github.io/std-training/
[no_std-training]: https://esp-rs.github.io/no_std-training/



## Translations

이 책은 자발적으로 번역되었습니다. 번역이 아래 리스트에 포함되길 원한다면 Pull Request 열어서 추가해주세요.

- [简体中文](https://narukara.github.io/rust-on-esp-book-zh-cn/) ([repository](https://github.com/Narukara/rust-on-esp-book-zh-cn))

- [한국어](https://ing03201.github.io/rust-on-esp-book-ko-kr/) ([repository](https://github.com/ing03201/rust-on-esp-book-ko-kr))

  

## 사용법

이 책은 계단식 챕터로 구성된 책입니다. 따라서 이전 챕터에 내용을 이해하지 못한채 다음 챕터로 넘어간다면 이해에 어려움이 있습니다.



## 이 책에 컨트리뷰팅하기

이 책에 대한 작업은 [이 저장소][book-repository]에 저장됩니다.

이 책을 따라가는데 어려움이 있거나 책의 일부분이 명확하지않다면 그것은 버그입니다. 

이 책 리포지토리의 [이슈][book-issues]탭에 제보해주세요.

오타 수정 및 번역 이슈의 경우 [한국어 리포지토리 이슈][book-issues-kr]탭에 제보해주시면 빠르게 응답하겠습니다.

[book-issues]: https://github.com/esp-rs/book/issues/
[book-repository]: https://github.com/esp-rs/book
[book-issues-kr]:  https://github.com/ing03201/rust-on-esp-book-ko-kr/issues



## 자료의 재사용

이 책은 다음 라이선스로 배포됩니다:

- 이 책에 수록된 코드 샘플 및 독립 Cargo 프로젝트는  [MIT License][mit-license] 와 [Apache License v2.0][apache-license]의 조건에 따라 라이센스가 부여됩니다.
- 이 책에 수록된 산문, 그림, 도표는 Creative Commons [CC-BY-SA v4.0][cc-license] 조건에 따라 라이센스가 부여됩니다..

요약하면, 작업에 당사의 텍스트 또는 이미지를 사용하려면 다음과 같은 작업이 필요합니다:

- 출처를 명시해야 합니다.(예: 슬라이드에 이 책을 언급하고 관련 페이지 링크를 제공합니다)
- [CC-BY-SA v4.0][cc-license] license에 대한 링크를 제공해야 합니다.
- 어떤 내용을 변경했는지 표시하고 동일한 라이센스에 따라 당사의 내용을 변경할 수 있습니다

이 책이 유용하다고 생각되면 알려주세요!

[mit-license]: https://opensource.org/licenses/MIT
[apache-license]: http://www.apache.org/licenses/LICENSE-2.0
[cc-license]: https://creativecommons.org/licenses/by-sa/4.0/legalcode
[전제조건]: #가정-및-전제조건

