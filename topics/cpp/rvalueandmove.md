## 함수의 반환 문에서의 이동 의미론(move semantics)

[**C++11 rvalues and move semantics confusion (return statement)**](https://stackoverflow.com/questions/4986673/c11-rvalues-and-move-semantics-confusion-return-statement)


아래 예시는 run-time error를 발생 시킬 수 있다는 것에 주의하자. return_vector 함수의 반환 값이 저장되는 변수인 rval_ref는 return_vector 함수내에서 이미 파괴되어버린 tmp 변수에 대한 reference 정보를 들고 있게된다. 운이 좋다면(즉, 그냥 실행될 수도 있다는 의미) 이 코드는 즉각적으로 충돌을 발생시킬 수 있다.
```cpp
std::vector<int>&& return_vector(void)
{
    std::vector<int> tmp {1,2,3,4,5};
    return std::move(tmp);
}

std::vector<int> &&rval_ref = return_vector();
```

<br>

> The best way to code what you're doing is:
```cpp
std::vector<int> return_vector(void)
{
    std::vector<int> tmp {1,2,3,4,5};
    return tmp;
}

std::vector<int> rval_ref = return_vector();
```

`tmp` 변수는 반환 구문내에서 암묵적으로 rvalue로 처리된다. RVO(return-value-optimization)를 통해 반환될 수도 있고, 만약 컴파일러가 RVO가 수행될 수 없다고 판단하는 경우에는 벡터의 이동 생성자를 사용하여 반환 작업을 수행할 것이다. RVO가 수행되지 않는 경우는 반환 형식이 이동 생성자를 가지고 있지 않는 경우다(이때는 복사 생성자가 사용됨)

> I.e. just as you would in C++03. tmp is implicitly treated as an rvalue in the return statement. It will either be returned via return-value-optimization (no copy, no move), or if the compiler decides it can not perform RVO, then it will use vector's move constructor to do the return. Only if RVO is not performed, and if the returned type did not have a move constructor would the copy constructor be used for the return.

<br>

아래 예제를 통해 명시적으로 std::move 함수를 사용하지 않더라도 이동 생성자를 호출하는 것을 확인할 수 있다. (단, 컴파일 옵션에 fno-elide-constructors를 추가해주어야 확인이 가능함)
```cpp
#include <iostream>

namespace cfd::rvo
{
    class my_object
    {
    public:
        my_object()
        {
            std::cout << "default :)" << std::endl;
        }

        my_object(const my_object &other)
        {
            std::cout << "copy :)" << std::endl;
        }

        my_object(my_object &&other)
        {
            std::cout << "move :)" << std::endl;
        }

        void print()
        {
            std::cout << "hello cfd::rvo world!" << std::endl;
        }
    };
}

cfd::rvo::my_object make_my_object()
{
    cfd::rvo::my_object mo;
    // 명시적으로 std::move를 사용하지 않아도 이동 생성자를 호출하는 것을 볼 수 있다.
    // 단, 컴파일 옵션에 fno-elide-constructors를 추가해주어야만 move constructor의 호출을 확인할 수 있음.
    // fno-elide-constructors 옵션을 추가하지 않는 경우에는 RVO에 의해 이동 비용 조차도 사라지게 된다. 이 얼마나 좋은가?
    return mo;
}

cfd::rvo::my_object make_my_object_with_move()
{
    cfd::rvo::my_object mo;
    return std::move(mo);
}

int main()
{
    std::cout << "=== call make_my_object() ===" << std::endl;
    auto mo = make_my_object();
    mo.print();
    std::cout << "=============================" << std::endl << std::endl;

    std::cout << "=== call make_my_object_for_rvalue_reference() ===" << std::endl;
    auto mo_for_rr = make_my_object_with_move();
    mo_for_rr.print();
    std::cout << "==================================================" << std::endl;

    return 0;
}
```

"반환값으로 복사될 오른값 참조 **매개변수**에 std::move를 적용하면 복사 생성이 이동 생성으로 변한다"라는 정보를 확대해석해서, "함수가 반환할 지역 변수에도 그런 최적화를 적용할 수 있을 것이다"라는 엉뚱한 결론을 이끌어내서는 안된다.
```cpp
Widget makeWidget()
{
    Widget w;
    ...
    return std::move(w); // w를 반환값으로 이동한다. 이렇게 하면 안 됨!
}
```

추론 과정에서 프로그래머가 간과한 것은, 이런 종류의 최적화를 위한 대비책을 표준 위원회가 예전부터 마련해 두었다는 점이다. makeWidget의 `복사` 버전에서, 만일 지역 변수 w를 함수의 반환값을 위해 마련한 메모리 안에 생성한다면 w의 복사를 피할 수 있다는 점은 이미 오래 전부터 알려져 있었다. 이것이 소위 반환값 최적화(return value optimization, RVO)이다.

컴파일러가 결과를 값 전달 방식으로 반환하는 함수의 어떤 지역 객체의 복사(또는 이동)를 제거할 수 있으려면 (1) 그 지역 객체의 형식이 함수의 반환 형식과 같아야 하고 (2) 그 지역 객체가 바로 함수의 반환값이어야 한다.

```cpp
Widget makeWidget()
{
    Widget w;
    ...
    return w;
}
```

이 예에서는 위 두 조건을 모두 만족하므로 제대로 된 C++ 컴파일러라면 반드시 반환값 최적화를 적용해서 w의 복사를 피할 것이라고 가정해도 안전하다. 즉, makeWidget의 `복사`버전은 사실 아무것도 복사하지 않는다.

정리하자면, 함수가 지역 객체를 값으로 반환하는 경우, 그 지역 객체에 std::move를 적용한다고 해서 컴파일러에게 도움이 되지는 않는다(복사 제거를 할 수 없는 상황이면 어차피 그 지역 객체를 오른값으로 취급하므로). 오히려 컴파일러를 방해할 수 있다(반환값 최적화를 무력화해서). 지역 객체에 std::move를 적용하는 것이 합당한 상황도 있지만(구체적으로 말하면, 더 이상 사용하지 않을 변수를 다른 함수에 넘겨주는 경우), 반환값 최적화가 적용될 수 있는 return 문이나 반환값 최적화의 대상이 아닌 값 전달 방식의 매개변수를 돌려주는 return 문에서 std::move를 적용하는 것은 합당한 상황에 해당하지 않는다.

**reference**
- https://stackoverflow.com/questions/4986673/c11-rvalues-and-move-semantics-confusion-return-statement
- https://stackoverflow.com/questions/19267408/why-does-stdmove-prevent-rvo-return-value-optimization
- 스콧 마이어스. (2015). 이펙티브 모던 C++, C++11과 C++14를 효과적으로 사용하는 42가지 방법. 인사이트