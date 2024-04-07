# Visual Studio Code

더 일반적인 개발 환경 중 하나는 RA라고도 알려진 [Rust Analyzer][rust-analyzer]와 함께 Microsoft의 [Visual Studio Code][vscode] 텍스트 편집기입니다.

Visual Studio Code는 풍부한 확장 생태계를 갖춘 오픈 소스 및 크로스 플랫폼 그래픽 텍스트 편집기입니다. [Rust Analyzer extension][rust-analyzer-extension]은 Rust를 위한 [언어 서버 프로토콜][language-server-protocol]의 구현을 제공하며 자동 완성, 이동 정의 등과 같은 기능을 추가로 포함합니다.

Visual Studio Code는 가장 인기 있는 패키지 관리자를 통해 설치할 수 있으며, 설치 프로그램은 공식 웹사이트에서 사용할 수 있습니다. [Rust Analyzer extension][rust-analyzer-extension]은 내장된 확장 관리자를 통해 Visual Studio Code에 설치할 수 있습니다.

Rust Analyzer와 함께 도움이 될 수 있는 다른 extension들이 있습니다:

- TOML 기반 구성 파일 편집을 위한 [Even Better TOML][even-better-toml]
- Rust dependencies 관리를 도와주는 [crates][crates]

[vscode]: https://code.visualstudio.com/
[rust-analyzer]: https://rust-analyzer.github.io/
[rust-analyzer-extension]: https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer
[language-server-protocol]: https://microsoft.github.io/language-server-protocol/
[even-better-toml]: https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml
[crates]: https://marketplace.visualstudio.com/items?itemName=serayuzgur.crates

## 팁과 요령

### `no_std`에서의 Rust Analyzer 사용하기

`std`를 지원하지 않는 대상을 위해 개발하는 경우, Rust Analyzer는 이상하게 동작할 수 있으며, 종종 다양한 오류를 보고합니다. 이것은 프로젝트에 `.vscode/settings.json` 파일을 만들고 다음을 채워서 해결할 수 있습니다:

```json
{
  "rust-analyzer.checkOnSave.allTargets": false
}
```

### Cargo Hints When Using Custom Toolchains

### 커스텀 툴체인을 사용할때 Cargo hints

`Xtensa` 대상과 마찬가지로 사용자 지정 툴체인을 사용하는 경우, 사용자 경험을 개선하기 위해 `rust-toolchain.toml` 파일을 통해 `cargo`에 몇 가지 힌트를 제공할 수 있습니다.

```toml
[toolchain]
channel = "esp"
components = ["rustfmt", "rustc-dev"]
targets = ["xtensa-esp32-none-elf"]
```

## 다른 IDE들

우리는 Rust를 잘 지원하고 개발자들 사이에서 인기가 있기 때문에 VS Code를 다루기로 결정했습니다. [CLion][clion]과 [vim][vim]과 같은 유사한 Rust를 지원하는 다른 IDE도 있지만, 이는 이 책의 범위를 벗어납니다.

[clion]: https://www.jetbrains.com/clion/
[vim]: https://www.vim.org/
