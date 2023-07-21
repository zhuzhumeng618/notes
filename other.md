# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 优势</font></p>

高效：go 是一个静态编译型的语言，可以将源码直接编译成机器码或可执行文件，同时 go 的 GC 和 goroutine 在处理大规模任务的时候相对于其他语言而言更为高效。

并发：go 内置 goroutine 和 channel，能够很方便的编写并发程序，通道可以在多个 goroutine 间进行数据的共享，所以它在网络编程、分布式系统、大数据处理方面具有一定得优势。

简单：语法简单，没有 Java、C++ 的继承和多态、面向对象等语言特性。

可读：团队协作方面也简单，有内置的 go fmt，统一代码风格。

跨平台：交叉编译，windows 编译成 linux 可执行文件，标准库中也有与平台无关的包，如 net、io，所以可以很方便的编写跨平台程序。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 数据类型</font></p>

go 中数据类型分为三种：基本数据类型、复合数据类型、其它数据类型。

基本数据类型：

1. 有符号整数类型：int、int8、int16、int32、int64
2. 无符号整数类型：uint、uint8、uint16、uint32、uint64
3. 布尔数据类型：true、false
4. 浮点数据类型：float32、float64
5. 复数数据类型：complex64、complex128

复合数据类型：

1. 数组：array
2. 切片：slice
3. 映射：map
4. 接口：interface
5. 结构体：struct

其它数据类型：

1. 通道：channel
2. 字符串：string
3. 指针：pointer
4. 函数：func

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 包使用</font></p>

包是代码的组织单元，每一个 go 的源文件都属于一个包，并且在源文件第一行使用 package 关键字声明源文件所属的包。

包中定义的任何可见的变量、函数、类型都可以被其他包进行引用。

1. 导入包：import 关键字可以用来导入需要使用的包。
2. 使用包：通过 package 定义的包名/或者在 import 时定义的包别名通过 . 选择器的方式进行使用包中函数、变量。
3. 自定义包：package xxx，编写需要公开的函数或变量，在代码中定义 init 函数。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 类型转换</font></p>

go 不支持隐式的类型转换，只能通过 类型() 的方式进行类型显式转换。

数值之间转换：支持整数与浮点之间的转换，但会损失精度。

字符串转整数：通过标准库 strconv 进行转包，这个包包括 atob、atoc、atof、atoi 的转换。

指针之间转换：需要用到 unsafe 包的 Pointer 函数。

自定义类型之间的转换：底层数据类型必须相同，通过类型的自定义函数进行转换。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 如何停止 goroutine</font></p>

goroutine 是 go 中的协程，也就是一个轻量级的线程，相比于传统线程，goroutine 创建和销毁的代价非常低，可以同时创建成千上万个协程，不会导致系统负担过重。

goroutine 开启也很方便，通过关键字 go 来启动一个协程，会在一个独立的栈空间执行响应的函数，可以在函数中执行阻塞和非阻塞操作。

在 goroutine 中使用通道来接收停止信号，当主线程需要停止 goroutine 时，向通道发送一个信号即可，goroutine 不断检测该通道是否有信号，如果有信号，立即退出。

主线程通过向 channel 中传递信号的方式可以关闭协程：

1. 通过 context.Context 关闭协程。
2. 通过 channel bool 关闭协程。
3. 通过 channel close 关闭协程。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 运行时检查变量类型</font></p>

reflect 反射技术是一种可以在程序运行期间检查变量类型、变量值的一个技术，可以在不清楚变量具体类型时操作内部数据。

通过 TypeOf 函数可以获取到变量的类型。

通过 ValueOf 函数可以获取到变量的值。

反射可能会带来一定得性能损失，尽量过度使用反射，编码时尽可能使用静态类型检查，避免在运行时进行类型检查和转换。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 接口之间可以存在的关系</font></p>

接口是一组由不同方法签名构成的一个集合。

嵌套：接口的嵌套可以包含嵌套接口(B 嵌套 A)的所有方法，B 的实现类必须实现 (B + A) 的所有方法，才能是实现了 B。

扩展：嵌套为前提，B 嵌套 A，A 可以通过在 B 接口中扩展新方法达到 A 接口的扩展。

嵌套是指：接口作为其他接口的一部分，其它接口拥有这个接口的所有方法，并可以新增新方法。

扩展是指：接口在其他接口上进行方法的扩展，实现类必须实现新添加的方法。

类型转换：接口除接口名不一致外，方法签名全部相同，则可以进行接口的转换。

接口实现：一个接口可以被多个不同的类型实现。

注意：接口与接口间没有继承，而是通过包含相同的方法集合来建立联系。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 同步锁特点</font></p>

同步锁是一种并发编程技术，可以保证多个 goroutine 之间对共享数据的安全访问，在 go 中主要是通过 sync 包中的 Mutex 类型进行实现。

同步锁的特点：

1. 互斥：这个特性保证了同一时刻只能有一个 goroutine 持有锁，其它 goroutine 需要等到当前 goroutine 释放锁。
2. 阻塞：锁被持有时，当前尝试获取锁的 goroutine 将会被阻塞，直到锁被释放。
3. 公平：锁的获取是公平的，即锁会按照申请的先后顺序分配给等待的 goroutine，避免了其它 goroutine 无法获取锁的情况。

同步锁的作用是保护共享数据，避免多个 Goroutine 同时对共享数据进行修改而导致的竞争条件和数据竞争问题。

在需要保证共享数据的安全访问时，可以使用同步锁来对临界区进行加锁，以避免并发修改数据产生的问题。

在使用同步锁时，需要注意避免死锁和饥饿等问题的发生，同时需要合理地设计锁的粒度和作用域，避免锁的竞争导致性能下降。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go channel</font></p>

**channel 是 goroutine 之间通信和同步的机制，channel 特点：**

线程安全：channel 可以安全的在 goroutine 之间传递数据，避免了数据竞争和死锁等问题。

阻塞式：当 channel 中没有数据时，读取操作会被阻塞，直到 channel 中有数据可读，当 channel 已满时，会对写操作进行阻塞，直到有空间可以进行写入。

有缓冲和无缓冲：不带缓冲的 channel 可以保证每次写入和读取都是同步的；带缓冲的 channel 可以在缓冲区未满时进行写入操作而不阻塞，直到缓冲区满时再阻塞写入操作。

可关闭：channel 可以被显式的关闭，以通知 channel 的接收方不再有数据可读，避免接收方被永久地阻塞。

**使用 channel 需要注意的问题：**

避免死锁：当使用 channel 进行 goroutine 之间的通信和同步时，需要确保不会出现死锁的情况；可以使用 select 语句和超时机制等方式来避免 channel 阻塞问题。

避免竞态条件：当多个 Goroutine 访问同一个 Channel 时，需要注意避免竞态条件的发生；可以使用 Mutex 和 sync 包中提供的其他同步机制来避免并发访问 channel 导致的问题。

合理使用缓冲：当使用带缓冲的 channel 时，需要根据实际需要设置缓冲区的大小，避免缓冲区过大或过小导致的性能问题。同时需要注意，当 channel 中的数据过多时，会导致内存占用过高，需要及时清理不必要的数据。

避免 channel 泄漏：当使用 channel 时，需要注意避免 channel 泄漏的问题，即在不需要使用 channel 时及时关闭 channel，避免 channel 占用过多的系统资源。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go channel 带缓冲</font></p>

在 Go 语言中，channel 缓冲是指在创建 channel 时设置的缓冲区大小。带缓冲的 channel 可以在缓冲区未满时进行写入操作而不阻塞，直到缓冲区满时再阻塞写入操作。

可以提高并发性能：使用带缓冲的 channel 可以提高并发程序的性能，因为缓冲区可以暂时存储数据，避免了每次数据传输时都需要阻塞等待的情况。这种方式特别适用于生产者-消费者模式，其中生产者的产生速度快于消费者的处理速度，缓冲区可以暂时存储一定量的数据，使得生产者和消费者的速度可以适度地解耦。

缓冲区大小需要合理设置：channel 缓冲区大小的设置需要根据实际应用场景进行合理的选择，过小的缓冲区可能会导致生产者被阻塞，过大的缓冲区可能会导致内存占用过高。一般来说，需要根据实际情况进行调整，以达到最优的性能表现。

带缓冲的 channel 可能会出现死锁问题：当使用带缓冲的 channel 进行 goroutine 之间的通信和同步时，需要注意避免死锁的问题。因为带缓冲的 Channel 可以在缓冲区未满时进行写入操作，如果生产者写入数据的速度过快，可能会导致缓冲区已满而阻塞生产者，此时如果消费者已经不再消费数据，整个程序就会进入死锁状态。

可以使用 close() 函数关闭 channel：当使用带缓冲的 channel 时，需要注意及时清理缓冲区中的数据，可以使用 close() 函数来显式地关闭 channel。关闭 channel 会使得 channel 中未被读取的数据被丢弃，并且后续的写入操作会导致 panic 异常。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go cap</font></p>

1. 数组：cap 函数返回数组长度。
2. 切片：cap 函数返回切片容量。
3. 通道：cap 函数返回 channel 的容量，也就是 channel 缓冲区的大小。

只能作用于包含指针的数据类型，如数组、切片、通道，而不能用于值类型的数据类型。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go new</font></p>

new 是 golang 中的内置函数，可以创建一个变量/类型对应的指针，值为零值，类型为该类型的指针类型。

new 返回一个指向新变量的指针，该变量的值为其类型的零值。在 Go 语言中，每个变量都有一个类型和一个值，而 new 可以用于创建变量的指针。

new 的作用在于在堆上分配内存空间，而不是在栈上分配。

使用 new 函数创建变量时，返回的指针指向在堆上分配的变量，即使该变量在函数调用结束后仍然存在。

因此，new 通常用于创建结构体、数组和其他复杂数据类型的指针。

需要注意的是，new 只能创建变量的指针，而不能用于创建变量本身。如果需要创建变量本身，可以使用变量声明语句。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go make</font></p>

make 是 golang 中的内置函数，可以为 slice、map、channel 这三种类型进行初始化操作。

需要注意的是，使用 make 函数创建的数据结构是分配在堆上的，并返回一个引用，即一个指向数据结构的指针。

这与使用 new 函数创建变量的方式不同，因为 new 只分配了变量所需的内存空间，而 make 既分配了变量所需的内存空间，又初始化了变量的其他属性。

因此，make 更适用于创建 slice、map 和 channel 等复杂的数据结构。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 数组与切片的区别</font></p>

长度：数组长度不可变，slice 有扩容机制。

分配内存：数组在声明时就会分配一段连续的内存空间。切片是动态的数据结构，底层是一个指向底层数组的指针、长度和容量，容量可以随元素的增加而自动扩容。

传递方式不同：数组作为函数形参时，传递的是拷贝此数组的一个副本，在函数内部对数组进行修改不会改变原有的数组值；切片作为函数形参时，传递的是拷贝切片的底层指针、长度、容量，在函数内部对切片进行修改会改变原有切片的值。

声明方式不同：array 使用 new 或者普通的，slice 必须使用 make 进行初始化。

综上所述，数组和切片都有自己的特点和优缺点，需要根据具体的需求来选择合适的数据结构。如果需要存储一组固定长度的元素，可以使用数组；如果需要动态增长和缩减元素，可以使用切片。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 值传递和地址传递</font></p>

值传递：在通过函数传参时，传递的是值的拷贝，在函数中修改参数的值不会影响原始值，golang 中基本数据类型、数组、结构体采用的都是值传递。

地址传递：在通过函数传参是，传递的是地址的拷贝，在函数中修改参数的值会影响原始值，golang 中 slice、map、pointer 类型是以地址传递。

使用值传递还是地址传递需要根据实际情况来决定，一般来说，如果参数是一个大型的结构体或者数组，使用地址传递可以避免值的拷贝，提高程序的效率。如果参数是一个简单的值类型，使用值传递即可。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 切片扩容</font></p>

当切片容量不足时，go 会为切片重新分配一个更大的内存空间，通常情况下，新空间为原来空间的 2 倍，然后将数据复制到新空间内，并在新空间最末尾添加新元素，最后返回新切片，切片的底层指针会指向新的底层数组。

需要注意的是，切片的扩容可能会导致底层数组重新分配内存空间，并将原来的数据复制到新的内存空间中，因此扩容操作的时间复杂度为 O(n)，其中 n 表示切片的长度。因此，如果需要对一个大型的切片进行频繁的扩容操作，可能会对程序的性能产生影响。为了避免这种情况，可以在创建切片时尽可能地指定切片的容量，或者使用数组来代替切片。

扩容条件：

切片的扩容条件：在使用 append 函数追加元素时，如果当前元素个数达到了底层数组容量，就会触发切片的扩容机制。

切片底层的数组容量在创建切片时确定，当切片长度小于底层数组容量时，不进行扩容；当切片长度大于底层数组容量时，触发扩容。

切片的扩容规则是将底层数组的容量翻倍，但是在实际扩容过程中，可能会存在一些优化策略，例如：在容量小于 1024 时，每次扩容增加 1 倍容量；在容量大于 1024 时，每次扩容在原容量基础上增加 25% 容量等等，这些细节 golang 会在运行时根据实际情况进行调整，用户不需要过多关注。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go defer</font></p>

多个 defer 语句，遵从后进先出(Last In First Out，LIFO)的原则，最后声明的 defer 语句，最先得到执行。

**defer 在 return 语句之后执行**，但在函数退出之前，defer 可以修改返回值。

无名返回值：defer 不会修改 return 值。
有名返回值：defer 会修改 return 值。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 底层实现</font></p>

slice 是基于数组的数据结构，结构体内部构造为：指向底层数组的指针、长度、容量，slice 的底层实现是一个动态数组，可以动态地增长和缩小，同时具有数组的索引和迭代操作的优点。

slice 创建时，golang 会为其分配一块连续的内存空间用于存储数据，并返回一个指向该内存区域的指针，该指针被称为 slice 的底层指针，slice 还会记录该内存区域的长度、容量。

在 slice 容量不足以存储新的元素时，Go 语言会自动重新分配一块更大的内存区域，并将原有的元素复制到新的内存区域中，然后将新的元素添加到新的内存区域中。这种自动扩容机制使得 slice 的大小可以根据需要自动调整，无需手动进行内存分配和释放操作，同时也保证了 slice 的连续性，使得它在访问和遍历元素时具有更好的性能表现。

需要注意的是，由于 slice 是对底层数组的引用，因此多个 slice 可以共享同一个底层数组。这种特性使得 slice 在函数之间传递时非常高效，同时也需要注意避免对 slice 中的元素进行修改，从而影响到其他共享同一个底层数组的 slice。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 扩容机制注意点</font></p>

切片的扩容机制是在原切片容量的基础上扩容，一般是 2 倍或 1.5 倍，具体扩容的倍数由实现算法决定，注意点：

1. 切片扩容会重新分配一块连续的内存空间，因此需要将原切片中的元素复制到新的内存空间中，这个过程可能会比较耗时。
2. 在使用 append 函数添加元素时，如果元素的总个数超过了容量，则触发扩容机制。
3. 切片扩容会导致原切片和返回的新切片指向不同的底层数组，因此原切片的修改不会影响新切片的值。
4. 切片扩容不是每次添加元素都会触发，而是当切片容量不足以容纳更多元素时才会触发。
5. 当切片容量小于 1024 时，扩容时新的容量会翻倍，当容量大于等于 1024 时，新的容量会增加原容量的 1/4，也就是乘以 1.25。
6. 由于切片底层是基于数组实现的，因此切片扩容时，如果原数组的容量不足以容纳新的元素，也会触发数组的重新分配和拷贝。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 扩容前后是否相同</font></p>

在 Golang 中，扩容前后的 Slice 是不同的。在进行 Slice 扩容时，会创建一个新的底层数组，并将原来的元素拷贝到新的数组中。因此，扩容前后的 Slice 指向的底层数组是不同的。

Golang 中的 Slice 是基于数组实现的，因此在创建 Slice 时，底层会创建一个数组来存储数据。当 Slice 中的元素个数超过底层数组的容量时，就需要进行扩容。而在 Golang 中，数组的大小是固定的，无法进行扩容，因此需要创建一个新的底层数组，并将原来的元素拷贝到新的数组中。这样就可以实现 Slice 的扩容了。由于扩容后底层数组的地址已经发生了变化，因此扩容前后的 Slice 底层数组是不同的，即扩容前后的 Slice 不再共享底层数组。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 参数传递、引用传递</font></p>

在 Golang 中，函数调用时参数传递可以分为值传递和引用传递。

值传递：将参数的值复制一份，然后将复制的值传递给函数，函数对参数的修改不会影响到原始的值。常见的值类型如 int、float、bool 等都是值类型，它们的传递都是值传递。

引用传递：将参数的地址复制一份，然后将复制的地址传递给函数，函数对参数的修改会影响到原始的值。常见的引用类型如 Slice、map、Channel、指针等都是引用类型，它们的传递都是引用传递。

需要注意的是，在 Golang 中数组虽然是引用类型，但是它的传递却是值传递。这是因为 Golang 的数组长度是固定的，数组的值复制时会将整个数组的元素都复制一遍，因此传递数组时的开销较大，而且数组的长度也不可变，因此将数组的地址复制一份也无法修改原数组的长度，所以 Golang 采用了值传递的方式。

总之，对于值类型的参数，使用值传递即可；对于引用类型的参数，使用引用传递可以避免大量数据的复制，提高程序的效率。同时，在使用引用类型的参数时，需要注意并发访问的问题。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go map 底层实现</font></p>

map 描述了一种键与值的映射关系，开发者通常会通过键来查询其对应的值。map 最常见的底层实现有两种：基于 **Hash 散列**和基于**平衡树**，两者的存取时间复杂度不同，Go 语言的 map 属于前者范畴。

Go 语言 map 的底层实现基于 Hash 散列。Hash 散列是一种著名的广义上的算法，它能够将**任意长度的数据映射**到**有限的值域**上面。

Hash 算法有两大核心：设计 Hash 函数和解决 Hash 冲突。

一个map 中会包含很多桶，每个桶中可以存放8个键值对。

**注意：键不重复 & 键必须可哈希（int/bool/float/string/array）**

设计 Hash 函数：

基本原则：

1. 尽可能让输入的数据映射到不同的值域上面。
2. 函数计算过程需要保证高性能。

但实际工程中由于输入数据范围是无限的，而输出值域范围是有限的，因此必然存在不同的输入数据经过映射后得到相同的输出值，这种现象称为 Hash 冲突。

![other-Hash 冲突](./images/other/map-Hash%20冲突.png)

常见的著名 Hash 算法还有：MD5、SHA1、SHA2 等等。

Java 中 Hashmap：

![map-Java Hashmap](./images/other/map-Java%20Hashmap.png)

Go 中 map：

![map-Java Hashmap](./images/other/map-go%20map.png)

解决 Hash 冲突：

如何解决 Hash 冲突是 Hash 算法中的核心一环，最常见的做法是拉链法。所谓拉链法是指当 Hash 冲突产生时，将出现冲突的 Bucket 位用链表这一数据结构串联。

![map-拉链法](./images/other/map-拉链法.png)

go 哈希函数：

map 的一个关键点在于，哈希函数的选择。在程序启动时，会检测 cpu 是否支持 aes，如果支持，则使用 aes hash，否则使用 memhash。这是在函数 alginit() 中完成，位于路径：src/runtime/alg.go 下。

hash 函数，有加密型和非加密型。

加密型的一般用于加密数据、数字摘要等，典型代表就是 md5、sha1、sha256、aes256 这种。

非加密型的一般就是查找。在 map 的应用场景中，用的是查找。

选择 hash 函数主要考察的是两点：性能、碰撞概率。

Go 语言中的 map 是一种无序的键值对的集合，底层实现使用了哈希表（hash table）。

具体来说，Go 语言中的 map 实际上是一个指向哈希表的指针。哈希表本身是由若干个桶（bucket）组成的，每个桶包含了若干个键值对，每个键值对由一个 key 和一个 value 组成。在对 map 进行读写操作时，Go 语言会根据 key 计算出它在哈希表中的位置，然后直接访问对应的桶，从而实现高效的访问。

需要注意的是，Go 语言中的 map 不是线程安全的，因此在多线程并发访问时需要使用锁等机制来保证安全。

另外，由于哈希表的大小是固定的，因此当 map 中的元素数量达到一定程度时，需要对哈希表进行扩容。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go map 扩容</font></p>

在 Golang 中，map 的底层实现是使用哈希表（Hash Table）实现的。在插入新元素时，如果当前 map 中的元素个数达到了当前 map 所能容纳的最大元素个数，就会触发扩容操作。

map 的扩容操作会重新创建一个更大的哈希表，并将旧哈希表中的元素重新哈希到新的哈希表中。同时，新哈希表的大小一定是旧哈希表大小的两倍，因为 Golang 采用了指数级扩容策略，每次扩容后 map 可以容纳的元素个数是之前的两倍。

当 map 进行扩容时，由于哈希表中的元素需要重新哈希到新的哈希表中，因此会涉及到大量的内存复制操作，导致性能下降。为了减少这种情况的发生，Golang 中的 map 实现采用了增量式哈希算法，可以在扩容时只复制新增的元素，从而提高性能。

需要注意的是，在 map 进行扩容时，可能会导致哈希冲突的数量增加，因此扩容后的 map 的性能可能会有所下降。为了避免这种情况，可以考虑在创建 map 时指定初始容量，以减少扩容的次数。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go map 查找</font></p>

在 Go 语言中，使用 map 查找一个键值对的过程可以通过 `map[key]` 来完成，返回值是对应的值和一个表示是否存在的布尔值。

具体来说，如果 map 中存在该键，则返回对应的值和布尔值 `true`；如果不存在该键，则返回值类型的零值和布尔值 `false`。

需要注意的是，map 的键类型必须支持相等运算，例如，数字、字符串、指针、通道、接口类型、结构体类型等都是支持的，但是数组、切片、函数类型等不支持。

在 Golang 中，map 的查找是通过哈希表实现的。当程序执行 map 查找操作时，会先根据哈希函数将 key 转换成一个哈希值，然后在哈希表中查找该哈希值对应的桶(bucket)，再在桶中查找对应的键值对。

具体来说，当 map 中的键值对数量超过一定阈值时，会触发自动扩容操作。扩容操作会重新分配更大的桶数组，并将原有的键值对重新哈希分布到新的桶中。

在查找时，Golang 的 map 会先通过哈希值定位到对应的桶(bucket)，然后在桶中遍历链表（每个桶可能对应多个键值对）查找对应的键值对。在遍历链表的过程中，如果发现某个键值对的 key 与要查找的 key 相等，则返回该键值对的 value。

需要注意的是，如果 map 中的键值对过多，桶中的链表会很长，查找时效率会降低，因此需要根据实际情况合理设置 map 的容量和哈希函数，以充分利用哈希表的优势。同时，当 map 中的键值对类型为复杂类型（如结构体）时，需要重载对应的哈希函数和比较函数，以确保哈希表的正确性。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 介绍 channel</font></p>

go 语言中的 channel 是用于多个 goroutine 之间进行通信的一种机制，通过 channel 可以在多个 goroutine 间安全地传递数据。

channel 是一种类型，可以使用内置的 make() 函数来创建它们，创建 channel 时，需要指定它们可以传输的数据类型。

使用 channel 时，可以在 goroutine 之间传递数据，通过它们进行同步（无缓冲）和异步（有缓冲）的操作，在使用 channel 时需要注意以下几点：

1. channel 是引用类型，可以像 slice 和 map 一样传递给函数。
2. 默认情况下，channel 是无缓冲的，只有当 goroutine 接收端准备好接收数据时，goroutine 发送端发送操作才会成功，如果发送操作没有被接收，goroutine 发送端将会阻塞。
3. 通过 make() 函数创建带缓冲的 channel 时，可以指定缓冲区的大小，在缓冲区没有被填满前，发送操作不会阻塞。
4. channel 支持多路复用，使用 select 语句在多个 channel 上进行选择和等待。
5. channel 可以用于控制 goroutine 的执行，例如通过关闭 channel 来通知 goroutine 退出。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go channel ring buffer</font></p>

channel 会被实现为一个 FIFO 队列，当向 channel 发送数据时，数据会被添加到队列的末尾，当从 channel 接收数据时，数据会被从队列的头部取出。

go 1.3 版本中，新增了一种环形缓冲区(ring buffer)的 channel 实现方式，可以用于提高 channel 的性能，具体来说，当创建一个缓冲区大小为 n 的 channel 时，go 会为其分配一个大小为 n 的环形缓冲区，而不是一个简单的队列。

使用环形缓冲区实现 channel 有以下几个好处：

1. 避免动态内存分配：在缓冲区大小确定的情况下，环形缓冲区可以在创建时一次性分配所需的内存，避免了频繁的动态内存分配和释放操作，从而提高了性能。
2. 提高缓存命中率：环形缓冲区会将元素放置在连续的内存块中，这样可以提高缓存命中率，从而减少缓存访问延迟，提高了通信的效率。
3. 支持无锁访问：由于 channel 是在多个 goroutine 之间进行通信的，因此通常会涉及到并发访问的问题，环形缓冲区的视线可以采用无锁算法，从而避免了锁竞争带来的开销，提高了并发访问的效率。

需要注意的是，使用环形缓冲区实现 channel 也有一些限制和注意事项。例如，缓冲区大小必须是 2 的幂次方，否则可能会导致缓冲区溢出或者浪费内存等问题。同时，对于特殊的 channel 操作，如 close、select 和带缓冲区的 channel 等，也需要注意环形缓冲区的使用方式。

ring buffer 是一种循环缓冲区，当达到缓冲区的末尾时，可以从头端重新开始写入数据。这样，可以避免缓冲区溢出的问题，并且在写入和读取数据时保持高效的性能。

在使用 ring buffer 时，需要确保写入数据的速度不会超过读取数据的速度，否则可能会导致 buffer 溢出的问题。因此，在使用 ring buffer 时需要仔细考虑数据流的速度和大小。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 方法和函数的区别</font></p>

方法是一个包含接收者参数的函数，为指定的接收者提供一些行为处理；

函数是一段代码，可被调用并接收参数和返回结果。

与方法不同，函数没有接收者参数，因此它们无法直接修改调用者的状态。函数在 go 语言中是一等公民，可以像任何其他类型的值一样被传递和赋值。函数还可以是匿名的，或者被作为闭包使用，以便在不同的作用域中进行操作。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 指针接收者和值接收者</font></p>

值接收者会在方法调用时会对接收者进行复制，当方法内部存在对接收者的修改操作时，不会改变原有接收者的数据。

指针接收者避免了在方法调用时对接收者复制的操作，从而提高程序的性能，当方法内部存在对接收者的修改操作时，会改变原有接收者的数据。

1. 不想改变接收者数据时用值接收者，需要改变接收者数据时使用指针接收者。
2. 看自己的编码习惯。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 函数返回局部变量的指针是否安全</font></p>

一般来说，局部变量会在函数返回后被销毁，因此被返回的指针就成为了“无所指”的引用，程序会进入未知状态。

但这在 go 中是安全的，go 编译器将会对每个局部变量进行逃逸分析。如果发现局部变量的作用域超出该函数范围，则不会将内存分配在栈上，而是分配在堆上，因为他们不在栈区，所以即使释放函数，其内容也不会受影响。

编译时可以借助选项 -gcflags=-m，查看变量逃逸的情况。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go 函数传参到底是值传递还是引用传递</font></p>

go 只有值传递。

引用类型和引用传递是有区别的。

函数使用指针参数时，指针自身的地址是被复制了一份，这个地址是不同的，但是指针内部存储的变量地址是没有变化的，还是指向原有变量的内存空间。

slice 类型：形参和实际参数内存地址一样，不代表是引用类型；下面进行详细说明 slice 还是值传递，传递的是指针，是一个结构体，他的第一个元素是一个指针类型，这个指针指向的是底层数组的第一个元素。当参数是 slice 类型的时候，fmt.printf 通过 %p 打印的 slice 变量的地址其实就是内部存储数组元素的地址，所以打印出来形参和实参内存地址一样。

因为 slice 作为参数时本质是传递的指针，上面证明了指针也是值传递，所以参数为 slice 也是值传递，指针指向的是同一个变量，函数内对形参的修改，会修改原内容数据。

单纯的从 slice 这个结构体看，我们可以通过 modify 修改存储元素的内容，但是永远修改不了 len 和 cap，因为他们只是一个拷贝，如果要修改，那就要传递 &slice 作为参数才可以。

map 类型：形参和实际参数内存地址不一样，证明是值传递，通过 make 函数创建的 map 变量本质是一个 hmap 类型的指针 *hmap，所以函数内对形参的修改，会修改原内容数据。

channel 类型：形参和实际参数内存地址不一样，证明是值传递，通过 make 函数创建的 chan 变量本质是一个 hchan 类型的指针 *hchan，所以函数内对形参的修改，会修改原内容数据。

struct 类型：形参和实际参数内存地址不一样，证明是值传递。形参不是引用类型或者指针类型，所以函数内对形参的修改，不会修改原内容数据。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go defer 关键字的实现原理</font></p>

defer 能够让我们推迟执行某些函数的调用，推迟到当前函数返回前才执行。

defer、panic、recover 结合，形成了 go 语言风格的异常与捕获机制。

优点：方便开发者使用。

缺点：有性能损耗。

Go1.14 中编译器会将 defer 函数直接插入到函数的尾部，无需链表和栈上参数拷贝，性能大幅提升。把 defer 函数在当前函数内展开并直接调用，这种方式被称为 open coded defer（开放编码延迟）

函数退出前，按照先进后出的顺序，执行 defer 函数。

panic 后的 defer 函数不会被执行（遇到 panic，如果没有捕获错误，函数会立刻终止）

panic 没有被 recover 时，抛出的 panic 到当前 goroutine 最上层函数时，最上层程序直接异常终止。

panic 有被 recover 时，当前 goroutine 最上层函数正常执行。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go make 和 new 的区别</font></p>

make 和 new 是内置函数，不是关键字。

变量初始化两步：变量声明 + 变量内存分配。

var 关键字用来声明变量的，new 和 make 函数主要是用来分配内存的。

var 声明值类型时，系统会默认分配内存空间，并赋值该类型的零值。

var 声明引用类型和指针类型时，系统不会分配内存空间，默认就是 nil，如果直接进行使用，系统会发生错误，必须进行内存分配后才能使用。

new 和 make 两个内置函数，主要用来分配内存空间，有了内存，变量就能使用了。

使用场景的区别：

make 只能用来分配及初始化 slice、map、chan 的数据。

new 可以分配任意类型的数据，并且置零。

sli 也可以使用 new 进行内存分配，但是长度和容量都为 0，只能通过内置的 append 函数进行使用。

map 也可以使用 new 进行内存分配，但不能使用。

返回值的区别：

make 函数原型如下：返回的是 slice、map、chan 类型本身。

这 3 中类型就是引用类型，没有必要返回他们的指针。

new 函数返回一个指向该类型内存地址的指针。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 的底层实现原理</font></p>

切片是基于数组实现的，底层是数组，可以理解为对底层数组的抽象。

runtime/slice.go

```
type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

slice 占用 24 个字节 

array：指向底层数组的指针，占用 8 个字节。

len：slice 的长度，占用 8 个字节。

cap：slice 的容量，cap 总是大于等于 len，占用 8 个字节。

4 种初始化方式。

var、:=、make、arr\[0:3]

通过 go tool compile 得到汇编代码。

初始化 slice 调用的是 runtime.makeslice，makeslice 函数的工作主要就是计算 slice 所需内存大小，然后调用 mallocgc 进行内存的分配。

所需内存大小 = 切片中元素大小 * 切片的容量

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go array 和 slice 区别</font></p>

长度不同：

数组初始化必须指定数组长度，并且长度固定不可变。

切片舒适化无需指定切片长度，可以追加元素，在追加时，底层会根据扩容策略进行自动扩容。

函数传参方式不同：

数组是值类型，将一个数组赋值给另一个数组时，传递的是一份深拷贝，函数传参操作都会复制整个数组数据，会占用额外的内存，函数内对数组元素值的修改，不会修改原数组内容。

切片是引用类型，将一个切片复制给另一个切片时，传递的是一份浅拷贝，函数传参操作不会复制整个切片，只会复制 len 和 cap，底层共用同一个底层数组数据，不会占用额外的内容，函数内对数组元素值的修改，会修改原数组内容。

计算长度方式不同：

数组需要遍历计算数组长度，时间复杂度为 O(n)

切片底层包含 len 字段，可以通过 len() 计算切片长度，时间复杂度为 O(1)

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 深拷贝和浅拷贝</font></p>

深拷贝：拷贝的是数据本身，创造一个新对象，新创建的对象与源对象不共享内存，新创建的对象在内存中开辟一个新的内存地址，新对象值修改时不会影响原对象值。

实现深拷贝方式：

1. 使用内置函数 copy(dst,src []Type)；
2. 遍历 append 赋值。

浅拷贝：拷贝的是数据地址，只复制指向的对象的指针，此时新对象和老对象指向的内存地址是一样的，新对象值修改时老对象也会变化。

实现浅拷贝方式：引用类型的变量，默认赋值操作就是浅拷贝。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 扩容机制</font></p>

扩容会发生在 slice append 时候，当 slice 的 cap 不足以容纳新元素，就会进行扩容，扩容规则如下：

1. 如果新申请的容量比原有容量的两倍大，那么扩容后容量大小为新申请的容量；
2. 如果原有 slice 长度小于 1024，那么每次就扩容为原来的 2 倍；
3. 如果原有 slice 长度大于等于 1024，那么每次扩容就扩为原来的 1.25 倍。

新版本为 256。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go slice 为什么不是线程安全的</font></p>

线程安全定义：

1. 多个线程访问同一个对象时，调用这个对象的行为都可以获得正确的结果，那么这个对象就是线程安全的。
2. 若有多个线程同时执行写操作，一般都需要考虑线程同步，否则的话就可能影响线程安全。

go 实现线程安全常用的几种方式：

1. 互斥锁
2. 读写锁
3. 原子操作
4. sync.once
5. sync.atomic
6. channel

slice 底层结构并没有使用加锁等方式，不支持并发读写，所以并不是线程安全的，使用多个 goroutine 对类型为 slice 的变量进行操作，每次输出的值大概率都不会一样，与预期值不一致; slice 在并发执行中不会报错，但是数据会丢失。

# <p style ='background-color:#894e54;text-align:center;'><font color='white'>go map 底层实现原理</font></p>

map 是一个指针，占用 8 个字节，指向 hmap 结构体。

源码包 `runtime/map.go` 定义了 hmap 的数据结构：

hmap 包含若干个结构为 bmap 的数组，每个 bmap 底层都采用链表结构，bmap 通常叫其 bucket(桶)

hmap 结构体：

```
// A header for a Go map.
type hmap struct {
    count      int            // 代表哈希表中的元素个数，调用 len(map) 时，返回的就是该字段值。
    flags      uint8          // 状态标志（是否处于正在写入的状态等）
    B          uint8          // buckets（桶）的对数，如果 B=5，则 buckets 数组的长度 = 2^B =32，意味着有32个桶
    noverflow  uint16         // 溢出桶的数量
    hash0      uint32         // 生成 hash 的随机数种子
    buckets    unsafe.Pointer // 指向 buckets 数组的指针，数组大小为 2^B，如果元素个数为 0，它为 nil。
    oldbuckets unsafe.Pointer // 如果发生扩容，oldbuckets 是指向老的 buckets 数组的指针，老的 buckets 数组大小是新的 buckets 的 1/2；非扩容状态下，它为 nil。
    nevacuate  uintptr        // 表示扩容进度，小于此地址的 buckets 代表已搬迁完成。
    extra *mapextra           // 存储溢出桶，这个字段是为了优化 GC 扫描而设计的，下面详细介绍
 }

```

# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>
# <p style ='background-color:#894e54;text-align:center;'><font color='white'></font></p>


