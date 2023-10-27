# Tips

- [Tips](#tips)
  - [\[c++\] 문자열 파싱 방법](#c-문자열-파싱-방법)
  - [\[git\] submodule이란? submodule의 merge request 절차는?](#git-submodule이란-submodule의-merge-request-절차는)
  - [\[linux\] tcpreplay 사용 시, unable to send packet: message too long 메시지가 발생할 때 해결 방법](#linux-tcpreplay-사용-시-unable-to-send-packet-message-too-long-메시지가-발생할-때-해결-방법)
  - [\[linux\] tmux 사용 방법 정리](#linux-tmux-사용-방법-정리)
  - [\[word\] 집요함이 결과를 만들어낸다.](#word-집요함이-결과를-만들어낸다)
  - [\[linux\] rpm과 yum](#linux-rpm과-yum)
  - [\[linux\] google/sanitizes 를 이용한 동적 메모리 에러 취약점 방어](#linux-googlesanitizes-를-이용한-동적-메모리-에러-취약점-방어)
  - [\[c++\] boost::make\_iterator\_range()를 활용하여 range 객체화하기](#c-boostmake_iterator_range를-활용하여-range-객체화하기)
  - [\[c++\] 템플릿 클래스를 정의할 때 선언부와 구현부를 서로 다른 파일로 분리하는 방법](#c-템플릿-클래스를-정의할-때-선언부와-구현부를-서로-다른-파일로-분리하는-방법)
  - [\[c++\] ODR(One Definition Rule)이란?](#c-odrone-definition-rule이란)
  - [\[c++\] async의 콜백 함수로 클래스의 멤버 함수를 사용하는 경우 주의 사항](#c-async의-콜백-함수로-클래스의-멤버-함수를-사용하는-경우-주의-사항)
  - [\[c++\] dynamic cast 사용 시 주의 사항](#c-dynamic-cast-사용-시-주의-사항)
  - [\[c++\] 리팩토링 후기](#c-리팩토링-후기)
  - [\[c++\] 클래스 전방 선언 완벽 정리](#c-클래스-전방-선언-완벽-정리)
  - [\[git\] git commit 메시지에 템플릿 적용하는 방법](#git-git-commit-메시지에-템플릿-적용하는-방법)
  - [\[네트워크\] tcpdump로 실시간 패킷 흐름 확인하기](#네트워크-tcpdump로-실시간-패킷-흐름-확인하기)
  - [\[linux\] 사설인증서 만들기(서버, 클라이언트)](#linux-사설인증서-만들기서버-클라이언트)
  - [\[c++\] struct tm 변수를 항상 초기화 해야하는 이유](#c-struct-tm-변수를-항상-초기화-해야하는-이유)
  - [\[네트워크\] SO\_REUSERPORT 옵션으로 네트워크 서버 성능 향상 시키기](#네트워크-so_reuserport-옵션으로-네트워크-서버-성능-향상-시키기)
  - [\[c++\] 가변 길이 템플릿 작성 방법](#c-가변-길이-템플릿-작성-방법)
  - [\[c++\] const char\*에 boost::range 적용을 위해서는 boost::as\_literal 함수를 사용하라](#c-const-char에-boostrange-적용을-위해서는-boostas_literal-함수를-사용하라)
  - [\[linux\] 리눅스 공유라이브러리 형식](#linux-리눅스-공유라이브러리-형식)
  - [\[c++\] 공유 라이브러리와 -fPIC 컴파일 옵션](#c-공유-라이브러리와--fpic-컴파일-옵션)
  - [\[네트워크\] iptables](#네트워크-iptables)
  - [\[linux\] GDB 디버깅](#linux-gdb-디버깅)
  - [\[tool\] 아파치 superset](#tool-아파치-superset)
  - [\[개발\] 코드 리뷰 방법](#개발-코드-리뷰-방법)
  - [\[c++\] boost::strand를 사용하는 이유](#c-booststrand를-사용하는-이유)
  - [\[linux\] perf(performance counter for linux) 사용 방법](#linux-perfperformance-counter-for-linux-사용-방법)
  - [\[linux\] iostat 사용 방법](#linux-iostat-사용-방법)
  - [\[linux\] SAR(System Activity Reporter) 활용 방법](#linux-sarsystem-activity-reporter-활용-방법)
  - [\[linux\] SPEC 파일의 Source 경로에 명시된 파일을 다운로드 받는 방법](#linux-spec-파일의-source-경로에-명시된-파일을-다운로드-받는-방법)
  - [\[golang\] golang 소스 코드를 직접 빌드해서 빌드 환경 구축하는 방법](#golang-golang-소스-코드를-직접-빌드해서-빌드-환경-구축하는-방법)

<br>

## [c++] 문자열 파싱 방법
전체 문자열 중 일부가 target 문자열에 매칭되는지 여부를 확인하기 위한 과정 중에 추출된 부분 문자열에 대하여 openssl 라이브러리에서 제공하는 MD5 함수를 호출해야하는 작업이 필요했습니다. 아래 [MD5 함수의 정의](https://www.openssl.org/docs/man1.1.1/man3/MD5.html)를 살펴보면 md5 message digest를 생성할 대상의 시작 지점을 가리키는 포인터(d)와 해당 시작 지점에서부터 얼마나 떨어져 있는지를 나타내는 길이(n), 그리고 생성된 md5 message digest를 저장할 공간을 가리키는 포인터(md)를 함수의 인자로 받고있는 것을 확인할 수 있습니다.
```cpp
// MD5 message digest of the n bytes at d and place it in md
unsigned char *MD5(const unsigned char *d, unsigned long n, unsigned char *md);
```

이처럼 원본 문자열을 변형시키거나 부분 문자열을 위한 임시 공간을 생성하지 않으면서 파싱하는 방법이 있다는 것을 기억해 두면 유용할 것입니다.

<br>

## [git] submodule이란? submodule의 merge request 절차는?
[git 공식 홈페이지](https://git-scm.com/docs/gitsubmodules)에서는 submodule을 아래와 같이 설명하고 있습니다.
submodule은 다른 저장소안에 내장된 저장소입니다. submodule은 자체적인 변경 이력을 가지고 있고, submodule이 포함된 저장소를 superproject라고 합니다.
> A submodule is a repository embedded inside another repository. The submodule has its own history; the repository it is embedded in is called a superproject.

submodule을 포함하는 프로젝트에서 submodule을 수정해야 할 일이 생겼다면..? 수정을 완료한 뒤 merge 요청은 수정한 submodule부터 merge를 완료하고 난 뒤에 해당 submodule을 포함하고 있는 프로젝트에 merge 요청을 수행합니다. 이때 해당 프로젝트의 수정 사항이 submodule뿐이라면 수정한 submodule의 commit 정보만 변경 사항으로 잡힐 것입니다.

<br>

## [linux] tcpreplay 사용 시, unable to send packet: message too long 메시지가 발생할 때 해결 방법
tcpreplay라는 툴을 사용하여 지정된 nic에 특정 패킷을 흘려보내고자 할 때, message too long이라는 에러 메시지가 발생하는 경우에는 MTU(Maximum Transmission Unit) 사이즈를 변경하는 것이 해결책이 될 수 있습니다.
```
sudo ifconfig eth0 mtu 2000
```

<br>

## [linux] tmux 사용 방법 정리
블로그 게시글 참조
* https://chelseafandev.github.io/2021/07/22/tip-how-to-use-tmux-md/

<br>

## [word] 집요함이 결과를 만들어낸다.
집요하게 파고 들어서 생각하다보면 좋은 결과를 도출해낼 때가 많다. 계속해서 왜(Why)를 생각하면서 하나씩 파고 들어보자. 집요하게 반복하는 왜(Why)를 통해서 결과가 도출된 과정이 보인다.

<br>

## [linux] rpm과 yum
* rpm: .rpm 파일을 사용하여 직접 패키지를 설치하기 위한 명령어입니다. 의존성을 갖는 패키지의 경우에는 의존 관계에 맞게 순서대로 패키지를 설치해야하는 번거로움이 있습니다.
* yum: yum은 기본적으로 yum repository로 지정된 저장소에 접근하여 원하는 패키지를 설치할 수 있도록 해주는 명령어 입니다. 의존 패키지까지 자동으로 설치해주기 때문에 매우 편리합니다.
* yum repository: rpm들이 보관되는 저장소입니다. yum을 통한 패키지 설치는 이 저장소에 저장된 rpm을 기준으로 수행됩니다.
  * RPMS/ : 바이너리 RPM에는 소스 및 패치에서 빌드된 바이너리가 포함되어 있습니다.
  * SRPMS/ : SRPM에는 소스 코드와 SPEC 파일이 포함되어 있으며, 바이너리 RPM에 소스 코드를 빌드하는 방법을 설명합니다. 선택적으로 소스 코드에 대한 패치도 포함됩니다.
  * repodata/ : repository에 대한 meta 데이터들이 포함되어 있습니다.
  * RPM-GPG-KEY : 제공하는 패키지에 대한 유효성 검증을 위해 사용되는 공개키 정보입니다.

RPMS/ 디렉토리에 존재하는 rpm들을 yum을 통해 설치하는 경우에는 rpm 파일명에서 해당 패키지의 버전 정보 이전까지의 명칭을 사용하면 됩니다. 예를 들어, rpm 파일명이 `nodejs-6.14.3-1.el7.x86_64.rpm`이라고 한다면 설치 명령어는 아래와 같습니다.
```
yum install nodejs
```

(참고사항1) `el7`의 의미는?
**EL** is short for Red Hat **E**nterprise **L**inux (EL).
* EL6 is the download for Red Hat 6.x, CentOS 6.x, and CloudLinux 6.x.
* EL5 is the download for Red Hat 5.x, CentOS 5.x, CloudLinux 5.x.
* EL7 is the download for Red Hat 7.x, CentOS 7.x, and CloudLinux 7.x.

(참고사항2) `x86_64`의 의미는?
아키텍쳐 종류를 의미합니다.

Red Hat 공식 홈페이지에서 [RPM 패키징 가이드](https://access.redhat.com/documentation/ko-kr/red_hat_enterprise_linux/7/html/rpm_packaging_guide/index)문서를 제공하고 있으니 참고바랍니다.

<br>

## [linux] google/sanitizes 를 이용한 동적 메모리 에러 취약점 방어
어떤 방식으로 메모리 취약점을 동적으로 점검할 수 있을까에 대한 궁금증을 해소하기 위해 [공식 Wiki](https://github.com/google/sanitizers/wiki)를 살펴보도록 하겠습니다.

<br>

## [c++] boost::make_iterator_range()를 활용하여 range 객체화하기
boost::iterator_range() 함수는 begin과 end iterator로부터 range 객체를 생성합니다. range 객체는 대게 begin()과 end()를 메소드 함수로 갖는 container로부터 생성되기 때문에 boost::iterator_range() 함수는 사용자 정의 iterator를 래핑(wrapping)하는데 유용하게 사용됩니다.
> iterator_range() creates a range object from a begin and end iterator. Since ranges are constructed implicitly from containers that have .begin() and .end() methods, it's mostly useful for wrapping custom iterators.

실제로는 template 타입을 자동으로 추론(deduce)해주는 boost::make_iterator_range() 함수를 사용하는 것이 조금 더 편리합니다.
> In practice, it's more convenient to use the make_iterator_range() function that deduces the template type automatically.

```cpp
#include <iostream>
#include <iterator>
#include <sstream>

#include <boost/range.hpp>

int main()
{
    std::stringstream ss("The quick brown fox jumps over the lazy dog.");
    auto begin = std::istream_iterator<std::string>(ss);
    auto end = std::istream_iterator<std::string>();

    // iterator_range
    auto range = boost::iterator_range<decltype(begin)>(begin, end);
    for (const auto & word : range) {
        std::cout << "[" << word << "] ";
    }
    std::cout << std::endl;

    // make_iterator_range 함수를 사용하는 경우에는 template type을 자동으로 추론(deduce)해준다.
    for (const auto & word : boost::make_iterator_range(begin, end)) {
        std::cout << "[" << word << "] ";
    }
    std::cout << std::endl;

    return 0;
}
```
```
[The] [quick] [brown] [fox] [jumps] [over] [the] [lazy] [dog.] 
[The] [quick] [brown] [fox] [jumps] [over] [the] [lazy] [dog.]
```

<br>

## [c++] 템플릿 클래스를 정의할 때 선언부와 구현부를 서로 다른 파일로 분리하는 방법
헤더 파일의 맨 하단에 템플릿 클래스를 구현한 파일을 include 시켜주면 된다. (#include는 대상이되는 파일을 단순히 붙여 넣어주는 역할을 하므로)
```cpp
#ifndef MYTEMPLATECLASS_H
#define MYTEMPLATECLASS_H

namespace ggultip
{
	template <typename T>
	class mytemplateclass
	{
	public:
		mytemplateclass();
		~mytemplateclass();
		
		...
	};
}

#endif

#include "mytemplateclass.impl.h"
```

```cpp
#pragma once

namespace ggultip
{
	template <typename T>
	mytemplateclass<T>::mytemplateclass()
	{
		...
	}
	
	template <typename T>
	mytemplateclass<T>::~mytemplateclass()
	{
		...
	}
	
	...
}
```

<br>

## [c++] ODR(One Definition Rule)이란?
* https://learn.microsoft.com/en-us/cpp/cpp/program-and-linkage-cpp?view=msvc-170
* https://modoocode.com/320

C++ 프로그램에서 변수나 함수의 이름와 같은 심볼은 그것들의 생명 주기가 유지되는 범위 내에서는 횟수에 관계없이 선언(declaration)될 수 있습니다. 하지만 오직 한번만 정의(definition) 될 수 있습니다. 이 규칙이 바로 "One Definition Rule(ODR)"입니다.
> In a C++ program, a symbol, for example a variable or function name, can be declared any number of times within its scope. However, it can only be defined once. This rule is the "One Definition Rule" (ODR).

프로그램은 하나 또는 그 이상의 해석 유닛(Translation Unit)으로 구성되어있습니다. 해석 유닛은 구현 파일과 그것에 직/간접적으로 포함된 모든 헤더 파일들로 이루어져있습니다. 보통 구현 파일들은 .cpp나 .cxx와 같은 파일 확장자를 갖습니다. 헤더 파일의 경우에는 .h 또는 .hpp를 파일 확장자로 갖습니다. 각각의 해석 유닛은 컴파일러에 의해 독립적으로 컴파일됩니다. 컴파일이 완료된 이후에, 링커는 컴파일된 해석 유닛들을 하나의 프로그램으로 합치는 역할을 수행합니다. ODR 규칙의 위반은 대게 링커 에러로 나타납니다. 이 링커 에러는 동일한 이름이 하나 이상의 해석 유닛에서 정의(definition)된 경우에 발생합니다.
> A program consists of one or more translation units. A translation unit consists of an implementation file and all the headers that it includes directly or indirectly. Implementation files typically have a file extension of .cpp or .cxx. Header files typically have an extension of .h or .hpp. Each translation unit is compiled independently by the compiler. After the compilation is complete, the linker merges the compiled translation units into a single program. Violations of the ODR rule typically show up as linker errors. Linker errors occur when the same name is defined in more than one translation unit.

각 TU 에 존재하는 모든 변수, 함수, 클래스, enum, 템플릿 등등의 정의(Definition) 은 유일 해야 하고 `inline` 이 아닌 모든 함수의 변수들의 정의는 전체 프로그램에서 유일해야 합니다.

<br>

## [c++] async의 콜백 함수로 클래스의 멤버 함수를 사용하는 경우 주의 사항
* https://stackoverflow.com/questions/57427740/how-to-pass-a-function-and-its-parameters-to-stdasync-inside-a-member-functio

std::async 함수의 인자로 반드시 `this`를 함께 넘겨주어야 합니다.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <future>

using namespace std;

class splitter
{
    public:
    splitter() = default;
    virtual ~splitter() = default;
    bool execute(vector<string> &vstr);
    bool split_files(vector<string> &vstr);
};

bool splitter::split_files(vector<string> &vstr)
{
    for(auto & file : vstr)
    {
        // do something
        cout << file << endl;
    }
    return true;
}

bool splitter::execute(vector<string> &vstr)
{
    // 3번째 인자로 this가 들어가야한다는 것에 주의 하자!
	auto fut = std::async(std::launch::async, &splitter::split_files, this, std::ref(vstr));
    bool good = fut.get();
    return good;
}

int main()
{
    vector<string> filenames {
                                "file1.txt",
                                "file2.txt",
                                "file3.txt"
                             };

    splitter split;
    split.execute(filenames);

    return 0;
}
```

<br>

## [c++] dynamic cast 사용 시 주의 사항
* https://www.geeksforgeeks.org/rtti-run-time-type-information-in-cpp/

dynamic casting 이후에는 반드시 nullptr 체크를 해주자. 그리고 dynamic cast의 경우 비용이 꽤 크기때문에 static cast를 사용할 수 있는 상황이라면 static cast를 우선 하자.

> Using `dynamic_cast`: In an inheritance hierarchy, it is used for downcasting a base class pointer to a child class. On successful casting, it returns a pointer of the converted type and, however, it fails if we try to cast an invalid type such as an object pointer that is not of the type of the desired subclass.

```cpp
#include <iostream>
using namespace std;
 
// initialization of base class
class B {};
 
// initialization of derived class
class D : public B {};
 
// Driver Code
int main()
{
    B* b = new D; // Base class pointer
    D* d = dynamic_cast<D*>(b); // Derived class pointer
    if (d != NULL)
        cout << "works";
    else
        cout << "cannot cast B* to D*";
    getchar(); // to get the next character
    return 0;
}
```

* *업캐스팅*은 static_cast
* *다운캐스팅* dynamic_cast
```cpp
#include <iostream>

template <typename DerivedT>
class Base
{
public:
    Base()
    {
        
    }

    // 반드시 추가!
    virtual ~Base()
    {

    }

    DerivedT *derived()
    {
        // 다운캐스팅
        return static_cast<DerivedT *>(this);
    }

    void print_derived_info()
    {
        derived()->mark(); // 기반클래스의 멤버함수에서 파생클래스의 public 멤버함수를 호출할 수 있다
        derived()->set_idx(100); // 기반클래스의 멤버함수에서 파생클래스가 상속받는 클래스의 public 멤버함수를 호출할 수 있다.
    }
};

template <typename DerivedT>
class AnotherBase
{
public:
    AnotherBase()
    {
        idx_ = 0;
    }

    void set_idx(int new_idx)
    {
        idx_ = new_idx;
        std::cout << "call set_idx(idx: " << idx_ << ")" << std::endl;
    }

private:
    int idx_;
};

template <int kind, template <typename> typename DerivedT>
class Derived 
    : public Base<Derived<kind, DerivedT>>
    , public AnotherBase<Derived<kind, DerivedT>>
{
public:
    typedef Derived<kind, DerivedT> this_type;
	typedef DerivedT<this_type> derived_type;

    Derived(std::string msg)
    {
        mark_ = "i'm ";
        mark_ += msg;
        mark_ += " derived";
    }

    void call_test()
    {
        derived_type::print_derived_info();
    }

    void mark()
    {
        std::cout << mark_ << std::endl;
    }

private:
    std::string mark_;
};

int main()
{
    Derived<100, Base> derived("first");
    Derived<100, Base> derived2("second");

    derived.call_test();
    derived2.call_test();

    // 업캐스팅
    Derived<100, Base>* d = new Derived<100, Base>("third");
    Base<Derived<100, Base>>* b = d;
    
    // 다운캐스팅(static_cast)
    Base<Derived<200, Base>>* b2 = new Derived<200, Base>("forth");
    Derived<200, Base>* d2;
    d2 = static_cast<Derived<200, Base>*>(b2);
    if(d2 != nullptr)
    {
        d2->mark();
    }
    
    // 다운캐스팅(dynamic_cast)
    Base<Derived<200, Base>>* b3 = new Derived<200, Base>("fifth");
    Derived<200, Base>* d3 = dynamic_cast<Derived<200, Base>*>(b3);
    if(d3 != nullptr)
    {
        d3->mark();
    }
    
    return 0;
}
```

<br>

## [c++] 리팩토링 후기
* 불필요한 값의 복사는 피하라(const & 활용)
* 함수를 템플릿 인자로 받아서 처리하도록 하라(템플릿 인자로 사용되는 함수는 람다로 정의)
* 코드의 중복을 최대한 피하라(이를 위해 템플릿을 사용하든, 별도의 함수를 사용하든 하자)
* for문은 range 형태로 작성하면 깔끔하다
* 반복문을 for문(range기반이 깔끔)과 람다를 활용하면 유용하다(동일한 함수와 입력 값을 사용하여 서로 다른 반복 작업을 수행할 수 있다.)
* lambda 내에서 클래스의 멤버 변수에 접근하려면 this를 캡쳐하라([해당 링크](https://stackoverflow.com/questions/30142730/c-lambda-capture-private-class-member) 참조)

```cpp
std::vector<int> container = {1, 2, 3, 4, 5};

template <typename ProcedureT>
bool loop_test(ProcedureT p)
{
	for (auto data : container)
	{
		if (p(data) == true)
		{
			return data;
		}
	}
}

// 호출 시
loop_test(
[](int data) -> bool {
	if(data == 3)
	{
		return true;
	}
	return false;
}
);
```

<br>

## [c++] 클래스 전방 선언 완벽 정리
* https://stackoverflow.com/questions/553682/when-can-i-use-a-forward-declaration

여러분이 컴파일러의 입장에 있다고 생각해봅시다: 타입을 전방 선언하는 경우 컴파일러가 알고있는 전부는 이 타입이 존재한다는 것뿐입니다; 그것의 사이즈나 멤버 또는 메소드에 관한 것은 전혀 알지 못합니다. 이것이 바로 불완전한 타입(incomplete type)이라고 불리는 이유입니다. 컴파일러가 이 타입의 레이아웃을 알아야하기 때문에, 여러분이 이 타입을 멤버 변수로 선언한다거나 혹은 부모 클래스로 사용한다거나 하는 것은 불가능합니다.
> Put yourself in the compiler's position: when you forward declare a type, all the compiler knows is that this type exists; it knows nothing about its size, members, or methods. This is why it's called an *incomplete type*. Therefore, you cannot use the type to declare a member, or a base class, since the compiler would need to know the layout of the type.

아래와 같은 클래스 전방 선언을 가정해보겠습니다.
> Assuming the following forward declaration.

여기 여러분이 할 수 있는것과 할 수 없는 것이 있습니다.
> Here's what you can and cannot do.

<br>

**What you can do with an incomplete type:**
* Declare a member to be a pointer or a reference to the incomplete type:
```cpp
class Foo {
		// 전방 선언한 클래스 타입의 포인터변수와 레퍼런스변수는 사용이 가능함
    X* p;
    X& r;
};
```

* Declare functions or methods which accept/return incomplete types:
```cpp
// 함수나 메소드의 인자와 반환 값에도 전방 선언한 클래스 타입을 사용할 수 있음
void f1(X);
X f2();
```

* Define functions or methods which accept/return pointers/references to the incomplete type (but without using its members):
```cpp
// 함수나 메소드의 인자와 반환값에 전방 선언한 클래스 타입의 포인터와 레퍼런스를 사용할 수 있음
// 단, 불완전한 타입의 멤버 변수 접근이나 멤버 함수의 호출은 불가함
void f3(X*, X&) {}
X& f4() {}
X* f5() {}
```

<br>

**What you cannot do with an incomplete type:**
* Use it as a base class
```cpp
// 부모클래스로 사용이 불가함
class Foo : public X // compiler error!
```

* Use it to declare a member:
```cpp
// (포인터나 레퍼런스가 아닌) 전방 선언한 클래스 타입을 멤버 변수로 사용할 수 없음
class Foo {
    X m; // compiler error!
};
```

* Define functions or methods using this type
```cpp
// (포인터나 레퍼런스가 아닌) 전방 선언한 클래스 타입을 함수나 메소드에 사용할 수 없음
void f1(X x) {} // compiler error!
X f2() {} // compiler error!
```

* Use its methods or fields, in fact trying to dereference a variable with incomplete type
```cpp
// 불완전한 타입의 멤버 변수의 접근이나 멤버 함수의 호출은 불가함
class Foo {
    X* m; // ok!
		
    void method()            
    {
        m->someMethod();      // compiler error!
        int i = m->someField; // compiler error!
    }
};
```

<br>

## [git] git commit 메시지에 템플릿 적용하는 방법 
```bash
$ touch ~/.gitmessage.txt
```

```bash
$ vim ~/.gitmessage.txt
```

```
##### 제목 - 50자 이내로 요약!

### [커밋 타입]: [작업내용]

##### 본문 - 한 줄에 최대 72 글자까지만 입력하기

# 1. 무엇을 수정했는지
# 2. 왜 수정했는지

# 꼬릿말은 아래에 작성: ex) #이슈 번호

#   [커밋 타입]  리스트
#   feat      : 기능 (새로운 기능)
#   fix       : 버그 (버그 수정)
#   refactor  : 리팩토링
#   style     : 스타일 (코드 형식, 세미콜론 추가: 비즈니스 로직에 변경 없음)
#   docs      : 문서 (문서 추가, 수정, 삭제)
#   test      : 테스트 (테스트 코드 추가, 수정, 삭제: 비즈니스 로직에 변경 없음)
#   chore     : 기타 변경사항 (빌드 스크립트 수정 등)
#   post      : 블로그 포스트 추가 (신규 포스트 작성 및 수정)
#   algo      : 알고리즘 문제 풀이 추가 (신규 문제 추가 및 문제 풀이 작성)
# ------------------
#   [체크리스트]
#     제목 첫 글자는 대문자로 작성했나요?
#     제목은 명령문으로 작성했나요?
#     제목 끝에 마침표(.) 금지
#     제목과 본문을 한 줄 띄워 분리하기
#     본문에 여러줄의 메시지를 작성할 땐 "-"로 구분했나요?
# ------------------
```

```bash
$ git config --global commit.template ~/.gitmessage.txt
```

<br>

## [네트워크] tcpdump로 실시간 패킷 흐름 확인하기
`eth0`로 유입되는 패킷 중 `tcp`이며 port 번호가 `5757`인 패킷 흐름 확인

```
tcpdump -nnS -vv -i eth0 tcp port 5757
```
* `nn`: 호스트/서비스명이 아닌 IP주소와 Port번호로 출력
* `S`: tcp 시퀀스 번호에 대해 상대적(relative) 번호가 아닌 절대적(absolute) 번호 출력
* `v, vv, vvv`: 패킷을 헤더부까지 자세히 출력

<br>

`[S]` : SYN

`[S.]` : SYN + ACK

`[.]` : ACK

`[P.]`: PUSH + ACK

`[F.]`: FIN + ACK
![](../resource/image/tcpdump_1.png)
![](../resource/image/tcpdump_2.png)

<br>

## [linux] 사설인증서 만들기(서버, 클라이언트)

서버 및 클라이언트 인증서 설정은 [해당 링크](https://docs.jelastic.com/ssl-for-pgsql/)에 설명이 아주 잘되어있어서 나중에 참고를 위해 번역 해보았다.

**1. PostgreSQL 서버 설정**

1.1. SSL 통신을 위해서 서버의 /var/lib/pgsql/data 경로에 아래 3가지 파일이 추가되어야한다.
- server.key : 서버 개인키
- server.crt : 서버 인증서
- root.crt : 신뢰할 수 있는 루트 인증서

1.2. 먼저, 첫번째 파일인 - (서버) 개인키를 아래 명령어를 통해 생성해보자.
```bash
cd /var/lib/pgsql/data
openssl genrsa -des3 -out server.key 1024
```
```bash
openssl rsa -in server.key -out server.key
```

1.3. 아래 명령어를 통해 생성된 (서버)개인 키의 적절한 권한과 소유권을 설정한다.
```bash
chmod 400 server.key
chown postgres.postgres server.key
```

1.4. 이제 서버 개인키에 기반한 서버 인증서를 생성해야한다.
```bash
openssl req -new -key server.key -days 3650 -out server.crt -x509 -subj '/C=KR/ST=Seoul/L=Seoul/O=Somansa/CN=somansa.com/emailAddress=bluetomorrow@somansa.com'
```

1.5. 우리는 직접 인증서를 서명할 것이므로 생성된 서버 인증서는 신뢰할 수 있는 루트 인증서로도 사용될 수 있다. 적절한 이름으로 서버 인증서를 복사하자.
```bash
cp server.crt root.crt
```

1.6. 이제 당신은 3가지의 인증서 파일을 모두 가지고 있기때문에 SSL을 활성화하고 사용하기 위한 PostgreSQL 데이터베이스 설정을 수행할 수 있다. 먼저 pg_hba.conf 파일을 열어 아래와 같이 수정한다.
```bash
# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD
# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
hostssl all         all         127.0.0.1/32          md5 clientcert=1

# IPv4 remote connections for authenticated users
hostssl all         all         0.0.0.0/0             md5 clientcert=1
```

1.7. 다음은 postgresql.conf 파일을 열어 아래와 같이 수정한다.
```bash
ssl = on
ssl_ca_file = 'root.crt'
```

1.8. 마지막으로 PostgreSQL 서비스를 재시작한다.
```bash
systemctl restart postgresql-11.service
```

**2. 클라이언트 인증서**
2.1. 위에서 작업했던 서버 인증서가 앞으로의 작업에 필요하기 때문에 서버와의 SSH 접속을 유지하고, 생성할 클라이언트의 개인키를 /tmp 경로에 임시 저장한다.
```bash
openssl genrsa -des3 -out /tmp/postgresql.key 1024
openssl rsa -in /tmp/postgresql.key -out /tmp/postgresql.key
```

2.2. 다음으로 클라이언트 인증서를 생성하고 그것을 위에서 생성했던 신뢰할 수 있는 루트 인증서(root.crt)로 서명한다.
```bash
openssl req -new -key /tmp/postgresql.key -out /tmp/postgresql.csr -subj '/C=KR/ST=Seoul/L=Seoul/O=Somansa/CN=postgres'
openssl x509 -req -in /tmp/postgresql.csr -CA root.crt -CAkey server.key -out /tmp/postgresql.crt -CAcreateserial
```

💡 Common Name(/CN=) 은 반드시 pg_hba.conf 파일에서 사용한 USER 이름과 동일한 이름을 사용해야한다.<br>
💡 두번째 명령어에 보이는 root.crt와 server.key 파일은 반드시 전체 경로를 명시해 주어야한다. 위의 경우에는 해당 파일들이 존재하는 경로에서 명령어를 수행했기때문에 이름만 적어도 무방하다.

2.3. postgresql.key, postgresql.crt, root.crt 이 3개의 파일이 준비가 됐다면, 당신의 클라이언트 머신의 .postgresql 폴더에 이 파일들을 이동시켜준다. 또한 좀 더 나은 보안이 필요하다면 postgresql.key 파일의 권한을 400으로 설정할 수 있다.
```bash
chmod 400 postgresql.key
```

💡 PostgreSQL 서버의 /tmp 경로에 생성해둔 인증서 파일들은 반드시 삭제해주자!

<br>

## [c++] struct tm 변수를 항상 초기화 해야하는 이유

struct tm 변수를 초기화하지 않고 tm 구조체의 모든 값을 할당 해주지 않고나서 mktime을 호출하는 경우 에러가 발생(`1`을 반환)할 수 있음. (tm 구조체에 할당되지 않는 값에 쓰레기 값이 들어가는 경우가 발생함👻)

아래와 같이 struct tm 변수들은 반드시 **초기화**해주도록 하자
```cpp
struct tm not_before;
memset(&not_before, 0, sizeof(not_before));
struct tm not_after;
memset(&not_after, 0, sizeof(not_after));
struct tm time_stamp;
memset(&time_stamp, 0, sizeof(time_stamp));
```

<br>

## [네트워크] SO_REUSERPORT 옵션으로 네트워크 서버 성능 향상 시키기
* https://wiki.terzeron.com/OS_%EC%9D%BC%EB%B0%98_%EC%8B%9C%EC%8A%A4%ED%85%9C/OS%EC%99%80_Network_%EC%9D%BC%EB%B0%98/SO_REUSEPORT%EB%A5%BC_%EC%9D%B4%EC%9A%A9%ED%95%9C_%EB%84%A4%ED%8A%B8%EC%9B%8D_%EC%84%9C%EB%B2%84_%EC%84%B1%EB%8A%A5%ED%96%A5%EC%83%81
* https://lwn.net/Articles/542629/

리눅스 커널 3.9에 소켓 옵션 SO_REUSEPORT를 이용한 멋진 기능이 추가되었는데, 바로 같은 호스트(IP주소)의 같은 포트에 여러 개의 리스닝 소켓(서버 소켓)을 연결(bind)할 수 있음. SO_REUSEPORT 옵션을 사용하게 되면 여러 쓰레드가 하나의 소켓을 공유하고 동시에 listener/worker 역할을 분담해서 수행하는 방식으로 `40,000 cps`(connection per second) 정도의 성능까지 얻을 수 있었다고함.

```cpp
int sfd = socket(domain, socktype, 0);
int optval = 1;

setsockopt(sfd, SOL_SOCKET, SO_REUSEPORT, &optval, sizeof(optval));
bind(sfd, (struct sockaddr *) &addr, addrlen);
```

<br>

## [c++] 가변 길이 템플릿 작성 방법
* https://modoocode.com/290

`typename` 뒤에 ... 으로 오는 것을 템플릿 파리미터 팩(parameter pack)이라고 하고 함수의 인자로 `...`이 오는 것을 함수 파라미터 팩이라고 한다.

```
💡 템플릿 파라미터 팩과 함수 파라미터 팩의 차이점
 * 템플릿의 경우 타입 앞에 `...` 이 온다
 * 함수의 경우 타입 뒤에 `...` 가 온다
```

Logger 클래스 구현하면서 가변 길이 템플릿 활용. C++17에 도입된 `Fold Expression`이 없었다면 재귀 형태로 표현했어야했다.
```cpp
lass Logger
{
public:
		...
    template <typename ... Arguments>
    void info(char const* fmt, Arguments ... args);
		...
		
private:
		...
		template<typename ... Arguments>
    std::string format(const char* fmt, Arguments ... args);
		...
};

template<typename ... Arguments>
std::string Logger::format(const char* fmt, Arguments ... args)
{
	try
	{
		// Fold Expression!
		return boost::str((boost::format(fmt) % ... % args));
	}
	catch(boost::io::format_error& fe)
	{
		return std::string(fe.what()) + ": " + fmt;
	}

	return "";
}

template <typename ... Arguments>
void Logger::info(char const* fmt, Arguments ... args)
{
    std::string formatted = "[INFO] ";
    formatted += format(fmt, args...);
    enqueue(formatted);
}
```

<br>

## [c++] const char*에 boost::range 적용을 위해서는 boost::as_literal 함수를 사용하라
* https://www.caichinger.com/boost-range/boost-as_literal.html

```cpp
#include <iostream>
#include <string>
#include <vector>

#include <boost/range.hpp>
// as_literal() and as_array() need explicit imports.
#include <boost/range/as_literal.hpp>
#include <boost/range/as_array.hpp>

using std::cout;
using std::endl;

const char * const csptr = "BOOST";
const char csarr[] = "boost";


void raw_pointer_demo() {
    // Calling Boost Range functions on cstrings doesn't work very well.
    // The first doesn't compile, the second gives size() == 6, as it counts
    // the terminal \0:
    cout << "size(csptr): doesn't compile" /* << boost::size(csptr) */ << endl; 
    cout << "size(csarr): " << boost::size(csarr) << endl;
}

void as_literal_demo() {
    // boost::as_literal() solves both of these problems by converting the
    // cstrings to a string range. The size is 5, as you'd expect from a string.
    cout << "size(as_literal(csptr)): " << boost::size(boost::as_literal(csptr)) << endl;
    cout << "size(as_literal(csarr)): " << boost::size(boost::as_literal(csarr)) << endl;
}

void as_array_demo() {
    // boost::as_array() serves only a documentary purpose.
    // Calling it indicates that the author realizes that the argument is a
    // character array, but explicitly wants to treat it as an array as
    // opposed to a string.
    // Note that the behavior is exactly the same as in raw_pointer_demo().
    cout << "size(as_array(csptr)): doesn't compile"
      /* << boost::size(boost::as_array(csptr)) */ << endl;
    cout << "size(as_array(csarr)): " << boost::size(boost::as_array(csarr)) << endl;
}

int main() {
    cout << "csptr = (const char *)\"boost\"" << endl;
    cout << "csarr = \"boost\"" << endl;

    raw_pointer_demo();
    as_literal_demo();
    as_array_demo();

    return 0;
}
```

<br>

## [linux] 리눅스 공유라이브러리 형식

공유 라이브러리와 관련된 이름은 모두 `3개`

1. real name은 파일 시스템 상에서의 라이브러리 파일의 실제 이름입니다. (libXXX.so.major_version.minor_version)
2. soname은 호환성이 보장되는 논리적 이름으로서 라이브러리 내에 저장됩니다. (실제 이름의 파일을 가리키는 symbolic link를 하나 생성. libXXX.so.major_version)
3. linker name은 링커한테 알려주기 위한 lib 과 확장자를 제외한 이름이 됩니다. (soname에 대한 symbolic link를 생성. libXXX.so)

<br>

## [c++] 공유 라이브러리와 -fPIC 컴파일 옵션
* https://bpsecblog.wordpress.com/2016/05/25/memory_protect_linux_3/

Position-independent code(PIC)란 메모리의 어느 공간에나 위치할 수 있고 수정 없이 실행될 수 있는 **위치 독립 코드**입니다. 즉 이 코드를 사용하는 각 프로세스들은 이 코드를 서로 다른 주소에서 실행할 수 있으며, 실행 시 재배치가 필요 없습니다.

공유 라이브러리를 PIC로 생성하지 않으면 실행할 때 재배치에 시간이 소요된다는 단점과 다른 프로세스와 코드를 공유할 수 없게 될 수 있는 단점이 있기 때문에 통상적으로 공유 라이브러리를 작성할 때 .c 파일을 PIC로 컴파일 하는 것입니다.

> Once the program has been loaded and has started running, all the necessary contents of the binary file have been loaded into the process’s virtual address space. However, most programs also need to run functions from the system libraries, and these library functions must also be loaded. In the simplest case, the necessary library functions are embedded directly in the program’s executable binary file. Such a program is statically linked to its libraries, and statically linked executables can commence running as soon as they are loaded.
> 
> The main disadvantage of static linking is that every program generated must contain copies of exactly the same common system library functions. It is much more efficient, in terms of both physical memory and disk-space usage, to load the system libraries into memory only once. Dynamic linking allows that to happen.
> 
> Linux implements dynamic linking in user mode through a special linker library. Every dynamically linked program contains a small, statically linked function that is called when the program starts. This static function just maps the link library into memory and runs the code that the function contains. The link library determines the dynamic libraries required by the program and the names of the variables and functions needed from those libraries by reading the information contained in sections of the ELF binary. It then maps the libraries into the middle of virtual memory and resolves the references to the symbols contained in those libraries. It does not matter exactly where in memory these shared libraries are mapped: they are compiled into position-independent code (PIC), which can run at any address in memory.

<br>

## [네트워크] iptables
* https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-rg-en-4/s1-iptables-options.html
* https://itwiki.kr/w/%EB%A6%AC%EB%88%85%EC%8A%A4_iptables


방화벽 리스트 보기
```
iptables -t filter -L -xvn
```

* -x : Expands numbers into their exact values. On a busy system, the number of packets and bytes seen by a particular chain or rule may be abbreviated using K
 (thousands), M
 (millions), and G
 (billions) at the end of the number. This option forces the full number to be displayed.
* -v : Displays verbose output, such as the number of packets and bytes each chain has seen, the number of packets and bytes each rule has matched, and which interfaces apply to a particular rule.
* -n : Displays IP addresses and port numbers in numeric format, rather than the default hostname and network service format.

<br>

## [linux] GDB 디버깅
* https://visualgdb.com/gdbreference/commands/
* https://visualgdb.com/gdbreference/commands/set_sysroot
* https://visualgdb.com/gdbreference/commands/set_solib-search-path
* https://www.irya.unam.mx/computo/sites/manuales/fce12/debugger/cl/commandref/gdb_mode/cmd_set_substitu.htm

gdb를 통해 프로그램 실행
```
gdb {프로그램명(경로포함)}
```

브레이크 포인트 등록
```
(gdb) b 소스파일명:라인넘버
```

프로그램 시작
```
(gdb) r {인자를 추가해야한다면 입력}
```

브레이크 포인트에 걸린 후 이어서 진행
```
(gdb) c
```

기본적인 GDB 명령어
```
bt(=backtrace)
p(=print)
set print pretty on(or off)
f(=frame)
up
down
```

print 포맷 종류
```
d decimal
x hex
t binary
f floating point
i instruction
c character
```

현재 프레임 내의 지역 변수 정보 확인
```
(gdb) info locals
```

쓰레드 정보 확인
```
(gdb) info threads
```

쓰레드 진입(info threads 명령어를 통해 출력된 Id값으로 진입할 수 있음)
```
(gdb) thread {id}
```

레지스터 정보 확인
```
(gdb) info registers
```

* rax (eax) : 누산기(accumulator) 레지스터. 산술연산(덧셈, 나눗셈, 곱셈)이나 논리연산을 수행한 반환값이 저장
* rbx (ebx) : 베이스 레지스터
* rcx (ecx) : 카운터 레지스터. 반복 명령어 사용 시 반복 카운터로 사용되는 값을 저장
* rdx (edx) : 데이터 레지스터. 산술연산과 I/O 명령에서 rax(eax)와 함께 사용
* rsi (esi) : source 인덱스 레지스터
* rdi (edi) : destination 인덱스 레지스터
* rbp (ebp) : 베이스 포인터 레지스터. 스택의 시작 지점 주소를 저장
* rsp (esp) : 스택 포인터 레지스터. 스택의 가장 마지막 지점 주소를 저장
* rip : 명령 포인터 레지스터이다. 현재 명령의 위치를 가리킴

어셈블러 덤프
```
(gdb) disas
```

layout
```
layout src
layout split
layout asm
layout reg
```

특정 변수 값의 주소 출력
```
(gdb) p &{변수명}
```

특정 변수의 헥사값 출력
```
(gdb) x/{출력할크기}bx {변수의 주소값}
```

GDB에서 긴 문자열을 줄이지 않고 그대로 출력
```
(gdb) set print elements 0
```

GDB에서 중복된 문자열을 그대로 풀어서 출력
```
(gdb) set print repeats 0
```

Boost::shared_ptr을 사용한다면 GDB 디버깅 시 px와 pn정보를 출력할 수 있음
```cpp
element_type* px;                   // contained pointer
boost::detail::shared_count pn;     // reference count
```

프로세스가 실행 중인 상태에서 특정 쓰레드 ID를 통해 GDB 진입이 가능하며, 이후 `CTRL + C`를 통해 해당 쓰레드의 흐름을 중단하여 Backtrace 확인이 가능함(c 명령어를 통해 실행 재개)
```
gdb -p {gdb로 확인할 쓰레드 ID}
```

```
💡 아래 command들은 gdb 실행 시 -ex 옵션(execute a single GDB command)으로 추가해주면 유용함
```

1. **set sysroot {path}**
Specifies the local directory that contains copies of target libraries in the corresponding subdirectories. This option is useful when debugging with gdbserver.

*Typical use*
This command is useful when debugging remote programs via gdbserver and the libraries on the target machine (running gdbserver) do not match the libraries on the source machine (running gdb). In order to set breakpoints and find source lines that correspond to different code locations GDB needs to access the library files containing symbol information. Copying those libraries to locations under a local directory and specifying its path via **set sysroot** allows GDB find them.

2. **set solib-search-path {path}**
Specifies directories where GDB will search for shared libraries with symbols. This option is useful when debugging with gdbserver.

*Typical use*
This command is useful when debugging remote programs via gdbserver. If the shared library path on the remote computer and the GDB computer is different, GDB won't automatically find the local copy of the library and load its symbols unless the directory containing it is specified in **set solib-search-path**.

3. **set substitute-path {original path} {substitute path}**
Set a substitution rule for finding source files.

*Example*
In this example, the debuggee is compiled in Funct/src/temp/src, but the sources are in Funct/src. The debugger needs a substitute path to find the sources.

The debugger cannot find the sources because there is no sustitute path, so the list command does not display any source code:
```
(gdb) list main
No source file named /site/Funct/src/temp/src/c_sanity.c.
(gdb) show substitute-path
```

The set substitute-path command tells the debugger where to find the sources, so the list command displays the source code:
```
(gdb) set substitute-path '/site/Funct/src/temp/src' '/site/Funct/src'
(gdb) show substitute-path
/site/Funct/src/temp/src  ->  /site/Funct/src
(gdb) list main
16      }
17
18      iab2 (na, sum, ivar, nb)
19          int *na, sum[], *ivar, *nb;
20      {
```

<br>

## [tool] 아파치 superset
* https://superset.apache.org/
* https://github.com/apache/superset
  
Superset is a modern data exploration and data visualization platform. Superset can replace or augment proprietary business intelligence tools for many teams. Superset integrates well with a variety of data sources.

<br>

## [개발] 코드 리뷰 방법
* https://soojin.ro/review/reviewer

코드의 유지 보수성과 가독성에 중점을 두고 코드 리뷰를 수행하자.

**디자인**

코드 리뷰를 할 때 가장 중요한 것은 전체적인 코드의 설계다. 각 부분이 잘 디자인 되어 있는가? 해당 코드 베이스에 추가해야 하나 아니면 다른 라이브러리에 추가해야 하나? 지금 시점에 기능을 추가하는 것이 옳은가?

**기능**

작성자가 의도한 대로 코드가 동작하는가? 코드 동작이 사용자에게 유익한가? “사용자”는 프로그램의 소비자일 수도 있고 코드를 *사용할* 다른 개발자일 수도 있다. 리뷰어는 엣지 케이스도 고려해야 한다. 특히 **UI 변경**이 있는 경우에는 기능이 잘 동작하는지 확인한다.
또한 CL에 **병행 작업(concurrency)**이 있는 경우 교착 상태나 경쟁 상태가 발생하지 않는지 유심히 살펴야 한다. 이런 종류의 문제는 코드를 실행해도 찾기가 쉽지 않기 때문에 고민을 많이 해야하고 만약 위험이 발견되는 경우 병행 작업을 제거하는 편이 낫다.

**복잡도**

코드를 더 간단히 할 수는 없는가? 라인별, 함수별, 클래스별로 모두 따져보라. 복잡한 코드는 이해하는데 오래 걸리고, 사용하고 유지 보수할 때 버그가 발생할 가능성이 높다. **오버엔지니어링** 또한 불필요하게 복잡도를 증가시킨다. 리뷰어는 오버엔지니어링이 발생하지 않도록 각별히 유의하고 미래의 문제는 실제로 발생했을 때 해결하도록 한다.

**테스트**

유닛 테스트, 통합 테스트, E2E 테스트를 작성하도록 한다. [긴급 상황](https://soojin.ro/review/emergencies)이 아닌 일반적인 상황에서는 프로덕션 코드와 테스트 코드가 같은 CL에 들어있어야 한다. 보통 테스트 코드를 테스트하는 코드는 만들지 않기 때문에 테스트가 적합한지 여부는 반드시 사람이 확인해야 한다. 코드가 깨지면 테스트도 실패하는지, 적절한 단위로 나뉘어져 있는지 확인해본다. 테스트 코드도 유지 보수가 필요한 코드이기 때문에 이 또한 불필요하게 복잡하지 않은지 본다.

**이름**

변수, 클래스, 메서드 이름이 명료하게 지어졌는가? 좋은 이름은 대상이 무엇인지, 무얼 하는지 충분히 설명하면서도 읽기 어려울 정도로 길지 않다.

**주석**

주석이 명료하고 도움이 되는가? 정말 필요한 주석만 있는가? 유용한 주석은 코드가 *어떤* 일을 하는지가 아니라 **왜 존재하는지**를 담고 있다. 코드만 봐서는 어떤 일을 하는지 이해가 잘 안 된다면 코드를 더 간단하게 고쳐야한다. 정규 표현식이나 복잡한 알고리즘에 대해서는 예외로 어떤 일을 하는지를 적어도 좋지만 일반적으로는 코드에 담을 수 없는 정보를 주석에 남기는 편이 좋다.

**스타일**

구글에서 쓰이는 메이저 언어들은 모두 스타일 가이드가 있다. 스타일 가이드에 없는 사안은 개인의 선호에 맡기기 때문에 이걸로 코드 리뷰를 지연시키지는 말아야 한다. 그리고 코드 스타일 변경은 별도 CL로 리뷰 받아야 한다. 스타일 변경과 코드 수정을 한꺼번에 하게 되면 머지와 롤백이 힘들어지고 리뷰를 하기도 어렵다.

**문서**

코드에 대응되는 문서를 제대로 최신화 시켰는가? CL의 변경사항이 빌드, 테스트, 배포 방식을 바꾸게 되면 꼭 문서도 함께 업데이트한다.

**모든 줄**

리뷰어의 자격으로 CL의 모든 줄을 리뷰해야 한다. 자동 생성 코드나 규모가 큰 자료구조 같은 경우는 훑어만 봐도 괜찮지만 사람이 작성한 코드는 반드시 주의깊게 살핀다. 만약 코드가 보기 어려워서 리뷰가 더디면 작성자한테 설명을 요구한다. 구글은 훌륭한 개발자를 뽑기 때문에 당신이 이해할 수 없는 코드는 다른 개발자도 이해하지 못한다. 만약 당신이 올바르게 리뷰할 수 없는 특수한 부분(보안, 병렬, 접근성, i18n 등)이 있다면 자격이 있는 리뷰어를 지정해준다.

**문맥**

수정된 일부분만 보지말고 전체적인 문맥을 파악한다. 수정사항이 포함된 함수나 클래스 전체를 살펴라. 수정은 단 몇 줄이었지만 알고보니 100줄짜리 함수에 들어 있을 수도 있다. **절대로 전체 코드 품질을 떨어뜨리는 CL을 승인하지 않는다.** 보통 작은 변화가 모여서 전체 복잡도를 증가시키기 때문에 새로 추가 되는 코드에는 미미한 복잡도도 허용하지 말아라.

**좋은 점**

CL을 리뷰하다가 좋은 점을 발견하면 작성자에게 언급한다. 코드 리뷰를 하다보면 실수만 지적할 때가 있는데 좋은 부분에는 칭찬과 고마움을 아끼지 않는다. 멘토링 측면에서 잘못보다는 잘한 점을 알려주는 것이 더 값지다.

<br>

## [c++] boost::strand를 사용하는 이유
* I/O thread가 1개인 경우(즉, io_context::run() 함수가 호출되는 쓰레드가 1개인 경우)에는 오직 하나의 쓰레드에서만 핸들러가 호출되므로 동기화라는게 필요없음.
* 2개 이상의 I/O thread(즉, io_context::run() 함수가 호출되는 쓰레드가 2개 이상인 경우)를 사용하는 경우에는 핸들러를 동기화하기 위해 명시적인 Lock이 필요함.
* 이때 사용할 수 있는게 boost::asio::strand! 핸들러 내부에서 동기화를 위한 작업을 따로 해줄필요가 없다!

```cpp
#include <iostream>

#include <boost/asio.hpp>
#include <boost/thread.hpp>
#include <boost/thread/detail/thread_group.hpp>

int sum = 0;

class thread_func
{
public:
    thread_func(int idx, boost::asio::io_context& ctx) : idx_(idx), ctx_(ctx)
    {

    }

    void operator()()
    {
        std::ostringstream oss;
        oss << std::this_thread::get_id();
        printf("io_context::run #%d thread [%s]\n", idx_, oss.str().c_str());
        ctx_.run();
    }

private:
    int idx_;
    boost::asio::io_context& ctx_;
};

void make_sum_to_100_million_without_strand(int thread_count)
{
    boost::asio::io_context ctx;

    int loop_count = 0;
    switch (thread_count)
    {
    case 1:
        loop_count = 50000000;
        break;
    case 2:
        loop_count = 25000000;
        break;
    case 4:
        loop_count = 12500000;
        break;
    case 8:
        loop_count = 6250000;
        break;
    case 16:
        loop_count = 3125000;
        break;
    default:
        break;
    }

    // no strand
    for (int i = 0; i < thread_count; i++)
    {
        boost::asio::post(ctx, [&](){
            std::ostringstream oss;
            oss << std::this_thread::get_id();
            printf("hello boost::asio::post in io_context [%s]\n", oss.str().c_str());
            for (int i = 0; i < loop_count; i++)
            {
                sum += 2;
            }
        });
    }
    
    boost::thread_group tg;
    for (int i = 0; i < thread_count; i++)
    {
        tg.create_thread(thread_func(i+1, ctx));
    }
    tg.join_all();

    printf("[make_sum_to_100_million_without_strand, %d thread] sum : %d\n", thread_count, sum);
}

int main()
{
    int thread_count = 2; // 1, 2, 4, 8, 16
    make_sum_to_100_million_without_strand(thread_count)
    return 0;
}
```

실행결과
```
[make_sum_to_100_million_without_strand, 2 thread] sum : 50614948
```
(위 예시의 경우에는 2개의 쓰레드에서) 핸들러 동기화를 수행하지 않는 경우에는 1억이 정상적으로 출력되지 않음을 볼 수 있다.

```cpp
#include <iostream>

#include <boost/asio.hpp>
#include <boost/thread.hpp>
#include <boost/thread/detail/thread_group.hpp>

int sum = 0;

class thread_func
{
public:
    thread_func(int idx, boost::asio::io_context& ctx) : idx_(idx), ctx_(ctx)
    {

    }

    void operator()()
    {
        std::ostringstream oss;
        oss << std::this_thread::get_id();
        printf("io_context::run #%d thread [%s]\n", idx_, oss.str().c_str());
        ctx_.run();
    }

private:
    int idx_;
    boost::asio::io_context& ctx_;
};

void make_sum_to_100_million_with_strand(int thread_count)
{
    boost::asio::io_context ctx;
    boost::asio::io_context::strand strand(ctx);

    int loop_count = 0;
    switch (thread_count)
    {
    case 1:
        loop_count = 50000000;
        break;
    case 2:
        loop_count = 25000000;
        break;
    case 4:
        loop_count = 12500000;
        break;
    case 8:
        loop_count = 6250000;
        break;
    case 16:
        loop_count = 3125000;
        break;
    default:
        break;
    }

    // strand
    for (int i = 0; i < thread_count; i++)
    {
        boost::asio::post(strand.wrap([&](){
            std::ostringstream oss;
            oss << std::this_thread::get_id();
            printf("hello boost::asio::post in io_context [%s]\n", oss.str().c_str());
            for (int i = 0; i < loop_count; i++)
            {
                sum += 2;
            }
        }));
    }
    
    boost::thread_group tg;
    for (int i = 0; i < thread_count; i++)
    {
        tg.create_thread(thread_func(i+1, ctx));
    }
    tg.join_all();

    printf("[make_sum_to_100_million_with_strand, %d thread] sum : %d\n", thread_count, sum);
}

int main()
{
    int thread_count = 16; // 1, 2, 4, 8, 16
    make_sum_to_100_million_with_strand(thread_count);
    return 0;
}
```

실행결과
```
[make_sum_to_100_million_with_strand, 16 thread] sum : 100000000
```
(위 예시의 경우에는 16개의 쓰레드에서) strand를 사용하여 핸들러 동기화를 수행하였기때문에 1억이 정상적으로 출력됨을 확인할 수 있다.

<br>

## [linux] perf(performance counter for linux) 사용 방법
* https://velog.io/@mythos/Linux-Tutorial-11-%EC%BB%A4%EB%84%90-%EC%84%B1%EB%8A%A5-%EC%B8%A1%EC%A0%95-%EB%8F%84%EA%B5%AC-perf

top 옵션은 현재 커널 내부에서 실행되는 함수(Symbol)들의 부하(Overhead) 비율을 스냅샷 형태로 표시한다.
```
perf top -p {확인할 특정 프로세스의 id}
perf top -t {확인할 특정 쓰레드의 id}
```

stat 은 perf 수행 중 커널 내부에서 발생한 이벤트 처리에 대한 통계 정보를 보여준다.
```
perf stat {통계 정보를 확인할 명령어}
```

위에서 확인해본 top 옵션은 커널에서 실행하고 있는 정보들을 실시간으로 확인할 수 있다는 장점이 있지만, 반대로 내용이 실시간으로 변하기 때문에 분석하기에는 어려울 수 있다. record 옵션을 사용하면 이러한 분석 정보를 파일로 저장할 수 있다.
```
perf record -p {저장할 특정 프로세스의 id}
perf reocrd -ag // 모든 프로세스의 정보 저장 및 스택 정보도 함께 저장
```

report 옵션은 record 에 의해 저장된 perf.data 파일을 분석하기 위해 사용되어질 수 있다.
```
perf report perf.data
```

<br>

## [linux] iostat 사용 방법
* https://blueyikim.tistory.com/556

iostat이란? 디스크 입출력 통계를 표시하는 명령어

```
# iostat
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           8.08    0.00    2.56    3.12    0.09   86.15

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
vda             514.75      4862.98      3565.66 7167829260 5255628922
```

* tps: 초당 입출력 작업 건수. tps가 높다는 것은 CPU가 busy라는 의미임
* KB_read/s: 초당 읽어들인 블록(512바이트) 수
* KB_wrtn/s: 초당 쓰여진 블록(512바이트) 수
* KB_read: 지금까지 읽어들인 블록(512바이트) 수
* KB_wrtn: 지금까지 쓰여진 블록(512바이트) 수

```
# iostat -x
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           8.08    0.00    2.56    3.12    0.09   86.15

Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
vda             110.63   196.79  305.96  208.78  4862.86  3565.57    32.75     1.56    3.04    1.31    5.56   0.22  11.21
```

* rrqm/s: 디바이스 큐에 대기중인 초당 읽기 요청의 건수
* wrqm/s: 디바이스 큐에 대기중인 초당 쓰기 요청의 건수
* r/s: 디바이스에 요청한 초당 읽기 요청의 건수
* w/s: 디바이스에 요청한 초당 쓰기 요청의 건수
* rsec/s: 디바이스에 초당 읽어들인 섹터의 개수
* wsec/s: 디바이스에 초당 기록한 섹터의 개수
* avgrq-sz: 디바이스에 요청한 초당 평균 데이터의 크기
* avgqu-sz: 디바이스에 요청한 초당 평균 큐 길이
* await: 디바이스에서 처리되기 위해서 요청된 I/O 평균 시간(밀리초). 큐에서 소요된 시간과 처리된 시간이 더해서 출력됨
* svctm: 디바이스에서 처리한 I/O 평균 시간(밀리초)
* %util: 디바이스에서 요청한 I/O 작업을 수행하기 위해 사용한 CPU 시간 비율. 이 값이 100%에 가까워지면 디바이스가 한계에 도달했다고 보면됨

<br>

## [linux] SAR(System Activity Reporter) 활용 방법
* https://www.cubrid.com/CUBRIDwiki/71317

/var/log/sa 경로에 (최근 30일 기준)일자 별로 sa와 sar 파일이 존재한다.

sa 파일은 리눅스에서 아래와 같은 sar 명령어를 통해 열어볼 수 있다.
```
sar -A -f {sa파일명} | less
```

sar 파일은 sa파일의 결과를 텍스트 형태로 출력해둔 파일이다.

<br>

## [linux] SPEC 파일의 Source 경로에 명시된 파일을 다운로드 받는 방법
* https://stackoverflow.com/questions/33177450/how-do-i-get-rpmbuild-to-download-all-of-the-sources-for-a-particular-spec

`spectool`을 사용하면 SPEC 파일에 명시된 Source 경로를 참조하여 자동으로 파일을 다운로드 받을 수 있다. 통상적으로 spectool을 사용하여 소스를 다운로드 받은 후 rpmbuild를 통해 RPM 파일을 생성한다.
```
$ cd
$ pwd
/home/centos
$ cd /home/centos/rpmbuild
$ ll
BUILD  BUILDROOT  RPMS  SOURCES  SPECS  SRPMS
$ spectool -C SOURCES/ -g SPECS/myspec.spec
$ rpmbuild -ba SPECS/myspec.spec
```

<br>

## [golang] golang 소스 코드를 직접 빌드해서 빌드 환경 구축하는 방법
* https://go.dev/doc/install/source
* https://go.dev/doc/toolchain
* https://go.dev/blog/rebuild

```
1. Build Go 1.4(Go 1.4 was the last distribution in which the toolchain was written in C)
# pwd
/home/centos/jhlee
# mkdir go-src-1.4
# tar -C /home/centos/jhlee/go-src-1.4 -xzf go1.4-bootstrap-20171003.tar.gz
# cd go-src-1.4/go/src
# export CGO_ENABLED=0
# ./make.bash
```

2. Build Go 1.17 with Go 1.4
```
# pwd
/home/centos/jhlee
# mkdir go-src-1.17
# tar -C /home/centos/jhlee/go-src-1.17 -xzf go1.17.src.tar.gz
# cd go-src-1.17/go/src
# export GOROOT_BOOTSTRAP=/home/centos/jhlee/go-src-1.4/go
# ./all.bash
```

3. Build Go 1.21.1 with Go 1.17
```
# pwd
/home/centos/jhlee
# mkdir go-src-1.21.1
# tar -C /home/centos/jhlee/go-src-1.21.1 -xzf go1.21.1.src.tar.gz
# cd go-src-1.21.1/go/src
# export GOROOT_BOOTSTRAP=/home/centos/jhlee/go-src-1.17/go
# ./all.bash
```

4. Complete!
```
ALL TESTS PASSED
---
Installed Go for linux/amd64 in /home/centos/jhlee/go-src-1.21.1/go
Installed commands in /home/centos/jhlee/go-src-1.21.1/go/bin
*** You need to add /home/centos/jhlee/go-src-1.21.1/go/bin to your PATH.
```

---

Building Go cmd/dist using /home/centos/jhlee/go-1.16. (go1.16.15 linux/amd64)
found packages main (build.go) and building_Go_requires_Go_1_17_13_or_later (notgo117.go) in /home/centos/jhlee/goroot/src/cmd/dist
```

toolchain을 통해 빌드 후에 새롭게 생성되는 폴더
* `bin/`
* `pkg/`

이후 Go 소스 코드 빌드 시에 필요한 폴더(`GOROOT`를 기준으로 아래 폴더의 위치를 파악함)
* src/
* pkg/
* bin/
  
빌드 시에 설정해주어야할 `환경 변수`
* `PATH`: `go`와 `gofmt` 바이너리가 포함된 `bin/`` 폴더 경로를 추가해주어야함
* `GOROOT`: `src/`와 `pkg/` 폴더가 존재하는 경로를 추가해주어야함
* `GOPROXY`: https://proxy.golang.org,direct 로 설정