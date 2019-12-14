# Java基础-9-泛型

Java泛型[https://www.cnblogs.com/coprince/p/8603492.html](https://www.cnblogs.com/coprince/p/8603492.html)

泛型（Generic）：参数化类型（parameterized type）称为泛型。

泛型是将数据类型参数化，即在编写代码时将数据类型定义成参数，这些类型参数在使用之前才进行指明。泛型提高了算法的重用性，使得程序更加灵活、安全和简洁。

Java语言没有真正的实现泛型。Java中的泛型都是在编译器这个层次来实现的，编译器生成的Java字节码是不包含泛型信息的，Java源代码中的泛型类型信息在将Java源代码编译成字节码时，在字节码中被擦除，这个过程即类型擦除。
