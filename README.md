# The Rust on ESP Book

이 리포지토리는 "The Rust on ESP" 채그이 소스를 가지고있습니다

## 빠른 시작

이 책은 [`mdbook`] 을 사용하여 생성되며 추가적으로 다이어그램 지원을 위해 [`mdbook-mermaid`] 전처리를 사용합니다. 이 툴들을 설치하려면 아래 명령어를 참고하세요.

```shell
cargo install mdbook mdbook-mermaid
```

 `mdbook` 과`mdbook-mermaid` 가 설치되었다면, 아래를 실행하여 리포지토리를 복제하고 개발 서버를 시작할 수 있습니다:

```shell
git clone https://github.com/esp-rs/book
cd book/
mdbook serve
```

[`mdbook`]: https://github.com/rust-lang/mdBook
[`mdbook-mermaid`]: https://github.com/badboy/mdbook-mermaid

## 라이센스

The Rust on ESP Book은 다음 라이센스로 배포됩니다:

- 이 책에 포함된 코드 샘플은 [MIT License] 와 [Apache License v2.0] 로 라이센스 되어있습니다.
- 이 책에 수록된 문장들은 크리에이티브 커먼즈 라이센스 [CC-BY-SA v4.0] 로 라이센스 되어있습니다.

[mit license]: ./LICENSE-MIT
[apache license v2.0]: ./LICENSE-APACHE
[cc-by-sa v4.0]: ./LICENSE-CC-BY-SA

### 컨트리뷰션

상세하게 표시하지 않는 이상, 당신이 만든 모든 컨트리뷰션은 Apache-2.0 license와 같이 라이센스를 받아야합니다.

컨트리뷰션하실 때, [Rust Documentation Style Guide](rust-doc-style-guide.md)를 참고해주세요.
