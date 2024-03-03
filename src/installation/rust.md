# Rust 설치

[Rust][rust-lang-org]가 설치되어있는지 확인하십시오. 설치되어있지 않은경우, [rustup][rustup.rs-website]웹사이트를 참고하십시오.

> 🚨 **Warning**: 유닉스 기반 시스템을 사용할 때 시스템 패키지 관리자(예시. `brew`, `apt`, `dnf`, etc.)를 통해 설치한다면, 다양한 문제와 비호환성이 발생할 수 있으므로 대신 [rustup][rustup.rs-website]을 사용하는 것이 가장 좋습니다.

Windows를 사용하는 경우 아래 나열된 ABI 중 하나를 설치했는지 확인합니다. 자세한 내용은 rustup 책의 [Windows][rustup-book-windows] 챕터를 참고하세요.
- **MSVC**: 추천하는 ABI, `rustup` 기본 요구사항 리스트에 있습니다. Visual Studio에서 만든 소프트웨어와 상호운용하려면 이것을 사용하세요.
- **GNU**: GCC 툴체인을 사용하는 ABI. MinGW/MSYS2 툴체인으로 만들어진 소프트웨어와 함께 사용하려면 직접 설치하세요.

[대체 설치방법][rust-alt-installation]도 참고하세요.

[rustup.rs-website]: https://rustup.rs/
[rust-alt-installation]: https://rust-lang.github.io/rustup/installation/other.html
[rustup-book-windows]: https://rust-lang.github.io/rustup/installation/windows.html
[rust-lang-org]: https://www.rust-lang.org/
