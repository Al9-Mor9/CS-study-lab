# Ch04.Process Management

# 실습 1. 프로세스의 생성과 종료

## 목차

```
1. 프로세스 생성
	- 부모 프로세스로부터 자식 프로세스를 생성해보자 + 기본 팁
	- 자식 프로세스에서 외부 프로그램을 실행해보자
	- 자식 프로세스가 완료될 때까지 부모 프로세스를 기다리게 해보자
2. 프로세스 종료
	- 자식 프로세스가 스스로 꺼지도록 종료해보자
	- 부모 프로세스에서 자식 프로세스를 강제 종료시켜보자
```

## 1. 프로세스의 생성

프로세스는 부모로부터 자식이 만들어지는, 트리 형태의 계층 구조를 이루고 있습니다.

부모 프로세스로부터 자식 프로세스들을 생성해보고, 각 프로세스의 진행은 어떻게 이루어지는지 확인해봅시다.

### 1-1. 부모 프로세스로부터 자식 프로세스를 생성해보자 + 기본 팁

프로세스의 생성은 `fork()` 시스템콜에 의해 이루어집니다. 다들 우분투를 설치하셨다면 ctrl + alt + t를 눌러 터미널을 열어봅시다.

![Terminal](./img/terminal.png)

리눅스에서는 이 `fork()`를 포함한 시스템 콜, 라이브러리 콜, 명령어 등에 대한 기본 매뉴얼을 제공하고 있는데, 이 매뉴얼은 `man` 명령어를 통해서 열 수 있습니다. `fork()`의 세부 매뉴얼을 보기 위해 `man fork`를 입력해봅시다.

![이 매뉴얼은 q를 눌러 끌 수 있습니다.](./img/man.png)

이 매뉴얼은 q를 눌러 끌 수 있습니다.

매뉴얼을 읽어보면 `fork()` 가 어떤 값을 리턴하고 어떤 기능을 하는 함수인지 쭉 나와있으니 이후의 실습에서도 참고하시길 바랍니다.

이후 실습은 Ubuntu에서도 VSCode를 설치할 수 있으니, VSCode를 사용해도 좋고, 터미널에서 VIM을 써도 됩니다.

---

우선 `fork()` 함수에 대해 알아보면 다음과 같습니다.

```c
#include <sys/types.h>
#include <unistd.h>
pid_t fork(void); //pid_t는 int값임
//Returns: 0 to child, PID of child to parent, −1 on error
```

`fork()` 는 자식 프로세스를 만드는 함수로 여러 특징들이 있는데, 그 중 하나는 바로 ************************************************************************************한 번 호출되지만 두 번 리턴한다************************************************************************************는 것입니다. `fork()` 함수는 부모 프로세스에서 한 번 호출되지만, 부모 프로세스에게는 자식 프로세스의 PID를, 자식 프로세스에게는 0을 리턴해 총 두 번 리턴합니다.

아래의 코드를 실행해봅시다.

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>

int main(){
    int PID = fork();

    if (PID == 0){//PID가 0이면 자식 프로세스
        printf("I am child process. I got %d from fork() function\n", PID);
    }
    else if (PID > 0){//PID > 0이면 부모 프로세스. PID는 자식 프로세스의 PID
        printf("I am parent process. I got %d from fork() function\n", PID);
    }
}
```

이를 실행해보면 child process는 `fork()`로부터 0을 리턴받고, parent process는 `fork()`로부터 양의 정수 값을 리턴받았음을 확인할 수 있습니다. 그런데 이상한 점은 위 코드를 몇 번 실행해보면 출력의 순서가 뒤바뀌기도 한다는 점입니다.

```
(출력 예1)
I am parent process. I got value of 78411 from fork() function
I am child process. I got value of 0 from fork() function

(출력 예2)
I am child process. I got value of 0 from fork() function
I am parent process. I got value of 78411 from fork() function
```

이렇게 출력되는 것들의 순서가 일정하지 않은 이유는 `fork()`가 된 이후부터 부모 프로세스와 자식 프로세스는 **동시에 돌아가는** 별개의 프로세스가 되기 때문입니다.

---

`fork()` 의 두 번째 특징으로는, **자식 프로세스는 부모 프로세스를 복제하지만 그 둘은 별개의 메모리 공간을 차지한다**는 것입니다.

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>

int main(){
    int PID = fork();
		int x = 123;

    if (PID == 0){//PID가 0이면 자식 프로세스
        x++;
        printf("I am child process. my x is %d\n", x);
    }
    else if (PID > 0){//PID > 0이면 부모 프로세스. PID는 자식 프로세스의 PID
        x--;
        printf("I am parent process. my x is %d\n", x);
    }
}
```

위 코드를 실행하면 자식 프로세스의 `x`는 124, 부모 프로세스의 `x`는 122가 됐음을 알 수 있습니다. 자식 프로세스는 123이라는 처음에는 부모의 `x` 값을 복제하지만 이후 둘은 별개의 메모리 공간을 가지는, 서로 다른 프로세스가 됐기 때문입니다. 

### 1-2. 자식 프로세스에서 외부 프로그램을 실행해보자

## 2. 프로세스 종료