# LLVM-Start
same as in the report.
## 1. LLVM Installation
````
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
sudo ./llvm.sh all 我一直在这步出错，原因是dkpg interrupted
export PATH=$PATH:/lib/llvm-17/bin
echo "export PATH=\$PATH:/lib/llvm-17/bin/" >> ~/.profile
````
### Check if successfully installed:
````
opt --version 
````
### if "dkpg interrupted":
````
sudo rm /var/lib/dpkg/updates/*
sudo apt-get update
sudo apt-get upgrade
````
## 2. Write a test.c
````
nano test.c
````
### and insert:
````
// test.c
#include <stdio.h>

int add(int a, int b) {
    return a + b;
}

int main() {
    int x = 5;
    int y = 10;
    int result = add(x, y);

    printf("The sum of %d and %d is: %d\n", x, y, result);

    return 0;
}
````
"ctrl+0+enter" to save, "ctrl+X" to exit, "enter" to assure the name.
## 3. Translation

Code Format:

（1）source（.c）to binary executable：
````
clang test.c -o test_binary
````
（2）source（.c）到目标文件（.o）：
````
clang -c test.c -o test.o
````
（3）source（.c）to Assembly Code（.s）：
````
clang -S -mllvm --x86-asm-syntax=intel test.c -o test.s
````
（4) source（.c）to LLVM Bitcode（.bc）：
````
clang -c -emit-llvm test.c -o test.bc
````
（5）source（.c）to LLVM IR（.ll）：
````
clang -S -emit-llvm test.c -o test.ll
````

LLVM IR Format：
（1）LLVM IR（.ll）to LLVM Bitcode（.bc）：
````
llvm-as test.ll -o test.bc
````

（2）LLVM Bitcode（.bc）to LLVM IR（.ll）：
````
llvm-dis test.bc -o test.ll
````
LLVM IR（.ll）to Assembly Code（.s）：
````
llc test.ll -o test.s
````
Explain LLVM IR，without compiling：
````
lli test.ll
````

## 4. Generate Control Flow Graph
### Install graphviz 

````
sudo apt install graphviz
````
### Visualize .ll(IR)
````
opt -passes=dot-cfg -disable-output test.ll
ls -la
//dot -Tpng .function_name.dot -o function_name.png//template
dot -Tpng .add.dot -o add.png
````



