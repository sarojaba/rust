# Rust 프로그래밍 언어

이것은 표준 라이브러리, 도구 그리고 문서등이 포함된 Rust 컴파일러이다.

## 설치하기

현재 Rust 컴파일러는 [tarball]로 제작되고, Windows의 경우는 [installer][win-exe] 사용을 추천한다.

Rust 컴파일러는 Rust로 작성되었기에, 스스로 미리 컴파일된 "snapshot" 버전으로 만들어진다(개발의 초기 단계에). 이처럼, 소스를 빌드하려면 snapshots을 받아오기 위해 인터넷 연결과, 유효한 스냅샷 바이너리를 실행할 수 있는 OS가 필요하다.

현재 스냅샷 바이너리는 아래의 플랫폼들에서 제작되고 테스트된다:

* Windows (7, Server 2008 R2), x86 only
* Linux (various distributions), x86 and x86-64
* OSX 10.6 ("Snow Leopard") or greater, x86 and x86-64

다른 플랫폼에서 작동하는 것을 찾을 수도 있지만, 선호되는 빌드 환경을 지원하는 것이 우선의 목표이다.

> ***Note:*** 윈도우즈 사용자들은 위키에서 세부적인
> [getting started][wiki-start] 내용을 읽어야한다.
> 바이너리 인스톨러를 사용하더라도 윈도우즈에서 빌드하려면 MinGW 설치가 필요하다.
> 자세한 내용은 여기서 논의하지 않는다.

소스로부터 빌드하려면 사전에 필요한 패키지는 다음과 같다:

* g++ 4.4 or clang++ 3.x
* python 2.6 or later (but not 3.x)
* perl 5.0 or later
* gnu make 3.81 or later
* curl

비교적 현대적인 *nix 시스템을 사용하고 필요 조건을 만족한다고 가정하면, 다음의 명령들을 수행하면 된다.

    $ curl -O http://static.rust-lang.org/dist/rust-0.6.tar.gz
    $ tar -xzf rust-0.6.tar.gz
    $ cd rust-0.6
    $ ./configure
    $ make && make install

만약 목표 디렉토리를 수정할 권한을 가지고 있지 않다면 `sudo make install`을 사용해야 할 수도 있다. `configure` 명령의 인자로 `--prefix`를 전달하여 설치 장소를 조정할 수 있다. 다양한 다른 옵션들도 지원되고 `--help` 명령을 통해 더 자세한 정보를 얻을 수 있다.

`make install`이 성공적으로 완료되면 `/usr/local/bin` 디렉토리 내의 몇가지 프로그램을 볼 수 있을 것이다. `rustc`는 the Rust 컴파일러, `rustdoc`는
API-문서 도구, 그리고 `rustpkg`는 Rust 패키지 관리자 및 빌드 시스템이다.

[wiki-start]: https://github.com/mozilla/rust/wiki/Note-getting-started-developing-Rust
[tarball]: http://static.rust-lang.org/dist/rust-0.6.tar.gz
[win-exe]: http://static.rust-lang.org/dist/rust-0.6-install.exe


## License

Rust is primarily distributed under the terms of both the MIT license
and the Apache License (Version 2.0), with portions covered by various
BSD-like licenses.

See LICENSE-APACHE, LICENSE-MIT, and COPYRIGHT for details.

## More help

The [tutorial] is a good starting point.

[tutorial]: http://sarojaba.github.io/rust-doc-korean/doc/tutorial.html
