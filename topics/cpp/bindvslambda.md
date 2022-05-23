## Bind vs Lambda

[**Bind Vs Lambda?**](https://stackoverflow.com/questions/1930903/bind-vs-lambda)

> A key advantage of lambdas is they can reference member functions statically, while bind can only reference them through a pointer. Worse, at least in compilers that follow the "itanium c++ ABI" (e.g. g++ and clang++) a pointer to a member function is twice the size of a normal pointer.

> So with g++ at least, if you do something like std::bind(&Thing::function, this) you get a result that is three pointers in size, two for the pointer to member function and one for the this pointer. On the other hand if you do [this](){function()} you get a result that is only one pointer in size.

> The g++ implementation of std::function can store up to two pointers without dynamic memory allocation. So binding a member function to this and storing it in a std::function will result in dynamic memory allocation while using a lambda and capturing this will not.

AS-IS
```cpp
worker_thread_ = boost::make_shared<boost::thread>(boost::bind(&Logger::log_handler, this));
::pthread_setname_np(worker_thread_->native_handle(), "logger worker");
```

TO-BE
```cpp
worker_thread_ = boost::make_shared<boost::thread>([this](){
    this->log_handler();
});
::pthread_setname_np(worker_thread_->native_handle(), "logger worker");
```