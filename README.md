# Rust

- tip
    - cargo 시작
        
        ```jsx
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
        
        ```jsx
        //psudo code
        FUNCTION main {
        	INTEGER a = 2
        	CALL stack_only(a)
        }
        
        FUNCTION stack_only(INTEGER b){
        	INTEGER c = 3
        }
        ```
        
        ![Untitled](Rust%20c6953706361e478797e176d87945f9ae/Untitled.png)
        
    - heap
        - It's a region of the process memory that is NOT automatically managed.
        - It has no size restrictions.
        - It's accessible by any function, anywhere in the program.
        - heap allocations are expensive and we should avoid them when possible
        - fragmented 되어 있음(stack처럼 차곡차곡 쌓는 구조가 아님)
        
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
        
        ![Desktop - 1.png](Rust%20c6953706361e478797e176d87945f9ae/Desktop_-_1.png)
        
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
    - functions
        -