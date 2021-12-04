- tip
    - cargo 시작
        
        ```bash
        source /usr/local/cargo/env
        ```
        
    - 현재 버전 확인
        
        ```bash
        rustup toolchain list
        ```
        
    - nightly 버전 설치
        
        ```bash
        rustup toolchain install nightly
        ```
        
    - gdb 디버거를 통해 코드 한줄한줄 스택, 힙메모리 등등을 다 확인할 수 있다(신세계...!!!!)
        
        [GDB: The GNU Project Debugger](https://www.gnu.org/software/gdb/)
        
- memory management
    - stack
        - stack is a special region of the process memory that stores variables created by each function.
        - memory for each function is called a stack frame
        - for every function all a new stack frame is allocated on top of the current one
        - The size of every variable on the stack has to be known at compile time.
            
            (사이즈가 정해져야 한다)
            
        - when a function exits, It's that frame is released.
        
        ```rust
        //psudo code
        FUNCTION main {
        	INTEGER a = 2
        	CALL stack_only(a)
        }
        
        FUNCTION stack_only(INTEGER b){
        	INTEGER c = 3
        }
        ```
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b87e1c45-51af-45ce-9514-9ef58d8aec8f/Untitled.png)
        
    - heap
        - It's a region of the process memory that is NOT automatically managed.
        - It has no size restrictions.
        - It's accessible by any function, anywhere in the program.
        - heap allocations are expensive and we should avoid them when possible
        - fragmented 되어 있음(stack처럼 차곡차곡 쌓는 구조가 아님)
        - string 은 heap에 저장된다(컴파일타임에 사이즈가 정해지지 않는다)
        
        ```rust
        fn main() {
            let a = 2;
            let result = stack_only(a);
            dbg!(result);
        }
        
        fn stack_only(b: i32) -> i32 {
            let c = 3;
            return b + c + stack_and_heap();
        }
        
        fn stack_and_heap() -> i32 {
            let d = 5;
            let e = Box::new(7);
            return d + *e;
        }
        ```
        
        ![Desktop - 1.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79f2cf31-5690-4ab3-98c8-ffe9775e1c80/Desktop_-_1.png)
        
    - smart pointer
        - smart pointer is just a wrapper around the roll pointer
        - 메모리 해제를 위한게 주요한 기능(나중에 맞는지 찾아보기)
- basic data types
    - primitve type
        - booleans
        - characters
            - single unicode value(4 bytes)
        - integers
            - u : unsigned
            - i : signed
        - floats
        - 선언 : immutable, mutable 두가지 방식
            - rust 는 타입 추론 가능 let a = 1; 이런 식으로 초기화
            - 명시적 초기화 let a: 32 = 1.0;
    - functions
        - function signature must declare the number and the type of the parameters
    - macro
        - macros are used for metaprogramming
        - can be code with a variable number of parameters and with different types
        - cargo-expand 패키지 설치 후 cargo expand 명령어 입력시 매크로가 펼쳐져서 나온다
    - String
        - scope를 벗어나게 되면 drop function이 실행됨(smart pointer와 유사)
        - drop function 은 다른 언어의 destructor와 비슷함
        - the pattern of the allocating values at the end of their lifetime is called raii(Resource acquisition is initialization)
- ownership
    - three ownership in rust
        - Each value in rust is ownered by a variable
        - When the owner goes out of scope, the value will be deallocated
        - There can  only be ONE owner at a time.
    - By guaranteeing a single owner, rust eliminates the possibility of a double free error
        
        ```rust
        let mut one = String::new();
        let mut two = one;
        // 이렇게 되면 소유권이 two로 넘어감
        ```
        
- References and Borrowing
    - references : allow us to refer to a valie without taking ownership of it
    - &String → 과같이 파라미터 등에서 사용(포인터를 넘기는거임)
    - mutable borrow 후에는 다른 borrow를 사용할 수 없다. (immutable은 여러번 가능)
        
        ```rust
        let mut s1 = &mut input;
        // 불가 let mut s2 = &mut input;
        // 불가 let s3 = &input;
        ```
        
    - dd