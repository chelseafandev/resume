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

## [c++] 템플릿은 왜 ODR(One Definition Rule)을 위반하지 않는걸까?

**ODR이란?**
* https://learn.microsoft.com/en-us/cpp/cpp/program-and-linkage-cpp?view=msvc-170
> In a C++ program, a symbol, for example a variable or function name, can be declared any number of times within its scope. However, it can only be defined once. This rule is the "One Definition Rule" (ODR).

> A program consists of one or more translation units. A translation unit consists of an implementation file and all the headers that it includes directly or indirectly. Implementation files typically have a file extension of .cpp or .cxx. Header files typically have an extension of .h or .hpp. Each translation unit is compiled independently by the compiler. After the compilation is complete, the linker merges the compiled translation units into a single program. Violations of the ODR rule typically show up as linker errors. Linker errors occur when the same name is defined in more than one translation unit.

**템플릿이 ODR을 위반하지 않는 이유**
* https://stackoverflow.com/questions/34552380/why-cs-vector-templated-class-doesnt-break-one-definition-rule

> The same way any template definitions don't break the ODR — the ODR specifically says that template definitions may be duplicated across translation units, as long as they are literally duplicates (and, since they are duplicates, no conflict or ambiguity is possible).

> [C++14: 3.2/6]: There can be more than one definition of a class type (Clause 9), enumeration type (7.2), inline function with external linkage (7.1.2), class template (Clause 14), non-static function template (14.5.6), static data member of a class template (14.5.1.3), member function of a class template (14.5.1.1), or template specialization for which some template parameters are not specified (14.7, 14.5.5) in a program provided that each definition appears in a different translation unit, and provided the definitions satisfy the following requirements [..]

> Multiple inclusions of <vector> within the same translation unit are expressly permitted and effectively elided, more than likely by "#ifndef" header guards.