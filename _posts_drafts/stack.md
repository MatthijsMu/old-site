many programming 

A nice property of scoped data is that we only need to manage its memory during its lifetime, and that this lifetime is known at compile time. And because its visibility is limited to its scope, in function calls we can only see and only have to manage data in the scope of the callee. For example:

```c
void f() {
    int a = 3;
    h();
    return;
}

void h() {
    int b = 5;
    // a = 5; does not make sense here, a is not in scope
    return;
}
```

Function calls work "like a ladder": we can only see the data in the frame of the function that we are currently executing, and once we `return`, the callee's memory can be freed. 

This makes managing the memory of scoped data very easy: it can naturally be done using stacks