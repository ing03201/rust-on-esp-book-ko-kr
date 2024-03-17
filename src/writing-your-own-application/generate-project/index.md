# 템플릿으로 프로젝트 만들기

현재 두 템플릿 레포지토리를 유지하고있습니다.
- [`esp-template`][esp-template] - `no_std` 템플릿
- [`esp-idf-template`][esp-idf-template] - `std` 템플릿

두 템플릿 모두 기존 템플릿을 기반으로한 새 프로젝트를 만들 수 있는 도구 [`cargo-generate`][cargo-generate] 기반으로 합니다. 

두 템플릿 모두 기존 템플릿을 기반으로 새 프로젝트를 만들 수 있는 도구인  [`cargo-generate`][cargo-generate]를 기반으로 합니다. 우리의 경우,  [`esp-idf-template`][esp-idf-template] 또는  [`esp-template`][esp-template]를 사용하여 필요한 모든 구성과 종속성을 가진 애플리케이션을 생성할 수 있습니다.

1.  `cargo generate`를 설치합니다:
    ```shell
    cargo install cargo-generate
    ```
    
2. 템플릿 중 하나를 기반으로 프로젝트를 생성하세요.:
    - `esp-template`:
        
        ```shell
        cargo generate esp-rs/esp-template
        ```
        템플릿 프로젝트에 대한 자세한 내용은 [Understanding `esp-template`][understanding-esp-template]을 참조하십시오.
    - `esp-idf-template`:
        
        ```shell
        cargo generate esp-rs/esp-idf-template cargo
        ```
        템플릿 프로젝트에 대한 자세한 내용은 [Understanding `esp-idf-template`][understanding-esp-idf-template]을 참조하십시오.

    `cargo generate` 서브커맨드가 호출되면, 타겟 앱에 관한 몇 가지 질문에 답하라는 메시지가 표시됩니다. 이 과정이 완료되면, 당신은 모든 올바른 구성으로 빌드 가능한 프로젝트를 갖게 될 것입니다.
    
3. 생성된 프로젝트를 빌드/실행하세요:
   - `cargo build`를 사용하여 적절한 툴체인과 대상을 사용하여 프로젝트를 컴파일하세요.
   - `cargo run`을 사용하여 프로젝트를 컴파일하고, 플래시하고, 타겟 장치로 시리얼 모니터를 여세요.

[cargo-generate]: https://github.com/cargo-generate/cargo-generate
[esp-idf-template]: https://github.com/esp-rs/esp-idf-template
[esp-template]: https://github.com/esp-rs/esp-template
[understanding-esp-template]: ./esp-template.md
[understanding-esp-idf-template]: ./esp-idf-template.md

## 템플릿에서 개발 컨테이너 사용하기

두 템플릿 저장소 모두 Dev Containers 지원에 대한 프롬프트가 있습니다. 템플릿 README의 [Dev Containers][dev-container]  섹션에서 자세한 내용을 참조하십시오.

개발 컨테이너는 [Setting up a Development Environment][setting-env]장의 [Using Container][using-container] 섹션에서 설명된 [`idf-rust`][idf-rust] 컨테이너 이미지를 사용합니다. 이 이미지는 설치 없이 Espressif 칩용 Rust 애플리케이션을 개발할 준비가 된 환경을 제공합니다. Dev Containers는 또한 [Wokwi simulator][wokwi] 시뮬레이터와 통합하여 프로젝트를 시뮬레이션하고 [`web-flash`][web-flash]를 사용하여 컨테이너에서 깜박일 수 있습니다.

[dev-container]: https://github.com/esp-rs/esp-template/tree/main/docs#dev-containers
[idf-rust]: https://hub.docker.com/r/espressif/idf-rust/tags
[using-container]: ../../installation/using-containers.md
[wokwi]: https://wokwi.com/
[web-flash]: https://github.com/bjoernQ/esp-web-flash-server
[setting-env]: ../../installation/index.md
