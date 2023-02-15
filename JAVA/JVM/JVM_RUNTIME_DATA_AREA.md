# JVM - Runtime Data Area
  <br>


### 1-2.Run-Time Data Areas
> The Java Virtual Machine defines various run-time data areas that are used during execution of a program. Some of these data areas are created on Java Virtual Machine start-up and are destroyed only when the Java Virtual Machine exits. Other data areas are per thread. Per-thread data areas are created when a thread is created and destroyed when the thread exits.

Java Virtual Machine은 프로그램 실행 중에 사용되는 다양한 런타임 데이터 영역을 정의합니다. 이러한 데이터 영역 중 일부는 Java Virtual Machine 시작 시 생성되며 Java Virtual Machine이 종료될 때만 삭제됩니다. 다른 데이터 영역은 스레드당입니다. 스레드별 데이터 영역은 스레드가 생성될 때 생성되고 스레드가 종료될 때 삭제됩니다. 

#### 1-2-1.The pc Register

> Each Java Virtual Machine thread has its own pc (program counter) register. At any point, each Java Virtual Machine thread is executing the code of a single method, namely the current method. If that method is not native, the pc register contains the address of the Java Virtual Machine instruction currently being executed.  

각 Java Virtual Machine 스레드에는 자체 PC(프로그램 카운터) 레지스터가 있습니다. 모든 시점에서 각 Java Virtual Machine 스레드는 단일 메서드, 즉 해당 스레드에 대한 현재 메서드의 코드를 실행합니다. 해당 메서드가 네이티브가 아닌 경우 pc 레지스터에는 현재 실행 중인 Java 가상 시스템 명령의 주소가 포함됩니다. 스레드에서 현재 실행 중인 메서드가 네이티브인 경우 Java Virtual Machine의 PC 레지스터 값이 정의되지 않습니다.

#### 1-2-2. JVM Stacks

> Each Java Virtual Machine thread has a private Java Virtual Machine stack, created at the same time as the thread. A Java Virtual Machine stack stores frames (§2.6). A Java Virtual Machine stack is analogous to the stack of a conventional language such as C: it holds local variables and partial results, and plays a part in method invocation and return. Because the Java Virtual Machine stack is never manipulated directly except to push and pop frames, frames may be heap allocated. The memory for a Java Virtual Machine stack does not need to be contiguous.

각 Java 가상 시스템 스레드에는 스레드와 동시에 생성된 전용 Java 가상 시스템 스택이 있습니다. Java Virtual Machine 스택은 프레임을 저장합니다($2.6). 자바 가상 머신 스택은 C와 같은 전통적인 언어의 스택과 유사하다. 자바 가상 머신 스택은 푸시 프레임과 팝 프레임을 제외하고는 직접 조작되지 않기 때문에 프레임이 힙 할당될 수 있다. Java 가상 시스템 스택의 메모리는 연속적일 필요가 없습니다.

> If the computation in a thread requires a larger Java Virtual Machine stack than is permitted, the Java Virtual Machine throws a StackOverflowError.

스레드의 계산에 허용된 것보다 더 큰 Java Virtual Machine 스택이 필요한 경우 Java Virtual Machine은 StackOverflow Error를 발생시킵니다.

> If Java Virtual Machine stacks can be dynamically expanded, and expansion is attempted but insufficient memory can be made available to effect the expansion, or if insufficient memory can be made available to create the initial Java Virtual Machine stack for a new thread, the Java Virtual Machine throws an OutOfMemoryError.

Java Virtual Machine 스택을 동적으로 확장할 수 있고 확장을 시도하지만 확장에 사용할 수 있는 메모리가 부족하거나 새 스레드에 대한 초기 Java Virtual Machine 스택을 만드는 데 사용할 수 있는 메모리가 부족한 경우 Java Virtual Machine은 Out Of Memory Error를 발생시킵니다.

#### 1-2-3. Heap

> The Java Virtual Machine has a heap that is shared among all Java Virtual Machine threads. The heap is the run-time data area from which memory for all class instances and arrays is allocated.

Java 가상 시스템에는 모든 Java 가상 시스템 스레드 간에 공유되는 힙이 있습니다. 힙은 모든 클래스 인스턴스 및 어레이에 대한 메모리가 할당되는 런타임 데이터 영역입니다.

> The heap is created on virtual machine start-up. Heap storage for objects is reclaimed by an automatic storage management system (known as a garbage collector); objects are never explicitly deallocated. The Java Virtual Machine assumes no particular type of automatic storage management system, and the storage management technique may be chosen according to the implementor's system requirements. The heap may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger heap becomes unnecessary. The memory for the heap does not need to be contiguous.

힙은 가상 시스템을 시작할 때 생성됩니다. 개체에 대한 힙 스토리지는 가비지 수집기라고 하는 자동 스토리지 관리 시스템에 의해 회수되며, 개체 할당이 명시적으로 해제되지 않습니다. Java Virtual Machine은 특정 유형의 자동 스토리지 관리 시스템을 가정하지 않으며, 구현자의 시스템 요구 사항에 따라 스토리지 관리 기술을 선택할 수 있습니다. 힙은 고정된 크기이거나 계산에 의해 필요에 따라 확장될 수 있으며 더 큰 힙이 필요하지 않을 경우 수축될 수 있다. 힙의 메모리는 연속적일 필요가 없습니다.

#### 1-2-4. Method Area

> The Java Virtual Machine has a method area that is shared among all Java Virtual Machine threads. The method area is analogous to the storage area for compiled code of a conventional language or analogous to the "text" segment in an operating system process. It stores per-class structures such as the run-time constant pool, field and method data, and the code for methods and constructors, including the special methods used in class and interface initialization and in instance initialization 

JVM(Java Virtual Machine)에는 모든 JVM(Java Virtual Machine) 스레드 간에 공유되는 메소드 영역이 있습니다. 메서드 영역은 기존 언어의 컴파일된 코드를 위한 저장 영역과 유사하거나 운영 체제 프로세스의 "텍스트" 세그먼트와 유사합니다. 클래스 및 인터페이스 초기화와 인스턴스 초기화( §2.9 ) 에 사용되는 특수 메서드를 포함하여 런타임 상수 풀, 필드 및 메서드 데이터, 메서드 및 생성자 코드와 같은 클래스별 구조를 저장합니다.

> The method area is created on virtual machine start-up. Although the method area is logically part of the heap, simple implementations may choose not to either garbage collect or compact it. This specification does not mandate the location of the method area or the policies used to manage compiled code. The method area may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger method area becomes unnecessary. The memory for the method area does not need to be contiguous.

메서드 영역은 가상 머신 시작 시 생성됩니다. 메서드 영역은 논리적으로 힙의 일부이지만 간단한 구현에서는 가비지 수집 또는 압축을 선택하지 않을 수 있습니다. 이 사양은 메서드 영역의 위치나 컴파일된 코드를 관리하는 데 사용되는 정책을 요구하지 않습니다. 메소드 영역은 고정된 크기일 수도 있고 계산에 필요한 만큼 확장될 수도 있으며 더 큰 메소드 영역이 불필요해지면 축소될 수도 있습니다. 메서드 영역의 메모리는 연속적일 필요가 없습니다.

#### 1-2-5. Run-Time Constant Pool

> A run-time constant pool is a per-class or per-interface run-time representation of the constant_pool table in a class file (§4.4). It contains several kinds of constants, ranging from numeric literals known at compile-time to method and field references that must be resolved at run-time. The run-time constant pool serves a function similar to that of a symbol table for a conventional programming language, although it contains a wider range of data than a typical symbol table.

런타임 상수 풀은constant_pool 파일 에 있는 테이블 의 클래스별 또는 인터페이스별 런타임 표현입니다 class( §4.4 ). 여기에는 컴파일 타임에 알려진 숫자 리터럴에서 런타임에 확인되어야 하는 메서드 및 필드 참조에 이르기까지 여러 종류의 상수가 포함됩니다. 런타임 상수 풀은 일반적인 기호 테이블보다 더 넓은 범위의 데이터를 포함하지만 기존 프로그래밍 언어의 기호 테이블과 유사한 기능을 수행합니다.

#### 1-2-6. Native Method Stacks

> An implementation of the Java Virtual Machine may use conventional stacks, colloquially called "C stacks," to support native methods (methods written in a language other than the Java programming language). Native method stacks may also be used by the implementation of an interpreter for the Java Virtual Machine's instruction set in a language such as C. Java Virtual Machine implementations that cannot load native methods and that do not themselves rely on conventional stacks need not supply native method stacks. If supplied, native method stacks are typically allocated per thread when each thread is created.

nativeJVM(Java Virtual Machine)의 구현은 메서드(Java 프로그래밍 언어 이외의 언어로 작성된 메서드)를 지원하기 위해 구어체로 "C 스택"이라고 하는 기존 스택을 사용할 수 있습니다 . 원시 메소드 스택은 C와 같은 언어로 된 JVM(Java Virtual Machine) 명령어 세트에 대한 인터프리터 구현에 의해 사용될 수도 있습니다. 메소드를 로드할 수 없고 자체적으로 기존 스택에 의존하지 않는 JVM(Java Virtual Machine) 구현은 원시 메소드 스택을 제공할 필요가 없습니다 native . . 제공되는 경우 네이티브 메서드 스택은 일반적으로 각 스레드가 생성될 때 스레드별로 할당됩니다









### Reference
- https://docs.oracle.com/javase/specs/jvms/se11/html/index.html
- https://coding-factory.tistory.com/828
- https://catch-me-java.tistory.com/38
