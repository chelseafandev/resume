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
    return mo;
}

cfd::rvo::my_object make_my_object_with_move()
{
    cfd::rvo::my_object mo;
    // 명시적으로 std::move를 사용하지 않아도 이동 생성자를 호출하는 것을 볼 수 있다.
    // 단, 컴파일 옵션에 fno-elide-constructors를 추가해주어야만 move constructor의 호출을 확인할 수 있음.
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