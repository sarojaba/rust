% Rust 언어 튜토리얼

# 머리말

Rust는 타입 안전성, 메모리 안전성, 동시성 그리고 성능에 초점을 맞춘 프로그래밍 언어이다. 몇 가지로 분류되는 일반적인 에러들로부터 자유로운 대규모, 고성능 소프트웨어를 작성하도록 계획되었다. Rust는 능률적인 데이터 구조를 장려하는 고수준의 메모리 모델과 세그멘테이션 오류를 일으키는 무효한 메모리 접근을 예방하는 안전한 동시성 패턴을 가지고 있다. 컴파일 시간에 타입이 정해진다.

다중 패러다임 언어로서, Rust는 절차형, 함수형 그리고 객체지향 스타일로 작성된 코드를 모두 지원한다. 또한 다음의 고수준의 기능들을 포함한다:

* **타입 추론** 타입을 알려주는 지역 변수 선언은 선택적이다.
* **안전한 태스크 기반의 동시성** Rust의 가벼운 태스크들은 메모리를 공유하지 않는 대신 메시지를 통해 통신한다.
* **고차(Higher-order) 함수** 효율적이고 유연한 클로저는 반복과 제어 구조를 분리하여 제공한다.
* **패턴 매칭과 대수적 자료구조**
  Rust의 열거형(C의 열거형의 더 강력한 버전, 함수형 언어에서 대수적 자료구조와 비슷)에서 패턴 매칭은 프로그램 논리를 코드화하는 함축적이고 표현적인 방법이다.
* **다형성** Rust는 타입-인자의 함수와 타입, 타입 클래스 그리고 객체지향 스타일의 인터페이스를 가진다.

## 범위

이 문서는 Rust 프로그래밍 언어를 소개하는 튜토리얼이다. 문법, the 타입 시스템과 메모리 모델, 제너릭, 모듈을 포함한 언어의 기초를 다룬다. [Additional
tutorials](#what-next)은 언어에 특화된 기능을 더 깊이 다룬다.

이 튜토리얼은 독자가 하나 이상 의 C 계열의 언어에 이미 친숙하다고 가정한다. 포인터와 일반적인 메모리 관리 기법의 이해가 도움을 줄 것이다.

## 관습

튜토리얼을 통해서, 예제 코드에서 정의되는 언어 키워드와 식별자는 `코드 글꼴`로 표시된다.

코드 조각은 들여쓰여지고, monospaced 글꼴로 보여진다. 모든 코드 조각들이 전체 프로그램을 구성하지는 않는다. 간결함을 위해, 컴파일되지 않는 프로그램의 일부분을 보여줄 것이다. 실행해보기 위해, `fn main() { ... }` 안에 넣어야할 것이다, 실제로 정의되지 않은 이름을 참조하는 것을 확인해야한다.

> ***경고:*** Rust 계속 개발중인 언어이다. 언어의 잠재적인 변화, 구현 부족, 그리고 정지에 대해 알고있어야한다.

# 시작하기

현재 Rust 컴파일러는 [tarball]로 제작되고, Windows의 경우는 [installer][win-exe] 사용을 추천한다.

컴파일러는 Rust로 작성되었기에, 스스로 미리 컴파일된 "snapshot" 버전으로 만들어진다(개발 초기 단계에). 소스는 인터넷에서 지원되는 플랫폼에 맞게 이 스냅샷을 가져와 자동적으로 빌드한다.

현재 스냅샷 바이너리는 아래의 플랫폼들에서 제작되고 테스트된다:

* Windows (7, Server 2008 R2), x86 only
* Linux (various distributions), x86 and x86-64
* OSX 10.6 ("Snow Leopard") or greater, x86 and x86-64

다른 플랫폼에서 작동하는 것을 찾을 수도 있지만, 선호되는 빌드 환경을 지원하는 것이 우선의 목표이다.

> ***Note:*** 윈도우즈 사용자들은 위키에서 세부적인 [getting started][wiki-start] 내용을 읽어야한다. 바이너리 인스톨러를 사용하더라도 윈도우즈에서 빌드하려면 MinGW 설치가 필요하다. 자세한 내용은 여기서 논의하지 않는다. 마지막으로, `rustc`는 [referred to as `rustc.exe`][bug-3319]를 알고 있어야한다. 기대에 어긋나는 것이라는 것은 알고있다.

[bug-3319]: https://github.com/mozilla/rust/issues/3319
[wiki-start]: https://github.com/mozilla/rust/wiki/Note-getting-started-developing-Rust

소스로부터 빌드하려면 사전에 필요한 패키지는 다음과 같다:

* g++ 4.4 or clang++ 3.x
* python 2.6 or later (but not 3.x)
* perl 5.0 or later
* gnu make 3.81 or later
* curl

만약 충분히 필요 조건을 만족한다면, 다음의 명령들을 수행하면 된다.

~~~~ {.notrust}
$ curl -O http://static.rust-lang.org/dist/rust-0.8.tar.gz
$ tar -xzf rust-0.8.tar.gz
$ cd rust-0.8
$ ./configure
$ make && make install
~~~~

만약 목표 디렉토리를 수정할 권한을 가지고 있지 않다면 `sudo make install`을 사용해야 할 수도 있다. 설치 장소는 `configure` 명령의 인자로 `--prefix`를 전달하여 조정할 수 있다. 다양한 다른 옵션들도 지원되고 `--help` 인자를 통해 더 자세한 정보를 얻을 수 있습니다.

`make install`이 성공적으로 완료되면 `/usr/local/bin` 디렉토리 내의 몇가지 프로그램을 볼 수 있을 것이다. `rustc`는 the Rust 컴파일러, `rustdoc`는 API-문서 도구, 그리고 `rustpkg`는 Rust 패키지 관리자 및 빌드 시스템, `rusti`는 Rust REPL, 그리고 `rust`는, 위 명령들의 통합된 인터페이스, 몇개의 공통의 명령행 시나리오로 동작하는 도구이다.

[tarball]: http://static.rust-lang.org/dist/rust-0.8.tar.gz
[win-exe]: http://static.rust-lang.org/dist/rust-0.8-install.exe

## 자신의 첫 프로그램 컴파일하기

관습적으로 Rust 프로그램 파일은 `.rs`라는 확장자를 가진다. 아래의 프로그램을 가지는 `hello.rs` 파일을 만들어보자.

~~~~
fn main() {
    println("hello?");
}
~~~~

만약 Rust 컴파일러가 성공적으로 설치되었다면, `rustc hello.rs`라고 실행하면 `hello` 라는 실행파일이 생성되고(윈도우즈에서는 `hello.exe`), 실행하면 원하는 결과가 정확히 실행될 것이다.

The Rust 컴파일러는 에러가 발생한 경우 유용한 정보를 제공하려고 노력한다. 프로그램에 에러를 넣고(예를 들어, `println` 을 존재하지 않는 함수로 바꿔서) 컴파일을 하면, 아래와 비슷한 에러 메시지를 볼 수 있을 것이다.

~~~~ {.notrust}
hello.rs:2:4: 2:16 error: unresolved name: print_with_unicorns
hello.rs:2     print_with_unicorns("hello?");
               ^~~~~~~~~~~~~~~~~~~~~~~
~~~~

최대한 단순화하면, Rust 프로그램은 몇 개의 타입과 함수를 가지고 있는 `.rs` 파일이다. 만약 `main` 함수를 가지고 있으면, 실행되도록 컴파일 될 수 있다. Rust는 파일의 최상위 레벨에서 선언된 코드를 허용하지 않는다. 모든 구문은 함수 안에 존재해야 한다. Rust 프로그램은 다른 프로그램에 포함되는 라이브러리로서 컴파일 될 수 있다.

## rust 도구 사용하기

실행되는 것을 생성하기 위해 `rustc`를 직접적으로 사용하고, 수동으로 실행하는 것은 당신의 코드를 테스트하는 완전하고 유효한 방법이지만, 작은 프로젝트이거나 프로토타입이거나 당신이 초보자라면 `rust` 도구를 사용하는 것이 더 편리할 것이다.

`rust` 도구는 나머지 rust 도구들로의 중앙집중적 접근 뿐만아니라 소스 파일을 바로 실행하기 위해 간편한 단축 명령을 제공한다.
예를 들어, 현재 디렉토리에 `foo.rs` 파일이 있다면, `rust run foo.rs` 명령은 컴파일을 시도할 것이고, 성공하면 결과로 생성된 바이너리를 바로 실행한다.

모든 가능한 명령의 목록을 얻으려면, 어떤 인자도 없이 단순히 `rust`만 호출하면 된다.

## Rust 코드 수정하기

Rust 소스 배포판 `src/etc/vim/` 하위에 vim 하이라이트와 들여쓰기 스크립트가 있다. `src/etc/emacs/` 하위에 `rust-mode`라고 불리는 emacs 모드가 있지만, 디렉토리에 포함된 설명서를 꼭 읽어라. 특히, emacs 24에서 실행한다면, `rust-mode` 설치를 최신으로 유지하는 가장 쉬운 방법인 emacs 내부 패키지 관리자를 사용해라. 또한 Sublime Text 2를 위한 패키지는 [standalone][sublime]와 [Sublime Package Control][sublime-pkg]를 통해서 모두 가능하고, Kate를 위한 지원은 `src/etc/kate` 하위에 있다.

`src/etc/ctags.rust`를 통해 ctags를 지원하지만, 많은 다른 도구들과 편집기들은 아직 지원하지 않는다. 당신이 가장 선호하는 편집기를 위한 Rust 모드가 작성이 완료되면, 우리에게 알려주면 링크를 걸어주겠다.

[sublime]: http://github.com/dbp/sublime-rust
[sublime-pkg]: http://wbond.net/sublime_packages/package_control

# 기본 문법

당신이 C계열의 언어(C++, Java, JavaScript, C#, or PHP)로 프로그래밍을 했다고 가정하면, Rust는 비슷하다고 느낄 것이다. 코드는 중괄호로 구분되는 블록들의 나열이다. `if`와 `while`과 비슷한 분기나 반복을 위한 제어 구조도 있다. 함수는 `myfunc(arg1, args2);`라고 호출한다. 연산자도 C와 같고 거의 동일한 우선순위를 가진다. 주석 또한 C와 같다. 모듈 이름은 C++ 처럼 더블 콜론(::)으로 구분된다.

인지되는 주된 표면적 차이는 `if`와 `while`과 같은 제어 구조의 머리에 있는 조건이 괄호가 필요없고, 몸통은 *무조건* 중괄호로 둘러싸야한다. 몸통에 문장이 하나일때, 중괄호를 하지않는 것은 허용되지 않는다.

~~~~
# mod universe { pub fn recalibrate() -> bool { true } }
fn main() {
    /* A simple loop */
    loop {
        // A tricky calculation
        if universe::recalibrate() {
            return;
        }
    }
}
~~~~

`let` 키워드는 지역 변수를 나타낸다. 변수는 기본적으로 변하지 않는다. 나중에 재할당할 수 있는 지역 변수를 나타내기 위해, `let mut`를 사용한다.

~~~~
let hi = "hi";
let mut count = 0;

while count < 10 {
    println(fmt!("count: %?", count));
    count += 1;
}
~~~~

Rust는 지역 변수의 타입을 거의 항상 추론할 수 있지만, 콜론 다음에 타입의 이름을 적어줌으로써 변수의 타입을 명시해줄 수 있다. 반대로, 정적 아이템은 타입 주석이 항상 필요하다.

~~~~
static MONSTER_FACTOR: float = 57.8;
let monster_size = MONSTER_FACTOR * 10.0;
let monster_size: int = 50;
~~~~

앞의 예제에서 지역 변수는 shadow earlier 선언이다:
첫번째 `monster_size`는 `float`으로, 두번째 `monster_size`는 `int`로 선언되었다. 이 예제를 실제로 컴파일하더라도, 컴파일러는 첫번째 `monster_size`는 사용되지 않는다고 판단하고 경고를 표시할 것이다(이 상황은 프로그래머 오류를 발생시킬 가능성이 있기 때문이다). 사용되지 않는 변수가 의도적인 경우, 경고를 없애기 위해, `let _monster_size = 50;`처럼 이름을 밑줄로 시작하면 된다.

Rust 식별자는 알파벳 문자나 밑줄로 시작하고 그 이후로 알파벳 문자, 숫자, 밑줄의 나열된다. 선호되는 스타일은 함수, 변수, 그리고 모듈 이름은 가독성에 도움을 주기위해 밑줄이 포함된 소문자로 작성하고, 타입은 camel case로 작성한다.

~~~
let my_variable = 100;
type MyType = int;     // 기본 타입은 camel case로 작성하지 않는다.
~~~

## 표현식과 세미콜론

모든 코드에서 외관상 다르더라도, Rust의 문법과 C와 같은 predecessors 사이에는 근본적인 차이가 있다. C에서 문장인 많은 구조들은 Rust에서는 더 간결한 코드를 허용하는 표현식이다. 예를 들어, 다음과 같은 코드의 일부분을 작성할 수 있다.

~~~~
# let item = "salad";
let price;
if item == "salad" {
    price = 3.50;
} else if item == "muffin" {
    price = 2.25;
} else {
    price = 2.00;
}
~~~~

그러나, Rust에서는, `price`라는 이름을 반복할 필요가 없다.

~~~~
# let item = "salad";
let price =
    if item == "salad" {
        3.50
    } else if item == "muffin" {
        2.25
    } else {
        2.00
    };
~~~~

두 코드의 일부분은 정확히 동일하다. 둘다 주어진 조건에 따라 값을 `price`에 할당한다. 두번째 코드 조각의 블록안에 세미콜론이 없다는 것을 명심해라. 이것은 중요하다. 중괄호 사이의 블록내의 마지막 문장 뒤에 세미콜론의 누락은 전체 블록에게 마지막 표현식의 값을 준다.

다르게 말하면, Rust에서 세미콜론은 *표현식의 값을 무시한다*. 그래서 `if`의 분기가 `{ 4; }`처럼 되어있다면, 위의 예제는 단순히 `price`에 `()` (nil 또는 void)를 할당할 것이다. 그러나 세미콜론이 없으면, 각 분기는 서로 다른 값을 가지고, `price`는 분기에서 얻은 값을 가진다.

즉, 선언(변수를 위한 `let`, 함수를 위한 `fn`, 그리고 [traits](#traits), [enum types](#enums), 그리고 정적 아이템)은 표현식이고, 함수 몸체에 포함된다.

~~~~
fn is_four(x: int) -> bool {
   // No need for a return statement. The result of the expression
   // is used as the return value.
   x == 4
}
~~~~

## 기본 타입과 리터럴

일반적인 부호있는/없는 정수 타입인 `int`와 `uint`, 또한 8-, 16-, 32-, 그리고 64-bit 변수인, `i8`, `u16` 등이 있다. 정수는 10진수 (`144`), 16진수 (`0x90`), 또는 2진수 (`0b10010000`) 로 작성될 수 있다. 각 정수 타입은 리터럴의 타입을 가리키는데 사용될 수 있는 리터럴 접미사와 대응된다. 예를들어 `i`는 `int`, `u`는 `uint`, `i8`는 `i8` 타입이다.

정수 리터럴 접미사가 없으면, Rust는 근처 코드의 타입 어노테이션과 함수 시그니처에 근거해 정수 타입으로 추론할 것이다. 어떤 타입 정보도 없으면, Rust는 접미사없는 정수 리터럴인 `int` 타입으로 가정할 것이다.

~~~~
let a = 1;       // a is an int
let b = 10i;     // b is an int, due to the 'i' suffix
let c = 100u;    // c is a uint
let d = 1000i32; // d is an i32
~~~~

Rust는 `float`, `f32`, 그리고 `f64`의 세가지 부동소수점 타입이 있다. 부동소수점 숫자는 `0.0`, `1e6`, 또는 `2.1e-4` 처럼 표현된다. 정수처럼, 부동소수점 리터럴도 타입이 정확하게 추론된다. 명시적 타입의 리터럴인 접미사 `f`, `f32`, 그리고 `f64`으로 생성할 수 있다.

키워드 `true`와 `false`은 `bool` 타입의 리터럴을 생성한다.

`char` 타입인 문자의 리터럴은 `'x'`와 같이 작은 따옴표 사이에 4 바이트 유니코드 코드포인트를 적는다. C와 동일하게, Rust는 `\n`, `\r`, `\t` 같이 백슬러시 문자를 사용하는 다양한 탈출 문자를 이해한다. 문자열 리터럴은 쌍따옴표 사이에 적고, 동일한 탈출 문자를 허용한다. 문자열에 대해 더 알고싶으면 [later](#vectors-and-strings).

`()`로 적히는 nil 타입은 똑같이 `()`라고 쓰는 하나의 값만 가진다.

## 연산자

Rust의 연산자의 세트는 몇가지 놀라움을 가지고 있다. 산술연산은 `*`, `/`, `%`, `+`, `-` (곱하기, 몫, 나머지, 더하기, 빼기)로 구성된다. 또 `-`는 음수를 표현하는 단항 접미 연산. C와 마찬가지로, the 비트 연산자 `>>`, `<<`, `&`, `|`, `^`도 지원된다.

정수 값에 `!`를 적용하면, 모든 비트를 뒤집는다는 것을 명심하자(C의 `~` 처럼).

비교 연산자는 전통적인 `==`, `!=`, `<`, `>`,`<=`, and `>=` 이다. 단락 회로 (게으른) 논리 연산자는 `&&` (and) 그리고 `||` (or)가 있다.

타입 캐스팅을 위해, Rust는 `as`라는 이항 연산자를 사용한다. 좌측에 표현식은 좌측에, 타입은 우측에 놓고, 의미있는 변환이라면, 표현식의 결과는 주어진 타입으로 변한다.

~~~~
let x: float = 4.0;
let y: uint = x as uint;
assert!(y == 4u);
~~~~

## 문법 확장

*문법 확장*은 언어 안에 포함되지 않은 특별한 형식이지만, 대신 라이브러리로 제공된다. 어떤 이름이 문법 확장을 가리키는 것을 독자에게 더 명확히 하기위해, 모든 문법 확장의 이름은 `!`으로 끝난다. 표준 라이브러리는 앞으로 예제에서 종종 보게될 `sprintf` 스타일의 텍스트 형식지정자인 `fmt!`와 같이 가장 유용한 몇개의 문법 확장을 정의한다.

`fmt!`는 [printf][pf]가 지원하는 대부분의 지시자를 지원하지만, printf와 다르게, 지시자의 타입이 인자의 타입과 일치하지 않는 경우 컴파일 시간에 오류를 뱉는다.

~~~~
# let mystery_object = ();

println(fmt!("%s is %d", "the answer", 43));

// %? will conveniently print any type
println(fmt!("what is this thing: %?", mystery_object));
~~~~

[pf]: http://en.cppreference.com/w/cpp/io/c/fprintf

매크로 시스템을 통해 자신만의 문법 확장을 정의할 수 있다. 자세한 사항은 [macro tutorial][macros]를 참고하세요.

# 제어 구조

## 조건

우리는 이미 여러번 `if` 표현식을 보았다. 다시 떠올려보면, 중괄호는 필수적이고, `if`는 선택적으로 `else` 절을 가질 수 있고, `if`/`else`는 여러번 이을 수 있다:

~~~~
if false {
    println("that's odd");
} else if true {
    println("right");
} else {
    println("neither true nor false");
}
~~~~

`if` 구조에 주어진 조건은 *무조건* `bool` 타입이어야 한다. (은연중의 변환은 발생하진 않는다). 만약 분기가 값을 가지는 블록이면, 모든 분기에 블록의 끝의 값은 동일한 타입이어야 한다.

~~~~
fn signum(x: int) -> int {
    if x < 0 { -1 }
    else if x > 0 { 1 }
    else { 0 }
}
~~~~

## 패턴 매칭

Rust의 `match` 구조는 C의 `switch` 구조의 일반화되고, 깔끔한 버전이다. 값과 다수의 *분기*로 구성되고, 각 분기는 패턴으로 이름지어지고, 코드는 값과 각 패턴을 순서대로 하나라도 일치할 때까지 비교한다. 일치된 패턴은 대응하는 분기를 실행한다.

~~~~
# let my_number = 1;
match my_number {
  0     => println("zero"),
  1 | 2 => println("one or two"),
  3..10 => println("three to ten"),
  _     => println("something else")
}
~~~~

C와는 다르게, 분기 사이에 "내려가기(falling through)"가 없다. 단지 하나의 분기만 실행하고, 분기가 끝났을 때 명시적으로 `break`를 이용해 구조를 빠져나갈 필요가 없다.

A `match` 분기는 *패턴*, 화살표 `=>`, *액션* (표현식)순으로 구성된다. 리터럴은 유효한 패턴이고, 자신의 값만 일치한다. 모든 패턴이 동일한 변수의 집합과 결합한다면 하나의 분기는 파이프 연산자 (`|`)로 결합하여 다수의 서로 다른 패턴과 대응할 수도 있다. 숫자 리터럴 범위 패턴은 `M..N`처럼 두개의 점을 이용해 표현한다. 밑줄(`_`)은 어떠한 하나의 값에 대응하는 와일드카드 패턴이다. 별표(`*`)는 `enum` variant 에서 하나 혹은 여러 필드와 일치하는 독특한 와일드카드이다.

매칭된 분기에 있는 패턴 뒤에 두꺼운 화살표 `=>`가 따라오고, 그 다음은 평가되는 표현식이다. 각 케이스는 쉼표로 구분된다. 쉼표는 선택적이고, 각 케이스에 대해 블록 표현식을 사용하는 것보다 종종 편리하다.

~~~
# let my_number = 1;
match my_number {
  0 => { println("zero") }
  _ => { println("something else") }
}
~~~

`match` 구조는 *소모적(모든 가능한 경우를 다루는 분기를 가지고 있음)* 이어야 한다. 예를 들어, 와일드카드 패턴을 가지고 있는 분기가 생략되었다면 타입 판별기는 앞의 예제를 거부할 것이다.

패턴 매칭의 강력한 응용은 *디스트럭처링*이다. 이름을 데이터 타입의 내용에 대응하기위해 매칭하는 것.

> ***노트:*** 다음의 코드는 섹션 5.3에서 설명되는 튜플(`(float, float)`)을 사용한다. 이제 아이템의 목록으로서의 튜플을 떠올릴 수 있다.

~~~~
use std::float;
use std::num::atan;
fn angle(vector: (float, float)) -> float {
    let pi = float::consts::pi;
    match vector {
      (0f, y) if y < 0f => 1.5 * pi,
      (0f, y) => 0.5 * pi,
      (x, y) => atan(y / x)
    }
}
~~~~

A 패턴에서 변할 수 있는 이름은 어떤 값과 일치하고, 그리고 분기의 행동 내부의 매치된 값의 값으로 대응된다. 그래서, `(0f, y)` matches 첫번째 요소는 0이고 두번째 요소로 `y`와 엮이는 어떤 튜플과 일치한다. `(x, y)`는 두 요소를 가진 어떤 튜플과 일치하고, 두 요소는 변수와 엮인다.

`match` 분기는 패턴과 일치하는 것을 찾은 뒤, 분기를 수행할지 말지 결정하는 `bool` 타입의 표현식인 *패턴 가드*라는 이름의 가드 구문(`if EXPR`라고 적히는)을 가질 수 있다. 패턴에 엮인 변수의 범위는 이 가드 표현식 안에 있다. `angle` 예제의 첫번째 분기는 패턴 가드의 예를 보여준다.

앞서 단순 `let` 바인딩을 이미 보았지만, `let`는 당신이 생각하는 것보다 더 근사하다. 또한 디스트럭처링 패턴을 지원한다. 예를 들어, 튜플로부터 필드의 두 변수 `a`와 `b` 를 한번에 추출하기위해 다음처럼 쓸 수 있다.

~~~~
# fn get_tuple_of_two_ints() -> (int, int) { (1, 1) }
let (a, b) = get_tuple_of_two_ints();
~~~~

Let 바인딩은 단지 _반박할 수 없는_ 패턴만으로 동작한다. 즉, 매칭에 실패할 수 없는 패턴이다. 이것은 일치한 리터럴과 대부분의 `enum` 변형으로부터 `let`을 제외한다.

## 루프

`while`은 주어진 조건(`bool` 타입이어야함)이 `true`로 평가되는 동안 반복하는 루프를 의미한다. 루프 내에, 키워드 `break`는 루프를 중지하고, `loop`는 현재의 반복을 중지하고 다음 반복을 계속한다.

~~~~
let mut cake_amount = 8;
while cake_amount > 0 {
    cake_amount -= 1;
}
~~~~

`loop`는 무한 루프를 의미하고, `while true`라고 적는것보다 선호되는 방법이다.

~~~~
let mut x = 5u;
loop {
    x += x - 3;
    if x % 5 == 0 { break; }
    println(x.to_str());
}
~~~~

이 코드는 기묘한 수열을 출력하고 5로 나뉠 수 있는 수를 찾자마자 멈춘다.

# 자료 구조

## 구조체

Rust의 구조체 타입은 `struct` 예약어를 사용한 뒤에 선언된다: `struct Name { field1: T1, field2: T2 [, ...] }`, 에서 `T1`, `T2`, ... 는 타입을 의미한다. 구조체를 만들기위해, 동일한 문법을 사용해야하나, `struct` 예약어를 제거해도 된다. 예를 들어 `Point { x: 1.0, y: 2.0 }`.

구조체는 C의 구조체와 아주 비슷하고 메모리에 같은 방식으로 적재된다.(그래서 C에서 Rust의 구조체를 읽을 수 있고, 반대의 경우도 가능하다.). `mypoint.x` 처럼 구조체 필드에 접근하기 위해 마침표 연산자를 사용한다.

~~~~
struct Point {
    x: float,
    y: float
}
~~~~

상속된 변화성은 만약 구조체가 변할 수 있는 슬롯에 있다면 (또는 구조체의 필드가 변할 수 있는 슬롯에 있던지 등) 구조체의 어떤 필드 하나라도 값이 변할 수 있다는 것을 뜻한다.

(`mypoint`를 보면) 변할 수 있는 위치에서는 타입의 값, `mypoint.y += 1.0`을 할 수 있다. 그러나 변할 수 없는 위치에서는, 상속된 변화성이 없는 구조체의 할당은 타입 오류를 일으킨다.

~~~~ {.xfail-test}
# struct Point { x: float, y: float }
let mut mypoint = Point { x: 1.0, y: 1.0 };
let origin = Point { x: 0.0, y: 0.0 };

mypoint.y += 1.0; // mypoint is mutable, and its fields as well
origin.y += 1.0; // ERROR: assigning to immutable field
~~~~

`match` 패턴은 구조체의 구조를 파괴한다. 기본 문법은 `Name { fieldname: pattern, ... }`이다.

~~~~
# struct Point { x: float, y: float }
# let mypoint = Point { x: 0.0, y: 0.0 };
match mypoint {
    Point { x: 0.0, y: yy } => { println(yy.to_str());                     }
    Point { x: xx,  y: yy } => { println(xx.to_str() + " " + yy.to_str()); }
}
~~~~

일반적으로, 구조체의 필드 이름은 같은 순서로 나타날 필요가 없다. 구조체의 모든 필드에 관심이 없으면, 구조체 패턴은 다른 모든 필드를 무시하는 것을 가리키기위해 `, _`(`Name { field1, _ }`처럼)로 끝낼 수 있다. 추가적으로, 구조체 필드는 단순히 필드 이름을 바인딩 이름으로 재사용하는 축약 매칭 형식을 가진다.

~~~
# struct Point { x: float, y: float }
# let mypoint = Point { x: 0.0, y: 0.0 };
match mypoint {
    Point { x, _ } => { println(x.to_str()) }
}
~~~

## 열거형

열거형은 몇가지 대표값을 갖는 데이터 타입이다. 예를들어, 앞서 봤던 타입을 보자.

~~~~
# struct Point { x: float, y: float }
enum Shape {
    Circle(Point, float),
    Rectangle(Point, Point)
}
~~~~

이 타입의 값은 하나의 `Point` 구조체와 하나의 부동소수점을 포함하는 `Circle`, 또는 두 개의 `Point` 구조체를 갖는 `Rectangle` 이다. 실행시간 중의 특정 대표값은 C의 "tagged union" 패턴처럼 타입이 가지고 있는 실제 형식의 식별자를 포함하지만, 더 정적인 보증을 한다.

위 선언은 특정 모양을 가리킬 수 있는 `Shape` 타입을 정의할 것이고, 두 개의 함수 `Circle`과 `Rectangle`은 (명시된 타입의 인자가 취해지는)타입의 값을 구성하는데 사용될 수 있다. 그래서 `Circle(Point { x: 0f, y: 0f }, 10f)`은 새로운 원 모양을 생성하는 방법이다.

열거형 값들은 파라미터를 가질 필요가 없다. 예를 들어, 다음의 `enum` 선언은 C의 열거형과 동일하다.

~~~~
enum Direction {
    North,
    East,
    South,
    West
}
~~~~

이 선언에서 상수로 정의된 `North`, `East`, `South`, 그리고 `West`는`Direction` 타입을 가진다.

C 스타일의 열거형이라면(즉, 파라미터를 가지는 열거형 값이 없는 경우), 식별 값을  상수 값으로 명시적으로 설정하는 것이 가능하다.

~~~~
enum Color {
  Red = 0xff0000,
  Green = 0x00ff00,
  Blue = 0x0000ff
}
~~~~

만약 열거형 값에 식별값이 명시되어 있지 않으면, 기본 값은 앞의 열거형 값에 1을 더한 값이 된다. 만약 첫번째 열거형 값에 식별 값이 없다면, 기본 값은 0이다. 예를 들어, `North`의 값은 0, `East`는 1, `South`는 2, 그리고 `West`는 3이다.

C와 비슷한 열거형이면, `int`의 식별값으로 변환하기 위해 `as` 형변환 연산자를 적용할 수 있다.

복수의 열거값을 가지는 열거형 타입에 대해, 구조파괴는 내용을 얻기위한 유일한 방법이다. `area`의 정의에서 모든 열거값 생성자는 패턴으로 사용될 수 있다.

~~~~
use std::float;
# struct Point {x: float, y: float}
# enum Shape { Circle(Point, float), Rectangle(Point, Point) }
fn area(sh: Shape) -> float {
    match sh {
        Circle(_, size) => float::consts::pi * size * size,
        Rectangle(Point { x, y }, Point { x: x2, y: y2 }) => (x2 - x) * (y2 - y)
    }
}
~~~~

개별의 필드를 무시하기 위해 `_` 하나를 적을 수 있고, `Circle(*)` 처럼 열거값의 모든 필드를 무시할 수 있다. 간략 형식으로, null의 열거형 패턴은 중괄호없이 적는다.

~~~~
# struct Point { x: float, y: float }
# enum Direction { North, East, South, West }
fn point_from_direction(dir: Direction) -> Point {
    match dir {
        North => Point { x:  0f, y:  1f },
        East  => Point { x:  1f, y:  0f },
        South => Point { x:  0f, y: -1f },
        West  => Point { x: -1f, y:  0f }
    }
}
~~~~

열거값은 구조체가 될 수 있다. 다음을 살펴보자.

~~~~
use std::float;
# struct Point { x: float, y: float }
# fn square(x: float) -> float { x * x }
enum Shape {
    Circle { center: Point, radius: float },
    Rectangle { top_left: Point, bottom_right: Point }
}
fn area(sh: Shape) -> float {
    match sh {
        Circle { radius: radius, _ } => float::consts::pi * square(radius),
        Rectangle { top_left: top_left, bottom_right: bottom_right } => {
            (bottom_right.x - top_left.x) * (bottom_right.y - top_left.y)
        }
    }
}
~~~~

## 튜플

Rust에서 튜플은 필드가 이름을 갖지 않는 것만 제외하면 정확히 구조체처럼 행동한다. 그래서, 점 표기로 필드에 접근할 수 없다. 튜플은 0을 제외한 어떤 숫자라도 인자로 받을 수 있다. (n개를 생각하더라도, 원한다면 빈 튜플 `()`로도 만들 수 있다).

~~~~
let mytup: (int, int, float) = (10, 20, 30.0);
match mytup {
  (a, b, c) => info!(a + b + (c as int))
}
~~~~

## 튜플 구조체

또한 Rust는 구조체와 튜플로 행동하는 _tuple structs_를 가진다, except that, 튜플과 다르게, 튜플 구조체는 이름을 가지고(그래서 `Foo(1, 2)`는 `Bar(1, 2)`와 서로 다른 타입이다.), 튜플 구조체의 _필드_는 이름을 가지지 않는다.

~~~~
struct MyTup(int, int, float);
let mytup: MyTup = MyTup(10, 20, 30.0);
match mytup {
  MyTup(a, b, c) => info!(a + b + (c as int))
}
~~~~

<a name="newtype"></a>

때때로 "newtypes"라고 불리는 하나의 필드를 가지는 튜플 구조체를 위한 특수한 경우가 있다.  (Haskell의 "newtype" 기능에서 영향을 받음). 새로운 이름은 기존 타입의 동의어뿐만 아니라 오히려 자신의 독특한 타입을 가지는 방식으로 새로운 타입을 정의하곤 했다.

~~~~
struct GizmoId(int);
~~~~

편의상, 역참조 (`*`) 단항 연산자를 이용해 구조체의 내용을 추출할 수 있다.

~~~~
# struct GizmoId(int);
let my_gizmo_id: GizmoId = GizmoId(10);
let id_int: int = *my_gizmo_id;
~~~~

이와같은 타입은 동일한 타입을 가지는 데이터 사이를 구분하기 유용할 수 있지만, 다른 방법이 사용되어야 한다.

~~~~
struct Inches(int);
struct Centimeters(int);
~~~~

위의 정의는 프로그램에서 다른 단위에 해당하는 혼란스러운 숫자를 피하도록 허용하는 단순한 방법이다.

# 함수

우리는 이미 몇가지 함수 정의를 보았다. `type`과 같이 다른 모든 정적인 선언처럼, 함수는 최상위와 다른 함수 내부 (또는 모듈 내부, [뒤에서](#modules-and-crates) 살펴볼 것임)에서 선언될 수 있다. `fn` 키워드는 함수를 뜻한다. 함수는 괄호 안에 쉼표로 구분된 `expr: type` 쌍의 인자 목록을 가진다. 화살표 `->`는 인자 목록과 함수의 반환 타입을 구분한다.

~~~~
fn line(a: int, b: int, x: int) -> int {
    return a * x + b;
}
~~~~

`return` 키워드는 함수의 몸통에서 즉시 반환한다. 선택적으로 반환하기 위한 표현식이 따라온다. 함수는 표현식을 생성하는 함수의 최상위 블록을 가짐으로써 값을 반환할 수 있다.

~~~~
fn line(a: int, b: int, x: int) -> int {
    a * x + b
}
~~~~

반환값을 적기위한 방식으로는 명식적으로 `return`이라고 적는 것보다 Rust 스타일이 더 낫다. `return`은 함수에서 일찍 반환할때 활용된다. 값을 반환하지 않는 함수는 nil `()`이 반환된다고 하고, 반환 타입과 반환 값 둘다 정의에서 생략될 수 있다. 다음의 두 함수는 동일하다.

~~~~
fn do_nothing_the_hard_way() -> () { return (); }

fn do_nothing_the_easy_way() { }
~~~~

함수의 끝에 있는 세미콜론은 `()`을 반환하는 것과 같다.

~~~~
fn line(a: int, b: int, x: int) -> int { a * x + b  }
fn oops(a: int, b: int, x: int) -> ()  { a * x + b; }

assert!(8 == line(5, 3, 1));
assert!(() == oops(5, 3, 1));
~~~~

`match` 표현식과 `let` 바인딩으로, 함수 인자는 구조파괴 패턴을 지원한다. 튜플로부터 처음 값을 풀고 반환하는 이 예제를 보면 `let`처럼, 인자 패턴은 반박될 수 없다.

~~~
fn first((value, _): (int, float)) -> int { value }
~~~

# 소멸자

*소멸자*는 객체에 더 이상 접근할 수 없을때, 객체에서 사용된 자원을 정리하는 것을 담당하는 함수이다. 소멸자는 파일, 소켓 그리고 힙 메모리와 같은 자원의 해제를 처리하는 내용이 정의될 수 있다.

객체는 자신의 소멸자가 호출된 이후에는 접근할 수 없기 때문에, 해제된 자원에 접근하는 동적인 실패가 없다. 작업이 실패했을 때, 태스크 내부의 모든 객체의 소멸자가 호출된다.

`~`는 힙에서 메모리 할당을 위한 유일한 처리를 나타낸다.

~~~~
{
    // an integer allocated on the heap
    let y = ~10;
}
// 소멸자는 `y`가 범위를 벗어나자마자 힙 메모리를 해제한다.
~~~~

Rust는 언어 차원에서 힙 메모리 할당을 위한 문법이 흔하게 사용되기 때문에 포함하지만, 동일한 의미로 타입에 따른 맞춤 소멸자가 구현될 수 있다.

# 소유권

Rust는 변수나 태스크에 국한된 가비지 컬렉터에 대한 객체의 생명주기의 관리를 위임하는 객체 소유권의 개념을 형식화한다. 객체의 소유자는 소멸자를 호출하여 객체의 생명주기를 관리하고, 객체가 변할 수 있는지 판단할 책임이 있다.

소유권은 재귀적이기 때문에, 변화성은 재귀적으로 상속되고 소멸자는 소유된 객체 트리가 가지고 있는 것들을 파괴한다. 변수는 최상위 소유자이고 포함된 객체가 스코프를 벗어낫을 때 파괴된다. 가비지 컬렉터에 의해 관리되는 박스는 새로운 소유권 트리를 시작하고, 소멸자는 수집될 때 호출된다.

~~~~
// 구조체는 `x`와 `y` 필드에 포함된 객체를 소유한다.
struct Foo { x: int, y: ~int }

{
    // `a`는 구조체의 소유자이고, 그래서 구조체의 필드의 소유자이다.
    let a = Foo { x: 5, y: ~10 };
}
// `a`가 스코프를 벗어나면, 구조체 필드의 `~int` 소멸자가 호출된다.

// `b`는 값이 변경될 수 있고, 값의 변경성은 객체의 소유권에 따라 상속된다.
let mut b = Foo { x: 5, y: ~10 };
b.x = 10;
~~~~

객체가 쓰레기가 수집된 박스를 가지지 않으면, 객체는 하나의 소유권 트리로 구성되고 태스크 사이로 전달되는 것을 허용하는 `Owned` 특징이 주어진다. 맞춤 소멸자는 `Owned` 타입에만 직접적으로 구현될 수 있으나, 쓰레기 수집된 박스는 맞춤 소멸자를 갖는 타입을 여전히 *포함*할 수 있다.

# 박스

기본적으로 많은 현대의 언어들은 값을 힙 메모리를 가리키는 포인터로 나타낸다. 대조적으로, Rust는 C와 C++처럼 직접적으로 해당 타입을 나타낸다. 달리 말하자면 Rust에서 데이터를 모으는 것은 *unboxed*라고 한다. 이는 `let x = Point { x: 1f, y: 1f };`라고 하면, 스택상에 구조체를 생성함을 뜻한다. 자료 구조로 복사한다면, 포인터가 아닌 구조체 전체를 복사한다.

일반적으로 `Point`와 같은 작은 구조체에서는 메모리에 할당하는 것과 포인터를 통해 간접적으로 가리키는것 보다 더 호율적이다. 그러나 큰 구조체나 변경될 수 있는 상태에서는, 스택이나 힙 상에서 단일 복사하고, 포인터를 통해 가리키는 것이 유용할 수 있다.

## 소유된 박스

소유된 박스(`~`)는 힙 상에서 유일하게 소유된 할당이다. 박스가 없으면 변경가능성과 소유자의 생명주기를 상속받는다.

~~~~
let x = 5; // 변경 불가능
let mut y = 5; // 변경 가능
y += 2;

let x = ~5; // 변경 불가능
let mut y = ~5; // 변경 가능
*y += 2; // * 연산자는 가지고 있는 값에 접근하기 위해 필요하다.
~~~~

소유된 박스의 목적은 재귀적인 자료 구조를 생성하거나 포인터보다 더 큰 객체를 싸게 전달하기 위해 간접 층을 추가하는 것이다. 소유된 box는 유일한 소유주를 갖기 때문에, 트리 자료 구조로만 나타내어질 수 있다.

다음의 구조체는 컴파일되지 않을것이다. 왜냐하면 간접의 누락은 구조체가 무한 크기를 갖는다는 것을 의미할 것이기 때문이다.

~~~~ {.xfail-test}
struct Foo {
    child: Option<Foo>
}
~~~~

> ***노트:*** `Option` 타입은 *선택적* 값을 나타내는 열거형이다. 이는 많은 다른 언어에서의 null이 가능한 포인터와 비교되지만, contained value unboxed 를 저장한다.

간접이 포함된 소유된 포인터는 힙 상에서 구조체의 외부에 자식을 할당한다, which 유한한 크기를 가지고 컴파일 타임에 오류가 발생하지 않는:

~~~~
struct Foo {
    child: Option<~Foo>
}
~~~~

## 관리되는 박스

관리되는 박스(`@`)는 태스크 로컬 쓰레기 수집기에 의해 관리되는 생명주기를 포함한 힙 할당이다. 태스크가 끝나기 전에 박스로 남은 참조가 없으면 어떤 포인터는 소멸될 것이다. 관리되는 박스는 소유자가 없고, 그래서 새로운 소유권 트리를 시작하고 변경가능성을 상속하지 않는다. 관리되는 박스는 포함된 객체를 소유하고, 변경가능성은 관리되는 박스(`@` 또는 `@mut`)의 타입에 의해 정의된다. 관리되는 박스에 포함된 객체는 `Owned`가 아니고, 태스크 간에 전송될 수 없다.

~~~~
let a = @5; // 변경 불가

let mut b = @5; // 변경 가능한 변수, 변경 불가능한 박스
b = @10;

let c = @mut 5; // 변경 불가능한 변수, 변경 가능한 박스
*c = 10;

let mut d = @mut 5; // 변경 가능한 변수, 변경 가능한 박스
*d += 5;
d = @mut 15;
~~~~

변경가능한 변수와 변경불가능한 변수는 타입이 호환된다면 같은 박스를 가리킬 수 있다. 박스의 변경가능성은 그 타입의 속성이지만, 가변의 핸들 예제에서 불변의 박스가 가변의 박스로의 참조는 할당될 수 없다.

~~~~
let a = @1;     // 불변 박스
let b = @mut 2; // 가변 박스

let mut c : @int;       // declare a variable with type managed immutable int
let mut d : @mut int;   // and one of type managed mutable int

c = a;          // box type is the same, okay
d = b;          // box type is the same, okay
~~~~

~~~~ {.xfail-test}
// but b cannot be assigned to c, or a to d
c = b;          // error
~~~~

# Move semantics

Rust는 인자 전달, 할당, 함수로부터의 값 반환시 얕은 복사를 사용한다. 복사된 값의 소유권 트리가 소유된 박스나 커스텀 소멸자를 가지는 타입을 포함한다면, 얕은 복사는 소유권의 이동이 고려된다. 값이 이동된 다음, 소스 위치에서 사용될 수 없고 거기서 소멸되지 않을 것이다.

~~~~
let x = ~5;
let y = x.clone(); // y는 새롭게 할당된 박스
let z = x; // 새 메모리가 할당되지 않음, x는 더이상 사용할 수 없음
~~~~

소유된 박스의 변경가능성은 소유권의 특성이기 때문에, 박스가 아닌, 그것이 이동되면 변경가능한 박스는 변경불가능이 될 것이고 그 반대도 동일하다.

~~~~
let r = ~13;
let mut s = r; // 박스는 변경가능이 된다.
*s += 1;
let t = s; // 박스는 변경불가능이 된다.
~~~~

# 빌린 포인터

Rust의 빌린 포인터는 범용의 참조 타입이다. 소유된 박스와는 대조적으로, 소유된 박스의 홀더는 메모리 참조의 소유자이다. 빌린 포인터는 암묵적인 소유권이 절대 아니다. 포인터는 어떤 객체로든 빌려질 수 있고, 컴파일러는 객체의 생명주기보다 오래 살 수 없다는 것을 검증한다.

예를 들어, 단순한 구조체 타입(`Point`)을 살펴보자.

~~~
struct Point {
    x: float,
    y: float
}
~~~~

점들을 다양한 방식으로 할당하기 위해 단순한 정의를 이용할 수 있다.
예를 들어, 코드에서, 각각의 세개의 지역 변수는 점을 포함할 수 있지만, 다른 위치에 할당되었다.

~~~
# struct Point { x: float, y: float }
let on_the_stack : Point  =  Point { x: 3.0, y: 4.0 };
let managed_box  : @Point = @Point { x: 5.0, y: 1.0 };
let owned_box    : ~Point = ~Point { x: 7.0, y: 9.0 };
~~~

어느 두 점 사이의 거리를 계산하는 절차를 작성하기를 원한다고 가정하면, 그것들이 저장되는 곳은 중요하지 않다.
예를 들어, `on_the_stack`과 `managed_box` 사이나, `managed_box`과 `owned_box` 사이의 거리를 계산하는 것을 좋아할 것이다. 한가지 옵션은 점 타입의 두 인자를 취하는 함수를 정의하는 것이다.
즉, 점들을 값으로 취하는 것이다. 그러나 이것은 함수를 호출할 때 점들이 복사될 것이다.
점들의 입장에서는, 이것은 아마도 그리 나쁘진 않지만, 자주 복사하는 것은 비싸다.
그러므로 점들을 포인터로 취하는 함수를 정의할 것이다. 이를 위해 빌린 포인터를 사용할 수 있다.

~~~
# struct Point { x: float, y: float }
# fn sqrt(f: float) -> float { 0f }
fn compute_distance(p1: &Point, p2: &Point) -> float {
    let x_d = p1.x - p2.x;
    let y_d = p1.y - p2.y;
    sqrt(x_d * x_d + y_d * y_d)
}
~~~

이제 다양한 방식으로 `compute_distance()`를 호출할 수 있다.

~~~
# struct Point{ x: float, y: float };
# let on_the_stack : Point  =  Point { x: 3.0, y: 4.0 };
# let managed_box  : @Point = @Point { x: 5.0, y: 1.0 };
# let owned_box    : ~Point = ~Point { x: 7.0, y: 9.0 };
# fn compute_distance(p1: &Point, p2: &Point) -> float { 0f }
compute_distance(&on_the_stack, managed_box);
compute_distance(managed_box, owned_box);
~~~

여기 `&` 연산자는 `on_the_stack` 변수의 주소를 취하는데 사용된다.
이는 왜냐하면 `on_the_stack`는 `Point` 타입을 가지고있고 (즉, 구조체 값), 값을 얻기위해 그 주소를 취해야한다.
또한 별명으로 만들었기 때문에, 이 _borrowing_ 지역 변수 `on_the_stack`을 호출한다.
즉, 동일한 데이터를 얻는 다른 경로이다.

In the case of the boxes `managed_box` and `owned_box`, however, no
explicit action is necessary. The compiler will automatically convert
a box like `@point` or `~point` to a borrowed pointer like
`&point`. This is another form of borrowing; in this case, the
contents of the managed/owned box are being lent out.

Whenever a value is borrowed, there are some limitations on what you
can do with the original. For example, if the contents of a variable
have been lent out, you cannot send that variable to another task, nor
will you be permitted to take actions that might cause the borrowed
value to be freed or to change its type. This rule should make
intuitive sense: you must wait for a borrowed value to be returned
(that is, for the borrowed pointer to go out of scope) before you can
make full use of it again.

For a more in-depth explanation of borrowed pointers, read the
[borrowed pointer tutorial][borrowtut].

[borrowtut]: tutorial-borrowed-ptr.html

## Freezing

Borrowing an immutable pointer to an object freezes it and prevents mutation.
`Owned` objects have freezing enforced statically at compile-time.

~~~~
let mut x = 5;
{
    let y = &x; // x is now frozen, it cannot be modified
}
// x is now unfrozen again
# x = 3;
~~~~

Mutable managed boxes handle freezing dynamically when any of their contents
are borrowed, and the task will fail if an attempt to modify them is made while
they are frozen:

~~~~
let x = @mut 5;
let y = x;
{
    let z = &*y; // the managed box is now frozen
    // modifying it through x or y will cause a task failure
}
// the box is now unfrozen again
~~~~

# Dereferencing pointers

Rust uses the unary star operator (`*`) to access the contents of a
box or pointer, similarly to C.

~~~
let managed = @10;
let owned = ~20;
let borrowed = &30;

let sum = *managed + *owned + *borrowed;
~~~

Dereferenced mutable pointers may appear on the left hand side of
assignments. Such an assignment modifies the value that the pointer
points to.

~~~
let managed = @mut 10;
let mut owned = ~20;

let mut value = 30;
let borrowed = &mut value;

*managed = *owned + 10;
*owned = *borrowed + 100;
*borrowed = *managed + 1000;
~~~

Pointers have high operator precedence, but lower precedence than the
dot operator used for field and method access. This precedence order
can sometimes make code awkward and parenthesis-filled.

~~~
# struct Point { x: float, y: float }
# enum Shape { Rectangle(Point, Point) }
# impl Shape { fn area(&self) -> int { 0 } }
let start = @Point { x: 10f, y: 20f };
let end = ~Point { x: (*start).x + 100f, y: (*start).y + 100f };
let rect = &Rectangle(*start, *end);
let area = (*rect).area();
~~~

To combat this ugliness the dot operator applies _automatic pointer
dereferencing_ to the receiver (the value on the left-hand side of the
dot), so in most cases, explicitly dereferencing the receiver is not necessary.

~~~
# struct Point { x: float, y: float }
# enum Shape { Rectangle(Point, Point) }
# impl Shape { fn area(&self) -> int { 0 } }
let start = @Point { x: 10f, y: 20f };
let end = ~Point { x: start.x + 100f, y: start.y + 100f };
let rect = &Rectangle(*start, *end);
let area = rect.area();
~~~

You can write an expression that dereferences any number of pointers
automatically. For example, if you feel inclined, you could write
something silly like

~~~
# struct Point { x: float, y: float }
let point = &@~Point { x: 10f, y: 20f };
println(fmt!("%f", point.x));
~~~

The indexing operator (`[]`) also auto-dereferences.

# 벡터와 문자열

벡터는 같은 타입의 어떤 값들을 포함하는 메모리상의 인접한 영역이다.
Rust의 다른 타입들처럼, 벡터는 스택, 로컬힙 또는 교환힙에 저장될 수 있다.
벡터를 가리키는 빌린 포인터는 '슬라이스'라고 불린다.

~~~
# enum Crayon {
#     Almond, AntiqueBrass, Apricot,
#     Aquamarine, Asparagus, AtomicTangerine,
#     BananaMania, Beaver, Bittersweet,
#     Black, BlizzardBlue, Blue
# }
// 고정 크기의 스택 벡터
let stack_crayons: [Crayon, ..3] = [Almond, AntiqueBrass, Apricot];

// 스택에 할당된 벡터를 가리키는 빌린 포인터
let stack_crayons: &[Crayon] = &[Aquamarine, Asparagus, AtomicTangerine];

// 로컬 힙(관리된) 크레용들의 벡터
let local_crayons: @[Crayon] = @[BananaMania, Beaver, Bittersweet];

// 교환 힙(소유된) 크레용들의 벡터
let exchange_crayons: ~[Crayon] = ~[Black, BlizzardBlue, Blue];
~~~

`+` 연산자는 벡터 타입에 적용할 때 접합을 의미한다.

~~~~
# enum Crayon { Almond, AntiqueBrass, Apricot,
#               Aquamarine, Asparagus, AtomicTangerine,
#               BananaMania, Beaver, Bittersweet };
# impl Clone for Crayon {
#     fn clone(&self) -> Crayon {
#         *self
#     }
# }

let my_crayons = ~[Almond, AntiqueBrass, Apricot];
let your_crayons = ~[BananaMania, Beaver, Bittersweet];

// 두 벡터를 합쳐 새로운 하나를 만든다.
let our_crayons = my_crayons + your_crayons;

// .push_all()는 벡터가 변경가능한 슬롯을 가지고 있다면 뒤에 추가할 것이다.
let mut my_crayons = my_crayons;
my_crayons.push_all(your_crayons);
~~~~

> ***Note:*** The above examples of vector addition use owned
> vectors. Some operations on slices and stack vectors are
> not yet well-supported. Owned vectors are often the most
> usable.

Square brackets denote indexing into a vector:

~~~~
# enum Crayon { Almond, AntiqueBrass, Apricot,
#               Aquamarine, Asparagus, AtomicTangerine,
#               BananaMania, Beaver, Bittersweet };
# fn draw_scene(c: Crayon) { }
let crayons: [Crayon, ..3] = [BananaMania, Beaver, Bittersweet];
match crayons[0] {
    Bittersweet => draw_scene(crayons[0]),
    _ => ()
}
~~~~

A vector can be destructured using pattern matching:

~~~~
let numbers: &[int] = &[1, 2, 3];
let score = match numbers {
    [] => 0,
    [a] => a * 10,
    [a, b] => a * 6 + b * 4,
    [a, b, c, ..rest] => a * 5 + b * 3 + c * 2 + rest.len() as int
};
~~~~

The elements of a vector _inherit the mutability of the vector_,
and as such, individual elements may not be reassigned when the
vector lives in an immutable slot.

~~~ {.xfail-test}
# enum Crayon { Almond, AntiqueBrass, Apricot,
#               Aquamarine, Asparagus, AtomicTangerine,
#               BananaMania, Beaver, Bittersweet };
let crayons: ~[Crayon] = ~[BananaMania, Beaver, Bittersweet];

crayons[0] = Apricot; // ERROR: Can't assign to immutable vector
~~~

Moving it into a mutable slot makes the elements assignable.

~~~
# enum Crayon { Almond, AntiqueBrass, Apricot,
#               Aquamarine, Asparagus, AtomicTangerine,
#               BananaMania, Beaver, Bittersweet };
let crayons: ~[Crayon] = ~[BananaMania, Beaver, Bittersweet];

// Put the vector into a mutable slot
let mut mutable_crayons = crayons;

// Now it's mutable to the bone
mutable_crayons[0] = Apricot;
~~~

This is a simple example of Rust's _dual-mode data structures_, also
referred to as _freezing and thawing_.

Strings are implemented with vectors of `u8`, though they have a
distinct type. They support most of the same allocation options as
vectors, though the string literal without a storage sigil (for
example, `"foo"`) is treated differently than a comparable vector
(`[foo]`).  Whereas plain vectors are stack-allocated fixed-length
vectors, plain strings are borrowed pointers to read-only (static)
memory. All strings are immutable.

~~~
// A plain string is a slice to read-only (static) memory
let stack_crayons: &str = "Almond, AntiqueBrass, Apricot";

// The same thing, but with the `&`
let stack_crayons: &str = &"Aquamarine, Asparagus, AtomicTangerine";

// A local heap (managed) string
let local_crayons: @str = @"BananaMania, Beaver, Bittersweet";

// An exchange heap (owned) string
let exchange_crayons: ~str = ~"Black, BlizzardBlue, Blue";
~~~

Both vectors and strings support a number of useful
[methods](#methods), defined in [`std::vec`]
and [`std::str`]. Here are some examples.

[`std::vec`]: std/vec.html
[`std::str`]: std/str.html

~~~
# enum Crayon {
#     Almond, AntiqueBrass, Apricot,
#     Aquamarine, Asparagus, AtomicTangerine,
#     BananaMania, Beaver, Bittersweet
# }
# fn unwrap_crayon(c: Crayon) -> int { 0 }
# fn eat_crayon_wax(i: int) { }
# fn store_crayon_in_nasal_cavity(i: uint, c: Crayon) { }
# fn crayon_to_str(c: Crayon) -> &str { "" }

let crayons = [Almond, AntiqueBrass, Apricot];

// Check the length of the vector
assert!(crayons.len() == 3);
assert!(!crayons.is_empty());

// Iterate over a vector, obtaining a pointer to each element
// (`for` is explained in the container/iterator tutorial)
for crayon in crayons.iter() {
    let delicious_crayon_wax = unwrap_crayon(*crayon);
    eat_crayon_wax(delicious_crayon_wax);
}

// Map vector elements
let crayon_names = crayons.map(|v| crayon_to_str(*v));
let favorite_crayon_name = crayon_names[0];

// Remove whitespace from before and after the string
let new_favorite_crayon_name = favorite_crayon_name.trim();

if favorite_crayon_name.len() > 5 {
   // Create a substring
   println(favorite_crayon_name.slice_chars(0, 5));
}
~~~

# Closures

Named functions, like those we've seen so far, may not refer to local
variables declared outside the function: they do not close over their
environment (sometimes referred to as "capturing" variables in their
environment). For example, you couldn't write the following:

~~~~ {.ignore}
let foo = 10;

fn bar() -> int {
   return foo; // `bar` cannot refer to `foo`
}
~~~~

Rust also supports _closures_, functions that can access variables in
the enclosing scope.

~~~~
fn call_closure_with_ten(b: &fn(int)) { b(10); }

let captured_var = 20;
let closure = |arg| println(fmt!("captured_var=%d, arg=%d", captured_var, arg));

call_closure_with_ten(closure);
~~~~

Closures begin with the argument list between vertical bars and are followed by
a single expression. Remember that a block, `{ <expr1>; <expr2>; ... }`, is
considered a single expression: it evaluates to the result of the last
expression it contains if that expression is not followed by a semicolon,
otherwise the block evaluates to `()`.

The types of the arguments are generally omitted, as is the return type,
because the compiler can almost always infer them. In the rare case where the
compiler needs assistance, though, the arguments and return types may be
annotated.

~~~~
let square = |x: int| -> uint { (x * x) as uint };
~~~~

There are several forms of closure, each with its own role. The most
common, called a _stack closure_, has type `&fn` and can directly
access local variables in the enclosing scope.

~~~~
let mut max = 0;
[1, 2, 3].map(|x| if *x > max { max = *x });
~~~~

Stack closures are very efficient because their environment is
allocated on the call stack and refers by pointer to captured
locals. To ensure that stack closures never outlive the local
variables to which they refer, stack closures are not
first-class. That is, they can only be used in argument position; they
cannot be stored in data structures or returned from
functions. Despite these limitations, stack closures are used
pervasively in Rust code.

## Owned closures

Owned closures, written `~fn` in analogy to the `~` pointer type,
hold on to things that can safely be sent between
processes. They copy the values they close over, much like managed
closures, but they also own them: that is, no other code can access
them. Owned closures are used in concurrent code, particularly
for spawning [tasks][tasks].

## Closure compatibility

Rust closures have a convenient subtyping property: you can pass any kind of
closure (as long as the arguments and return types match) to functions
that expect a `&fn()`. Thus, when writing a higher-order function that
only calls its function argument, and does nothing else with it, you
should almost always declare the type of that argument as `&fn()`. That way,
callers may pass any kind of closure.

~~~~
fn call_twice(f: &fn()) { f(); f(); }
let closure = || { "I'm a closure, and it doesn't matter what type I am"; };
fn function() { "I'm a normal function"; }
call_twice(closure);
call_twice(function);
~~~~

> ***Note:*** Both the syntax and the semantics will be changing
> in small ways. At the moment they can be unsound in some
> scenarios, particularly with non-copyable types.

## Do syntax

The `do` expression provides a way to treat higher-order functions
(functions that take closures as arguments) as control structures.

Consider this function that iterates over a vector of
integers, passing in a pointer to each integer in the vector:

~~~~
fn each(v: &[int], op: &fn(v: &int)) {
   let mut n = 0;
   while n < v.len() {
       op(&v[n]);
       n += 1;
   }
}
~~~~

As a caller, if we use a closure to provide the final operator
argument, we can write it in a way that has a pleasant, block-like
structure.

~~~~
# fn each(v: &[int], op: &fn(v: &int)) { }
# fn do_some_work(i: &int) { }
each([1, 2, 3], |n| {
    do_some_work(n);
});
~~~~

This is such a useful pattern that Rust has a special form of function
call that can be written more like a built-in control structure:

~~~~
# fn each(v: &[int], op: &fn(v: &int)) { }
# fn do_some_work(i: &int) { }
do each([1, 2, 3]) |n| {
    do_some_work(n);
}
~~~~

The call is prefixed with the keyword `do` and, instead of writing the
final closure inside the argument list, it appears outside of the
parentheses, where it looks more like a typical block of
code.

`do` is a convenient way to create tasks with the `task::spawn`
function.  `spawn` has the signature `spawn(fn: ~fn())`. In other
words, it is a function that takes an owned closure that takes no
arguments.

~~~~
use std::task::spawn;

do spawn() || {
    debug!("I'm a task, whatever");
}
~~~~

Look at all those bars and parentheses -- that's two empty argument
lists back to back. Since that is so unsightly, empty argument lists
may be omitted from `do` expressions.

~~~~
use std::task::spawn;

do spawn {
   debug!("Kablam!");
}
~~~~

If you want to see the output of `debug!` statements, you will need to turn on `debug!` logging.
To enable `debug!` logging, set the RUST_LOG environment variable to the name of your crate, which, for a file named `foo.rs`, will be `foo` (e.g., with bash, `export RUST_LOG=foo`).

# 메소드

메소드는 항상 메소드의 수신자의 타입을 가지는 `self`라고 불리는 특별한 인자로 시작한다는 점만 제외하면 함수와 비슷하다.
`self` 인자는 C++과 많은 다른 언어들에서의 `this`와 비슷하다.
메소드는 `my_vec.len()`에서 처럼 점 표기를 이용해 불려진다.

`impl` 키워드로 적혀지는 _Implementations_는 구조체와 열거형을 포함한 대부분의 Rust 타입의 메소드를 정의한다.
예를들어, 우리가 만든 `Shape` 열거형의 `draw` 메소드를 정의해보자.

~~~
# fn draw_circle(p: Point, f: float) { }
# fn draw_rectangle(p: Point, p: Point) { }
struct Point {
    x: float,
    y: float
}

enum Shape {
    Circle(Point, float),
    Rectangle(Point, Point)
}

impl Shape {
    fn draw(&self) {
        match *self {
            Circle(p, f) => draw_circle(p, f),
            Rectangle(p1, p2) => draw_rectangle(p1, p2)
        }
    }
}

let s = Circle(Point { x: 1f, y: 2f }, 3f);
s.draw();
~~~

이 코드는 `Shape`에 하나의 `draw` 메소드를 포함하기 위한 _implementation_을 정의한다.
In most respects the `draw` 메소드는 여타 다른 함수처럼 정의된다, `self`라는 이름을 제외하고.

`self`의 타입은 메소드가 구현되어있는 타입이나 이에 관련된 포인터이다.
인자로서 `self`, `&self`, `@self`, 또는 `~self`으로 적는다.
호출자는 메소드를 호출하기 위해 호환할 수 있는 포인터 타입을 차례로 가져야한다.

~~~
# fn draw_circle(p: Point, f: float) { }
# fn draw_rectangle(p: Point, p: Point) { }
# struct Point { x: float, y: float }
# enum Shape {
#     Circle(Point, float),
#     Rectangle(Point, Point)
# }
impl Shape {
    fn draw_borrowed(&self) { ... }
    fn draw_managed(@self) { ... }
    fn draw_owned(~self) { ... }
    fn draw_value(self) { ... }
}

let s = Circle(Point { x: 1f, y: 2f }, 3f);

(@s).draw_managed();
(~s).draw_owned();
(&s).draw_borrowed();
s.draw_value();
~~~

Methods typically take a borrowed pointer self type,
so the compiler will go to great lengths to convert a callee
to a borrowed pointer.

~~~
# fn draw_circle(p: Point, f: float) { }
# fn draw_rectangle(p: Point, p: Point) { }
# struct Point { x: float, y: float }
# enum Shape {
#     Circle(Point, float),
#     Rectangle(Point, Point)
# }
# impl Shape {
#    fn draw_borrowed(&self) { ... }
#    fn draw_managed(@self) { ... }
#    fn draw_owned(~self) { ... }
#    fn draw_value(self) { ... }
# }
# let s = Circle(Point { x: 1f, y: 2f }, 3f);
// As with typical function arguments, managed and owned pointers
// are automatically converted to borrowed pointers

(@s).draw_borrowed();
(~s).draw_borrowed();

// Unlike typical function arguments, the self value will
// automatically be referenced ...
s.draw_borrowed();

// ... and dereferenced
(& &s).draw_borrowed();

// ... and dereferenced and borrowed
(&@~s).draw_borrowed();
~~~

Implementations may also define standalone (sometimes called "static")
methods. The absence of a `self` parameter distinguishes such methods.
These methods are the preferred way to define constructor functions.

~~~~ {.xfail-test}
impl Circle {
    fn area(&self) -> float { ... }
    fn new(area: float) -> Circle { ... }
}
~~~~

To call such a method, just prefix it with the type name and a double colon:

~~~~
use std::float::consts::pi;
struct Circle { radius: float }
impl Circle {
    fn new(area: float) -> Circle { Circle { radius: (area / pi).sqrt() } }
}
let c = Circle::new(42.5);
~~~~

# Generics

Throughout this tutorial, we've been defining functions that act only
on specific data types. With type parameters we can also define
functions whose arguments have generic types, and which can be invoked
with a variety of types. Consider a generic `map` function, which
takes a function `function` and a vector `vector` and returns a new
vector consisting of the result of applying `function` to each element
of `vector`:

~~~~
fn map<T, U>(vector: &[T], function: &fn(v: &T) -> U) -> ~[U] {
    let mut accumulator = ~[];
    for element in vector.iter() {
        accumulator.push(function(element));
    }
    return accumulator;
}
~~~~

When defined with type parameters, as denoted by `<T, U>`, this
function can be applied to any type of vector, as long as the type of
`function`'s argument and the type of the vector's contents agree with
each other.

Inside a generic function, the names of the type parameters
(capitalized by convention) stand for opaque types. All you can do
with instances of these types is pass them around: you can't apply any
operations to them or pattern-match on them. Note that instances of
generic types are often passed by pointer. For example, the parameter
`function()` is supplied with a pointer to a value of type `T` and not
a value of type `T` itself. This ensures that the function works with
the broadest set of types possible, since some types are expensive or
illegal to copy and pass by value.

Generic `type`, `struct`, and `enum` declarations follow the same pattern:

~~~~
use std::hashmap::HashMap;
type Set<T> = HashMap<T, ()>;

struct Stack<T> {
    elements: ~[T]
}

enum Option<T> {
    Some(T),
    None
}
~~~~

These declarations can be instantiated to valid types like `Set<int>`,
`Stack<int>`, and `Option<int>`.

The last type in that example, `Option`, appears frequently in Rust code.
Because Rust does not have null pointers (except in unsafe code), we need
another way to write a function whose result isn't defined on every possible
combination of arguments of the appropriate types. The usual way is to write
a function that returns `Option<T>` instead of `T`.

~~~~
# struct Point { x: float, y: float }
# enum Shape { Circle(Point, float), Rectangle(Point, Point) }
fn radius(shape: Shape) -> Option<float> {
    match shape {
        Circle(_, radius) => Some(radius),
        Rectangle(*)      => None
    }
}
~~~~

The Rust compiler compiles generic functions very efficiently by
*monomorphizing* them. *Monomorphization* is a fancy name for a simple
idea: generate a separate copy of each generic function at each call site,
a copy that is specialized to the argument
types and can thus be optimized specifically for them. In this
respect, Rust's generics have similar performance characteristics to
C++ templates.

## Traits

Within a generic function the operations available on generic types
are very limited. After all, since the function doesn't know what
types it is operating on, it can't safely modify or query their
values. This is where _traits_ come into play. Traits are Rust's most
powerful tool for writing polymorphic code. Java developers will see
them as similar to Java interfaces, and Haskellers will notice their
similarities to type classes. Rust's traits are a form of *bounded
polymorphism*: a trait is a way of limiting the set of possible types
that a type parameter could refer to.

As motivation, let us consider copying in Rust.
The `clone` method is not defined for all Rust types.
One reason is user-defined destructors:
copying a type that has a destructor
could result in the destructor running multiple times.
Therefore, types with destructors cannot be copied
unless you explicitly implement `Clone` for them.

This complicates handling of generic functions.
If you have a type parameter `T`, can you copy values of that type?
In Rust, you can't,
and if you try to run the following code the compiler will complain.

~~~~ {.xfail-test}
// This does not compile
fn head_bad<T>(v: &[T]) -> T {
    v[0] // error: copying a non-copyable value
}
~~~~

However, we can tell the compiler
that the `head` function is only for copyable types:
that is, those that implement the `Clone` trait.
In that case,
we can explicitly create a second copy of the value we are returning
using the `clone` keyword:

~~~~
// This does
fn head<T: Clone>(v: &[T]) -> T {
    v[0].clone()
}
~~~~

This says that we can call `head` on any type `T`
as long as that type implements the `Clone` trait.
When instantiating a generic function,
you can only instantiate it with types
that implement the correct trait,
so you could not apply `head` to a type
that does not implement `Clone`.

While most traits can be defined and implemented by user code,
three traits are automatically derived and implemented
for all applicable types by the compiler,
and may not be overridden:

* `Send` - Sendable types.
Types are sendable
unless they contain managed boxes, managed closures, or borrowed pointers.

* `Freeze` - Constant (immutable) types.
These are types that do not contain anything intrinsically mutable.
Intrinsically mutable values include `@mut`
and `Cell` in the standard library.

* `'static` - Non-borrowed types.
These are types that do not contain any data whose lifetime is bound to
a particular stack frame. These are types that do not contain any
borrowed pointers, or types where the only contained borrowed pointers
have the `'static` lifetime.

> ***Note:*** These two traits were referred to as 'kinds' in earlier
> iterations of the language, and often still are.

Additionally, the `Drop` trait is used to define destructors. This
trait defines one method called `drop`, which is automatically
called when a value of the type that implements this trait is
destroyed, either because the value went out of scope or because the
garbage collector reclaimed it.

~~~
struct TimeBomb {
    explosivity: uint
}

impl Drop for TimeBomb {
    fn drop(&mut self) {
        for _ in range(0, self.explosivity) {
            println("blam!");
        }
    }
}
~~~

It is illegal to call `drop` directly. Only code inserted by the compiler
may call it.

## Declaring and implementing traits

A trait consists of a set of methods without bodies,
or may be empty, as is the case with `Send` and `Freeze`.
For example, we could declare the trait
`Printable` for things that can be printed to the console,
with a single method:

~~~~
trait Printable {
    fn print(&self);
}
~~~~

Traits may be implemented for specific types with [impls]. An impl
that implements a trait includes the name of the trait at the start of
the definition, as in the following impls of `Printable` for `int`
and `~str`.

[impls]: #methods

~~~~
# trait Printable { fn print(&self); }
impl Printable for int {
    fn print(&self) { println(fmt!("%d", *self)) }
}

impl Printable for ~str {
    fn print(&self) { println(*self) }
}

# 1.print();
# (~"foo").print();
~~~~

Methods defined in an implementation of a trait may be called just like
any other method, using dot notation, as in `1.print()`. Traits may
themselves contain type parameters. A trait for generalized sequence
types might look like the following:

~~~~
trait Seq<T> {
    fn length(&self) -> uint;
}

impl<T> Seq<T> for ~[T] {
    fn length(&self) -> uint { self.len() }
}
~~~~

The implementation has to explicitly declare the type parameter that
it binds, `T`, before using it to specify its trait type. Rust
requires this declaration because the `impl` could also, for example,
specify an implementation of `Seq<int>`. The trait type (appearing
between `impl` and `for`) *refers* to a type, rather than
defining one.

The type parameters bound by a trait are in scope in each of the
method declarations. So, re-declaring the type parameter
`T` as an explicit type parameter for `len`, in either the trait or
the impl, would be a compile-time error.

Within a trait definition, `Self` is a special type that you can think
of as a type parameter. An implementation of the trait for any given
type `T` replaces the `Self` type parameter with `T`. The following
trait describes types that support an equality operation:

~~~~
// In a trait, `self` refers to the self argument.
// `Self` refers to the type implementing the trait.
trait Eq {
    fn equals(&self, other: &Self) -> bool;
}

// In an impl, `self` refers just to the value of the receiver
impl Eq for int {
    fn equals(&self, other: &int) -> bool { *other == *self }
}
~~~~

Notice that in the trait definition, `equals` takes a
second parameter of type `Self`.
In contrast, in the `impl`, `equals` takes a second parameter of
type `int`, only using `self` as the name of the receiver.

Just as in type implementations, traits can define standalone (static)
methods.  These methods are called by prefixing the method name with the trait
name and a double colon.  The compiler uses type inference to decide which
implementation to use.

~~~~
use std::float::consts::pi;
trait Shape { fn new(area: float) -> Self; }
struct Circle { radius: float }
struct Square { length: float }

impl Shape for Circle {
    fn new(area: float) -> Circle { Circle { radius: (area / pi).sqrt() } }
}
impl Shape for Square {
    fn new(area: float) -> Square { Square { length: (area).sqrt() } }
}

let area = 42.5;
let c: Circle = Shape::new(area);
let s: Square = Shape::new(area);
~~~~

## Bounded type parameters and static method dispatch

Traits give us a language for defining predicates on types, or
abstract properties that types can have. We can use this language to
define _bounds_ on type parameters, so that we can then operate on
generic types.

~~~~
# trait Printable { fn print(&self); }
fn print_all<T: Printable>(printable_things: ~[T]) {
    for thing in printable_things.iter() {
        thing.print();
    }
}
~~~~

Declaring `T` as conforming to the `Printable` trait (as we earlier
did with `Clone`) makes it possible to call methods from that trait
on values of type `T` inside the function. It will also cause a
compile-time error when anyone tries to call `print_all` on an array
whose element type does not have a `Printable` implementation.

Type parameters can have multiple bounds by separating them with `+`,
as in this version of `print_all` that copies elements.

~~~
# trait Printable { fn print(&self); }
fn print_all<T: Printable + Clone>(printable_things: ~[T]) {
    let mut i = 0;
    while i < printable_things.len() {
        let copy_of_thing = printable_things[i].clone();
        copy_of_thing.print();
        i += 1;
    }
}
~~~

Method calls to bounded type parameters are _statically dispatched_,
imposing no more overhead than normal function invocation, so are
the preferred way to use traits polymorphically.

This usage of traits is similar to Haskell type classes.

## Trait objects and dynamic method dispatch

The above allows us to define functions that polymorphically act on
values of a single unknown type that conforms to a given trait.
However, consider this function:

~~~~
# type Circle = int; type Rectangle = int;
# impl Drawable for int { fn draw(&self) {} }
# fn new_circle() -> int { 1 }
trait Drawable { fn draw(&self); }

fn draw_all<T: Drawable>(shapes: ~[T]) {
    for shape in shapes.iter() { shape.draw(); }
}
# let c: Circle = new_circle();
# draw_all(~[c]);
~~~~

You can call that on an array of circles, or an array of rectangles
(assuming those have suitable `Drawable` traits defined), but not on
an array containing both circles and rectangles. When such behavior is
needed, a trait name can alternately be used as a type, called
an _object_.

~~~~
# trait Drawable { fn draw(&self); }
fn draw_all(shapes: &[@Drawable]) {
    for shape in shapes.iter() { shape.draw(); }
}
~~~~

In this example, there is no type parameter. Instead, the `@Drawable`
type denotes any managed box value that implements the `Drawable`
trait. To construct such a value, you use the `as` operator to cast a
value to an object:

~~~~
# type Circle = int; type Rectangle = bool;
# trait Drawable { fn draw(&self); }
# fn new_circle() -> Circle { 1 }
# fn new_rectangle() -> Rectangle { true }
# fn draw_all(shapes: &[@Drawable]) {}

impl Drawable for Circle { fn draw(&self) { ... } }
impl Drawable for Rectangle { fn draw(&self) { ... } }

let c: @Circle = @new_circle();
let r: @Rectangle = @new_rectangle();
draw_all([c as @Drawable, r as @Drawable]);
~~~~

We omit the code for `new_circle` and `new_rectangle`; imagine that
these just return `Circle`s and `Rectangle`s with a default size. Note
that, like strings and vectors, objects have dynamic size and may
only be referred to via one of the pointer types.
Other pointer types work as well.
Casts to traits may only be done with compatible pointers so,
for example, an `@Circle` may not be cast to an `~Drawable`.

~~~
# type Circle = int; type Rectangle = int;
# trait Drawable { fn draw(&self); }
# impl Drawable for int { fn draw(&self) {} }
# fn new_circle() -> int { 1 }
# fn new_rectangle() -> int { 2 }
// A managed object
let boxy: @Drawable = @new_circle() as @Drawable;
// An owned object
let owny: ~Drawable = ~new_circle() as ~Drawable;
// A borrowed object
let stacky: &Drawable = &new_circle() as &Drawable;
~~~

Method calls to trait types are _dynamically dispatched_. Since the
compiler doesn't know specifically which functions to call at compile
time, it uses a lookup table (also known as a vtable or dictionary) to
select the method to call at runtime.

This usage of traits is similar to Java interfaces.

By default, each of the three storage classes for traits enforce a
particular set of built-in kinds that their contents must fulfill in
order to be packaged up in a trait object of that storage class.

* The contents of owned traits (`~Trait`) must fulfill the `Send` bound.
* The contents of managed traits (`@Trait`) must fulfill the `'static` bound.
* The contents of borrowed traits (`&Trait`) are not constrained by any bound.

Consequently, the trait objects themselves automatically fulfill their
respective kind bounds. However, this default behavior can be overridden by
specifying a list of bounds on the trait type, for example, by writing `~Trait:`
(which indicates that the contents of the owned trait need not fulfill any
bounds), or by writing `~Trait:Send+Freeze`, which indicates that in addition
to fulfilling `Send`, contents must also fulfill `Freeze`, and as a consequence,
the trait itself fulfills `Freeze`.

* `~Trait:Send` is equivalent to `~Trait`.
* `@Trait:'static` is equivalent to `@Trait`.
* `&Trait:` is equivalent to `&Trait`.

Builtin kind bounds can also be specified on closure types in the same way (for
example, by writing `fn:Freeze()`), and the default behaviours are the same as
for traits of the same storage class.

## Trait inheritance

We can write a trait declaration that _inherits_ from other traits, called _supertraits_.
Types that implement a trait must also implement its supertraits.
For example,
we can define a `Circle` trait that inherits from `Shape`.

~~~~
trait Shape { fn area(&self) -> float; }
trait Circle : Shape { fn radius(&self) -> float; }
~~~~

Now, we can implement `Circle` on a type only if we also implement `Shape`.

~~~~
use std::float::consts::pi;
# trait Shape { fn area(&self) -> float; }
# trait Circle : Shape { fn radius(&self) -> float; }
# struct Point { x: float, y: float }
# fn square(x: float) -> float { x * x }
struct CircleStruct { center: Point, radius: float }
impl Circle for CircleStruct {
    fn radius(&self) -> float { (self.area() / pi).sqrt() }
}
impl Shape for CircleStruct {
    fn area(&self) -> float { pi * square(self.radius) }
}
~~~~

Notice that methods of `Circle` can call methods on `Shape`, as our
`radius` implementation calls the `area` method.
This is a silly way to compute the radius of a circle
(since we could just return the `radius` field), but you get the idea.

In type-parameterized functions,
methods of the supertrait may be called on values of subtrait-bound type parameters.
Refering to the previous example of `trait Circle : Shape`:

~~~
# trait Shape { fn area(&self) -> float; }
# trait Circle : Shape { fn radius(&self) -> float; }
fn radius_times_area<T: Circle>(c: T) -> float {
    // `c` is both a Circle and a Shape
    c.radius() * c.area()
}
~~~

Likewise, supertrait methods may also be called on trait objects.

~~~ {.xfail-test}
use std::float::consts::pi;
# trait Shape { fn area(&self) -> float; }
# trait Circle : Shape { fn radius(&self) -> float; }
# struct Point { x: float, y: float }
# struct CircleStruct { center: Point, radius: float }
# impl Circle for CircleStruct { fn radius(&self) -> float { (self.area() / pi).sqrt() } }
# impl Shape for CircleStruct { fn area(&self) -> float { pi * square(self.radius) } }

let concrete = @CircleStruct{center:Point{x:3f,y:4f},radius:5f};
let mycircle: @Circle = concrete as @Circle;
let nonsense = mycircle.radius() * mycircle.area();
~~~

> ***Note:*** Trait inheritance does not actually work with objects yet

## Deriving implementations for traits

A small number of traits in `std` and `extra` can have implementations
that can be automatically derived. These instances are specified by
placing the `deriving` attribute on a data type declaration. For
example, the following will mean that `Circle` has an implementation
for `Eq` and can be used with the equality operators, and that a value
of type `ABC` can be randomly generated and converted to a string:

~~~
#[deriving(Eq)]
struct Circle { radius: float }

#[deriving(Rand, ToStr)]
enum ABC { A, B, C }
~~~

The full list of derivable traits is `Eq`, `TotalEq`, `Ord`,
`TotalOrd`, `Encodable` `Decodable`, `Clone`, `DeepClone`,
`IterBytes`, `Rand`, `Default`, `Zero`, and `ToStr`.

# Crates and the module system

Rust's module system is very powerful, but because of that also somewhat complex.
Nevertheless, this section will try to explain every important aspect of it.

## Crates

In order to speak about the module system, we first need to define the medium it exists in:

Let's say you've written a program or a library, compiled it, and got the resulting binary.
In Rust, the content of all source code that the compiler directly had to compile in order to end up with
that binary is collectively called a 'crate'.

For example, for a simple hello world program your crate only consists of this code:

~~~~
// main.rs
fn main() {
    println("Hello world!");
}
~~~~

A crate is also the unit of independent compilation in Rust: `rustc` always compiles a single crate at a time,
from which it produces either a library or an executable.

Note that merely using an already compiled library in your code does not make it part of your crate.

## The module hierarchy

For every crate, all the code in it is arranged in a hierarchy of modules starting with a single
root module. That root module is called the 'crate root'.

All modules in a crate below the crate root are declared with the `mod` keyword:

~~~~
// This is the crate root

mod farm {
    // This is the body of module 'farm' declared in the crate root.

    fn chicken() { println("cluck cluck"); }
    fn cow() { println("mooo"); }

    mod barn {
        // Body of module 'barn'

        fn hay() { println("..."); }
    }
}

fn main() {
    println("Hello farm!");
}
~~~~

As you can see, your module hierarchy is now three modules deep: There is the crate root, which contains your `main()`
function, and the module `farm`. The module `farm` also contains two functions and a third module `barn`,
which contains a function `hay`.

(In case you already stumbled over `extern mod`: It isn't directly related to a bare `mod`, we'll get to it later. )

## Paths and visibility

We've now defined a nice module hierarchy. But how do we access the items in it from our `main` function?
One way to do it is to simply fully qualifying it:

~~~~ {.xfail-test}
mod farm {
    fn chicken() { println("cluck cluck"); }
    // ...
}

fn main() {
    println("Hello chicken!");

    ::farm::chicken(); // Won't compile yet, see further down
}
~~~~

The `::farm::chicken` construct is what we call a 'path'.

Because it's starting with a `::`, it's also a 'global path',
which qualifies an item by its full path in the module hierarchy
relative to the crate root.

If the path were to start with a regular identifier, like `farm::chicken`, it would be
a 'local path' instead. We'll get to them later.

Now, if you actually tried to compile this code example, you'll notice
that you get a `unresolved name: 'farm::chicken'` error. That's because per default,
items (`fn`, `struct`, `static`, `mod`, ...) are only visible inside the module
they are defined in.

To make them visible outside their containing modules, you need to mark them _public_ with `pub`:

~~~~
mod farm {
    pub fn chicken() { println("cluck cluck"); }
    pub fn cow() { println("mooo"); }
    // ...
}

fn main() {
    println("Hello chicken!");
    ::farm::chicken(); // This compiles now
}
~~~~

Visibility restrictions in Rust exist only at module boundaries. This
is quite different from most object-oriented languages that also
enforce restrictions on objects themselves. That's not to say that
Rust doesn't support encapsulation: both struct fields and methods can
be private. But this encapsulation is at the module level, not the
struct level.

For convenience, fields are _public_ by default, and can be made _private_ with the `priv` keyword:

~~~
mod farm {
# pub type Chicken = int;
# struct Human(int);
# impl Human { fn rest(&self) { } }
# pub fn make_me_a_farm() -> Farm { Farm { chickens: ~[], farmer: Human(0) } }
    pub struct Farm {
        priv chickens: ~[Chicken],
        farmer: Human
    }

    impl Farm {
        fn feed_chickens(&self) { ... }
        pub fn add_chicken(&self, c: Chicken) { ... }
    }

    pub fn feed_animals(farm: &Farm) {
        farm.feed_chickens();
    }
}

fn main() {
    let f = make_me_a_farm();
    f.add_chicken(make_me_a_chicken());
    farm::feed_animals(&f);
    f.farmer.rest();

    // This wouldn't compile because both are private:
    // f.feed_chickens();
    // let chicken_counter = f.chickens.len();
}
# fn make_me_a_farm() -> farm::Farm { farm::make_me_a_farm() }
# fn make_me_a_chicken() -> farm::Chicken { 0 }
~~~

> ***Note:*** Visibility rules are currently buggy and not fully defined, you might have to add or remove `pub` along a path until it works.

## Files and modules

One important aspect about Rusts module system is that source files are not important:
You define a module hierarchy, populate it with all your definitions, define visibility,
maybe put in a `fn main()`, and that's it: No need to think about source files.

The only file that's relevant is the one that contains the body of your crate root,
and it's only relevant because you have to pass that file to `rustc` to compile your crate.

And in principle, that's all you need: You can write any Rust program as one giant source file that contains your
crate root and everything below it in `mod ... { ... }` declarations.

However, in practice you usually want to split you code up into multiple source files to make it more manageable.
In order to do that, Rust allows you to move the body of any module into it's own source file, which works like this:

If you declare a module without its body, like `mod foo;`, the compiler will look for the
files `foo.rs` and `foo/mod.rs` inside some directory (usually the same as of the source file containing
the `mod foo;`). If it finds either, it uses the content of that file as the body of the module.
If it finds both, that's a compile error.

So, if we want to move the content of `mod farm` into it's own file, it would look like this:

~~~~ {.ignore}
// main.rs - contains body of the crate root
mod farm; // Compiler will look for 'farm.rs' and 'farm/mod.rs'

fn main() {
    println("Hello farm!");
    ::farm::cow();
}
~~~~

~~~~
// farm.rs - contains body of module 'farm' in the crate root
pub fn chicken() { println("cluck cluck"); }
pub fn cow() { println("mooo"); }

pub mod barn {
    pub fn hay() { println("..."); }
}
# fn main() { }
~~~~

In short, `mod foo;` is just syntactic sugar for `mod foo { /* content of <...>/foo.rs or <...>/foo/mod.rs */ }`.

This also means that having two or more identical `mod foo;` somewhere
in your crate hierarchy is generally a bad idea,
just like copy-and-paste-ing a module into two or more places is one.
Both will result in duplicate and mutually incompatible definitions.

The directory the compiler looks in for those two files is determined by starting with
the same directory as the source file that contains the `mod foo;` declaration, and concatenating to that a
path equivalent to the relative path of all nested `mod { ... }` declarations the `mod foo;`
is contained in, if any.

For example, given a file with this module body:

~~~ {.ignore}
// src/main.rs
mod plants;
mod animals {
    mod fish;
    mod mammals {
        mod humans;
    }
}
~~~

The compiler would then try all these files:

~~~ {.notrust}
src/plants.rs
src/plants/mod.rs

src/animals/fish.rs
src/animals/fish/mod.rs

src/animals/mammals/humans.rs
src/animals/mammals/humans/mod.rs
~~~

Keep in mind that identical module hierachies can still lead to different path lookups
depending on how and where you've moved a module body to its own file.
For example, if we move the `animals` module above into its own file...

~~~ {.ignore}
// src/main.rs
mod plants;
mod animals;
~~~
~~~ {.ignore}
// src/animals.rs or src/animals/mod.rs
mod fish;
mod mammals {
    mod humans;
}
~~~
...then the source files of `mod animals`'s submodules can
either be placed right next to that of its parents, or in a subdirectory if `animals` source file is:

~~~ {.notrust}
src/plants.rs
src/plants/mod.rs

src/animals.rs - if file sits next to that of parent module's:
    src/fish.rs
    src/fish/mod.rs

    src/mammals/humans.rs
    src/mammals/humans/mod.rs

src/animals/mod.rs - if file is in it's own subdirectory:
    src/animals/fish.rs
    src/animals/fish/mod.rs

    src/animals/mammals/humans.rs
    src/animals/mammals/humans/mod.rs

~~~

These rules allow you to have both small modules that only need
to consist of one source file each and can be conveniently placed right next to each other,
and big complicated modules that group the source files of submodules in subdirectories.

If you need to circumvent the defaults, you can also overwrite the path a `mod foo;` would take:

~~~ {.ignore}
#[path="../../area51/alien.rs"]
mod classified;
~~~

## Importing names into the local scope

Always referring to definitions in other modules with their global
path gets old really fast, so Rust has a way to import
them into the local scope of your module: `use`-statements.

They work like this: At the beginning of any module body, `fn` body, or any other block
you can write a list of `use`-statements, consisting of the keyword `use` and a __global path__ to an item
without the `::` prefix. For example, this imports `cow` into the local scope:

~~~
use farm::cow;
# mod farm { pub fn cow() { println("I'm a hidden ninja cow!") } }
# fn main() { cow() }
~~~

The path you give to `use` is per default global, meaning relative to the crate root,
no matter how deep the module hierarchy is, or whether the module body it's written in
is contained in its own file (remember: files are irrelevant).

This is different to other languages, where you often only find a single import construct that combines the semantic
of `mod foo;` and `use`-statements, and which tend to work relative to the source file or use an absolute file path
- Rubys `require` or C/C++'s `#include` come to mind.

However, it's also possible to import things relative to the module of the `use`-statement:
Adding a `super::` in front of the path will start in the parent module,
while adding a `self::` prefix will start in the current module:

~~~
# mod workaround {
# pub fn some_parent_item(){ println("...") }
# mod foo {
use super::some_parent_item;
use self::some_child_module::some_item;
# pub fn bar() { some_parent_item(); some_item() }
# pub mod some_child_module { pub fn some_item() {} }
# }
# }
~~~

Again - relative to the module, not to the file.

Imports are also shadowed by local definitions:
For each name you mention in a module/block, `rust`
will first look at all items that are defined locally,
and only if that results in no match look at items you brought in
scope with corresponding `use` statements.

~~~ {.ignore}
# // XXX: Allow unused import in doc test
use farm::cow;
// ...
# mod farm { pub fn cow() { println("Hidden ninja cow is hidden.") } }
fn cow() { println("Mooo!") }

fn main() {
    cow() // resolves to the locally defined cow() function
}
~~~

To make this behavior more obvious, the rule has been made that `use`-statement always need to be written
before any declaration, like in the example above. This is a purely artificial rule introduced
because people always assumed they shadowed each other based on order, despite the fact that all items in rust are
mutually recursive, order independent definitions.

One odd consequence of that rule is that `use` statements also go in front of any `mod` declaration,
even if they refer to things inside them:

~~~
use farm::cow;
mod farm {
    pub fn cow() { println("Moooooo?") }
}

fn main() { cow() }
~~~

This is what our `farm` example looks like with `use` statements:

~~~~
use farm::chicken;
use farm::cow;
use farm::barn;

mod farm {
    pub fn chicken() { println("cluck cluck"); }
    pub fn cow() { println("mooo"); }

    pub mod barn {
        pub fn hay() { println("..."); }
    }
}

fn main() {
    println("Hello farm!");

    // Can now refer to those names directly:
    chicken();
    cow();
    barn::hay();
}
~~~~

And here an example with multiple files:
~~~{.ignore}
// a.rs - crate root
use b::foo;
mod b;
fn main() { foo(); }
~~~
~~~{.ignore}
// b.rs
use b::c::bar;
pub mod c;
pub fn foo() { bar(); }
~~~
~~~
// c.rs
pub fn bar() { println("Baz!"); }
~~~

There also exist two short forms for importing multiple names at once:

1. Explicit mention multiple names as the last element of an `use` path:
~~~
use farm::{chicken, cow};
# mod farm {
#     pub fn cow() { println("Did I already mention how hidden and ninja I am?") }
#     pub fn chicken() { println("I'm Bat-chicken, guardian of the hidden tutorial code.") }
# }
# fn main() { cow(); chicken() }
~~~

2. Import everything in a module with a wildcard:
~~~
use farm::*;
# mod farm {
#     pub fn cow() { println("Bat-chicken? What a stupid name!") }
#     pub fn chicken() { println("Says the 'hidden ninja' cow.") }
# }
# fn main() { cow(); chicken() }
~~~

However, that's not all. You can also rename an item while you're bringing it into scope:

~~~
use egg_layer = farm::chicken;
# mod farm { pub fn chicken() { println("Laying eggs is fun!")  } }
// ...

fn main() {
    egg_layer();
}
~~~

In general, `use` creates an local alias:
An alternate path and a possibly different name to access the same item,
without touching the original, and with both being interchangeable.

## Reexporting names

It is also possible to reexport items to be accessible under your module.

For that, you write `pub use`:

~~~
mod farm {
    pub use self::barn::hay;

    pub fn chicken() { println("cluck cluck"); }
    pub fn cow() { println("mooo"); }

    mod barn {
        pub fn hay() { println("..."); }
    }
}

fn main() {
    farm::chicken();
    farm::cow();
    farm::hay();
}
~~~

Just like in normal `use` statements, the exported names
merely represent an alias to the same thing and can also be renamed.

The above example also demonstrate what you can use `pub use` for:
The nested `barn` module is private, but the `pub use` allows users
of the module `farm` to access a function from `barn` without needing
to know that `barn` exists.

In other words, you can use them to decouple an public api from their internal implementation.

## Using libraries

So far we've only talked about how to define and structure your own crate.

However, most code out there will want to use preexisting libraries,
as there really is no reason to start from scratch each time you start a new project.

In Rust terminology, we need a way to refer to other crates.

For that, Rust offers you the `extern mod` declaration:

~~~
extern mod extra;
// extra ships with Rust, you'll find more details further down.

fn main() {
    // The rational number '1/2':
    let one_half = ::extra::rational::Ratio::new(1, 2);
}
~~~

Despite its name, `extern mod` is a distinct construct from regular `mod` declarations:
A statement of the form `extern mod foo;` will cause `rustc` to search for the crate `foo`,
and if it finds a matching binary it lets you use it from inside your crate.

The effect it has on your module hierarchy mirrors aspects of both `mod` and `use`:

- Like `mod`, it causes `rustc` to actually emit code:
  The linkage information the binary needs to use the library `foo`.

- But like `use`, all `extern mod` statements that refer to the same library are interchangeable,
  as each one really just presents an alias to an external module (the crate root of the library your linking against).

Remember how `use`-statements have to go before local declarations because the latter shadows the former?
Well, `extern mod` statements also have their own rules in that regard:
Both `use` and local declarations can shadow them, so the rule is that `extern mod` has to go in front
of both `use` and local declarations.

Which can result in something like this:

~~~
extern mod extra;

use farm::dog;
use extra::rational::Ratio;

mod farm {
    pub fn dog() { println("woof"); }
}

fn main() {
    farm::dog();
    let a_third = Ratio::new(1, 3);
}
~~~

It's a bit weird, but it's the result of shadowing rules that have been set that way because
they model most closely what people expect to shadow.

## Package ids

If you use `extern mod`, per default `rustc` will look for libraries in the the library search path (which you can
extend with the `-L` switch).

However, Rust also ships with rustpkg, a package manager that is able to automatically download and build
libraries if you use it for building your crate. How it works is explained [here][rustpkg],
but for this tutorial it's only important to know that you can optionally annotate an
`extern mod` statement with an package id that rustpkg can use to identify it:

~~~ {.ignore}
extern mod rust = "github.com/mozilla/rust"; // pretend Rust is an simple library
~~~

[rustpkg]: rustpkg.html

## Crate metadata and settings

For every crate you can define a number of metadata items, such as link name, version or author.
You can also toggle settings that have crate-global consequences. Both mechanism
work by providing attributes in the crate root.

For example, Rust uniquely identifies crates by their link metadate, which includes
the link name and the version. It also hashes the filename and the symbols in a binary
based on the link metadata, allowing you to use two different versions of the same library in a crate
without conflict.

Therefor, if you plan to compile your crate as a library, you should annotate it with that information:

~~~~
// lib.rs

# #[crate_type = "lib"];
// Crate linkage metadata
#[link(name = "farm", vers = "2.5")];

// ...
# pub fn farm() {}
~~~~

You can also in turn require in a `extern mod` statement that certain link metadata items match some criteria.
For that, Rust currently parses a comma-separated list of name/value pairs that appear after
it, and ensures that they match the attributes provided in the `link` attribute of a crate file.
This enables you to, eg, pick a a crate based on it's version number, or to link an library under an
different name. For example, this two mod statements would both accept and select the crate define above:

~~~~ {.xfail-test}
extern mod farm(vers = "2.5");
extern mod my_farm(name = "farm", vers = "2.5");
~~~~

Other crate settings and metadata include things like enabling/disabling certain errors or warnings,
or setting the crate type (library or executable) explicitly:

~~~~
// lib.rs
// ...

// This crate is a library ("bin" is the default)
#[crate_type = "lib"];

// Turn on a warning
#[warn(non_camel_case_types)]
# pub fn farm() {}
~~~~

If you're compiling your crate with `rustpkg`,
link annotations will not be necessary, because they get
inferred by `rustpkg` based on the Package id and naming conventions.


> ***Note:*** The rules regarding link metadata, both as attributes and on `extern mod`,
              as well as their interaction with `rustpkg`
              are currently not clearly defined and will likely change in the future.

## A minimal example

Now for something that you can actually compile yourself.

We define two crates, and use one of them as a library in the other.

~~~~
// world.rs
#[link(name = "world", vers = "0.42")];
pub fn explore() -> &'static str { "world" }
~~~~

~~~~ {.xfail-test}
// main.rs
extern mod world;
fn main() { println("hello " + world::explore()); }
~~~~

Now compile and run like this (adjust to your platform if necessary):

~~~~ {.notrust}
> rustc --lib world.rs  # compiles libworld-<HASH>-0.42.so
> rustc main.rs -L .    # compiles main
> ./main
"hello world"
~~~~

Notice that the library produced contains the version in the file name
as well as an inscrutable string of alphanumerics. As explained in the previous paragraph,
these are both part of Rust's library versioning scheme. The alphanumerics are
a hash representing the crates link metadata.

## The standard library and the prelude

While reading the examples in this tutorial, you might have asked yourself where all
those magical predefined items like `println()` are coming from.

The truth is, there's nothing magical about them: They are all defined normally
in the `std` library, which is a crate that ships with Rust.

The only magical thing that happens is that `rustc` automatically inserts this line into your crate root:

~~~ {.ignore}
extern mod std;
~~~

As well as this line into every module body:

~~~ {.ignore}
use std::prelude::*;
~~~

The role of the `prelude` module is to re-exports common definitions from `std`.

This allows you to use common types and functions like `Option<T>` or `println`
without needing to import them. And if you need something from `std` that's not in the prelude,
you just have to import it with an `use` statement.

For example, it re-exports `println` which is defined in `std::io::println`:

~~~
use puts = std::io::println;

fn main() {
    println("println is imported per default.");
    puts("Doesn't hinder you from importing it under an different name yourself.");
    ::std::io::println("Or from not using the automatic import.");
}
~~~

Both auto-insertions can be disabled with an attribute if necessary:

~~~
// In the crate root:
#[no_std];
~~~

~~~
// In any module:
#[no_implicit_prelude];
~~~

## The standard library in detail

The Rust standard library provides runtime features required by the language,
including the task scheduler and memory allocators, as well as library
support for Rust built-in types, platform abstractions, and other commonly
used features.

[`std`] includes modules corresponding to each of the integer types, each of
the floating point types, the [`bool`] type, [tuples], [characters], [strings],
[vectors], [managed boxes], [owned boxes],
and unsafe and borrowed [pointers].  Additionally, `std` provides
some pervasive types ([`option`] and [`result`]),
[task] creation and [communication] primitives,
platform abstractions ([`os`] and [`path`]), basic
I/O abstractions ([`io`]), [containers] like [`hashmap`],
common traits ([`kinds`], [`ops`], [`cmp`], [`num`],
[`to_str`], [`clone`]), and complete bindings to the C standard library ([`libc`]).

The full documentation for `std` can be found here: [standard library].

[standard library]: std/index.html
[`std`]: std/index.html
[`bool`]: std/bool.html
[tuples]: std/tuple.html
[characters]: std/char.html
[strings]: std/str.html
[vectors]: std/vec.html
[managed boxes]: std/managed.html
[owned boxes]: std/owned.html
[pointers]: std/ptr.html
[`option`]: std/option.html
[`result`]: std/result.html
[task]: std/task.html
[communication]: std/comm.html
[`os`]: std/os.html
[`path`]: std/path.html
[`io`]: std/io.html
[containers]: std/container.html
[`hashmap`]: std/hashmap.html
[`kinds`]: std/kinds.html
[`ops`]: std/ops.html
[`cmp`]: std/cmp.html
[`num`]: std/num.html
[`to_str`]: std/to_str.html
[`clone`]: std/clone.html
[`libc`]: std/libc.html

## The extra library

Rust also ships with the [extra library], an accumulation of
useful things, that are however not important enough
to deserve a place in the standard library.
You can use them by linking to `extra` with an `extern mod extra;`.

[extra library]: extra/index.html

Right now `extra` contains those definitions directly, but in the future it will likely just
re-export a bunch of 'officially blessed' crates that get managed with `rustpkg`.

# What next?

Now that you know the essentials, check out any of the additional
tutorials on individual topics.

* [Borrowed pointers][borrow]
* [Tasks and communication][tasks]
* [Macros][macros]
* [The foreign function interface][ffi]
* [Containers and iterators](tutorial-container.html)
* [Error-handling and Conditions](tutorial-conditions.html)
* [Packaging up Rust code][rustpkg]

There is further documentation on the [wiki], however those tend to be even more out of date as this document.

[borrow]: tutorial-borrowed-ptr.html
[tasks]: tutorial-tasks.html
[macros]: tutorial-macros.html
[ffi]: tutorial-ffi.html
[rustpkg]: tutorial-rustpkg.html

[wiki]: https://github.com/mozilla/rust/wiki/Docs

