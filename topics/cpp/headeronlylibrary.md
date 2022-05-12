## Header-only 라이브러리와 multiple definition 에러

[**Header-only libraries and multiple definition errors**](https://stackoverflow.com/questions/3973218/header-only-libraries-and-multiple-definition-errors)

대표적인 header-only 라이브러리로 boost가 있는데 대체 이놈들은 어떻게 multiple definition 에러를 컨트롤 한 걸까?

> I want to write a library that to use, you only need to include one header file. However, if you have multiple source files and include the header in both, you'll get multiple definition errors, because the library is both declared and defined in the header. I have seen header-only libraries, in Boost I think. How did they do that?

<br>

친절한 어떤 분이 아래와 같이 답변을 남겨 주셨다. mutiple definition 에러의 경우 ODR 규칙을 어겼을때 발생하는데 템플릿의 경우에는 ODR 규칙을 만족하기 때문에 에러가 없다👍
> The main reason why Boost is largely header-only is because it's heavily template oriented. `Templates` generally get a pass from the one definition rule. In fact to effectively use templates, you must have the definition visible in any translation unit that uses the template.<br><br>Another way around the one definition rule (ODR) is to use `inline` functions. Actually, getting a free-pass from the ODR is what `inline` really does - the fact that it might inline the function is really more of an optional side-effect.<br><br>A final option (but probably not as good) is to make your functions static. This may lead to code bloat if the linker isn't able to figure out that all those function instances are really the same. But I mention it for completeness. Note that compilers will often inline `static` functions even if they aren't marked as `inline`.