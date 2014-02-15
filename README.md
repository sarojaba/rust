# The Rust Programming Language

This is a compiler for Rust, including standard libraries, tools and
documentation.

## Quick Start

### Windows

1. 다운로드 받고 [installer and MinGW][win-wiki]를 이용합니다.
2. [튜토리얼]을 읽습니다.
2. 즐깁니다!

> ***Note:*** Windows 사용자들은 위키에서 세부적인
> [getting started][wiki-start] 내용을 읽어야합니다.

[튜토리얼]: http://sarojaba.github.io/rust-doc-korean/doc/tutorial.html
[wiki-start]: https://github.com/mozilla/rust/wiki/Note-getting-started-developing-Rust
[win-wiki]: https://github.com/mozilla/rust/wiki/Using-Rust-on-Windows

### Linux / OS X

1. 사전에 필요한 패키지를 설치합니다.
    * `g++` 4.4 or `clang++` 3.x
    * `python` 2.6 or later (but not 3.x)
    * `perl` 5.0 or later
    * GNU `make` 3.81 or later
    * `curl`
2. 다운로드 받고 Rust를 빌드합니다.

    [tarball]을 다운로드 받거나 repo]로부터 직접 빌드할 수 있습니다.

    [tarball]에서 빌드하는 방법

        $ curl -O http://static.rust-lang.org/dist/rust-0.9.tar.gz
        $ tar -xzf rust-0.9.tar.gz
        $ cd rust-0.9

    또는 [repo]에서 빌드하는 방법

        $ git clone https://github.com/mozilla/rust.git
        $ cd rust

    Rust의 소스 코드를 구했다면, 구성과 빌드를 할 수 있습니다.

        $ ./configure
        $ make && make install

    > ***Note:*** 만약 목표 디렉토리를 수정할 권한을 가지고 있지 않다면 
    > `sudo make install`을 사용해야 할 수도 있습니다.
    > `configure` 명령의 인자로 `--prefix`를 전달하여
    > 설치 장소를 조정할 수 있습니다. 다양한 다른 옵션들도 지원되고
    > `--help` 명령을 통해 더 자세한 정보를 얻을 수 있습니다.

    `make install`이 성공적으로 완료되면,
    `/usr/local/bin` 디렉토리에 몇가지 프로그램이 설치될 것입니다.
    `rustc`는 the Rust 컴파일러, `rustdoc`는 API-문서 도구입니다.
    system.
3. [튜토리얼]을 읽습니다.
4. 즐깁니다!

[repo]: https://github.com/mozilla/rust
[tarball]: http://static.rust-lang.org/dist/rust-0.9.tar.gz
[tutorial]: http://static.rust-lang.org/doc/0.9/tutorial.html

## 노트

Rust 컴파일러는 Rust로 작성되었기에, (개발의 초기 단계에) 스스로 미리 컴파일된 "snapshot" 버전으로 만들어집니다.
이처럼, 소스를 빌드하려면 snapshots을 받아오기 위해 인터넷 연결과, 유효한 스냅샷 바이너리를 실행할 수 있는 OS가 필요합니다.

현재 스냅샷 바이너리는 몇가지 플랫폼들에서 제작되고 테스트됩니다.

* Windows (7, 8, Server 2008 R2), x86 only
* Linux (2.6.18 or later, various distributions), x86 and x86-64
* OSX 10.7 (Lion) or greater, x86 and x86-64

다른 플랫폼에서 작동하는 것을 찾을 수도 있지만, 이것들이 많이 사용되기 때문에 우리가 공식적으로 지원하는 빌드 환경입니다.

현재 Rust는 swapping 없이 빌드하기 위해 약 1.5G의 RAM을 필요로 합니다.
만약 swap이 사용된다면, 빌드하는데 아주 오랜 시간이 걸릴것입니다.

[wiki]에 더 많은 문서들이 있습니다..

[wiki]: https://github.com/mozilla/rust/wiki

## 라이선스

Rust는 MIT 라이선스와 Apache 라이선스 (버전 2.0)의 계약 조건에 따라 우선적으로 배포되고, 일부분은 여러가지의 BSD 계열의 라이선스를 따릅니다.

LICENSE-APACHE, LICENSE-MIT, 그리고 COPYRIGHT를 자세히 보시길 바랍니다.
