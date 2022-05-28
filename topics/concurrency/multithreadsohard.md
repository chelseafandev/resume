## 멀티쓰레드 프로그래밍이 왜 이리 힘드나요?

[**멀티쓰레드 프로그래밍이 왜이리 힘드나요**](https://deview.kr/2013/detail.nhn?topicSeq=64)

```cpp
*data = 10;
is_ready->store(true, memory_order_relaxed);
```
위 is_ready 에 쓰는 연산이 relaxed 이기 때문에 위의 *data = 10 과 순서가 바뀌어서 실행된다면 is_ready 가 true 임에도 *data = 10 이 채 실행이 끝나지 않을 수 있다는 것이지요. 따라서 consumer 쓰레드에서 is_ready 가 true 가 되었음에도 제대로된 data 를 읽을 수 없는 상황이 벌어집니다.


```cpp
while (!is_ready->load(memory_order_relaxed)) {
}

std::cout << "Data : " << *data << std::endl;
```
consumer 쓰레드에서도 마찬가지 입니다. 아래에 data 를 읽는 부분과 위 is_ready 에서 읽는 부분이 순서가 바뀌어 버린다면, is_ready 가 true 가 되기 이전의 data 값을 읽어버릴 수 있다는 문제가 생깁니다. 따라서 위와 같은 생산자 - 소비자 관계에서는 memory_order_relaxed 를 사용할 수 없습니다.

> 멀티쓰레드 환경에서 포인터를 사용하는 경우에는 Cache LIne Size Boundary에 걸치는지 꼼꼼하게 확인 해야함


멀티쓰레드에서의 공유메모리
- 다른 코어에서 보았을 때 업데이트 순서가 틀릴 수 있다.
- 메모리의 내용이 한 순간(클럭)에 업데이트 되지 않을 때도 있다.


지금까지 배운 모든 자료구조가 멀티쓰레드에서는 동작하지 않는다. 심지어 STL도 동작하지 않음👻즉, 자료 구조에 맞추어 최적화된 lock-free 알고리즘을 일일이 개발해야 한다.
- 물론 범용적으로 사용이 가능한 Intel TBB나 VS2012 PPL등이 있긴함
- CAS가 있으면 기존의 모든 싱글쓰레드 자료구조를 wait free로 바꿀 수 있다는 게 증명되어 있음. 하지만 100배 이상의 성능 저하가 발생할 수 있으니 현실에선 사용이 불가능하다고 봐야함.

[Intel® oneAPI Threading Building Blocks Documentation](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onetbb-documentation.html?s=Newest)