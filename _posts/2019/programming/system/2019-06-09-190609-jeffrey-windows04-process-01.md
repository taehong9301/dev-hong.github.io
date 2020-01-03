---
title: "프로세스 (Windows system programming) Section01"
excerpt: "제프리 리처의 Windows via C/C++, 4장 프로세스 Process, Section01"
search: true
categories:
  - Programming
  - System
tags:
  - Process
  - Windows System Programming
last_modified_at: 2019-06-09T17:10:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: /assets/images/thumbnail/2019/jeffrey-windows04-process.png
example_gallery:
  - url:
    image_path:
    alt: ""
    title: ""
---

# Chapter04. 프로세스

시스템이 수행 중인 프로세스를 어떻게 관리하는지에 대해 알아보자.

프로세스는 일반적으로 수행 중인 프로그램의 인스턴스(instance)라고 정의한다.

프로세스는 2개의 컴포넌트로 구성된다. (커널 오브젝트와 메모리 공간)

- 프로세스를 관리하기 위한 목적으로 커널 오브젝트를 가지고 있고, 커널 오브젝트에는 프로세스에 대한 각종 통계 정보를 저장하고 있다.
- 실행 모듈, DLL(dynamic-link library)의 코드와 데이터를 수용하는 주소공간이 있다. 이런 주소 공간은 스택, 힙과 같은 동적 메모리 할당에 사용되는 공간도 포함된다.

## 스레드

프로세스는 자력으로 수행될 수 없고, 무언가를 수행하기 위해서는 반드시 프로세스의 컨텍스트 내에서 수행되는 스레드(thread)가 있어야 한다.

스레드는 프로세스의 주소 공간에 위치한 코드를 수행한다. 하나의 프로세스는 다수의 스레드를 가질 수 있다. 이러한 스레드는 프로세스 내에서 동시에 실행된다.

각 스레드는 자신만의 CPU 레지스터(register) 집합과 스택(stack)을 가져야 한다.

프로세스가 생성되면 시스템은 자동적으로 첫번째 스레드를 생성한다. 이렇게 생성된 첫번째 스레드는 주 스레드(primary thread)라고 부른다. 추가적으로 스레드를 더 생성할 수 있다.

프로세스의 주소 공간 내의 코드를 수행할 스레드가 없다면, 프로세스는 계속해서 존재해야 할 이유가 없다. 따라서 시스템은 자동적으로 프로세스와 프로세스 주소공간을 파괴한다.

## 스레드와 스케줄링

운영체제는 모든 스레드가 동시에 수행될 수 있도록 CPU 시간을 조금씩 나누어준다. 이때 라운드 로빈(round-robin)방식으로 주어지고, 이때 주어지는 단위 시간을 퀀텀(quantum)이라고 부른다. 실제로는 동시에 수행되는것이 아니지만 단위시간이 짧아 동시에 실행되는 것처럼 보인다.

다수의 CPU가 있는 경우 공평하게 CPU시간을 분배하는 알고리즘이 상당히 복잡하다. 윈도우의 경우는 각 CPU 별로 서로 다른 스레드를 수행하도록 스케줄링하고 있다. 그리고 스레드에 대한 모든 관리와 스케줄링은 커널(kernel)에서 담당한다.

## Section01. 첫 번째 윈도우 애플리케이션 작성

윈도우에는 2가지 형태의 애플리케이션이 있다.

- GUI: 그래픽 유저 인터페이스(Graphic User Interface)
  - GUI 기반 애플리케이션: 메모장(Notepad), 계산기(Calculator), 워드패드(WordPad) 등등
- CUI: 콘솔 유저 인터페이스(Console User Interface)
  - CUI 기반 애플리케이션은 일반적으로 윈도우를 생성하지 않는 텍스트 기반으로 된 애플리케이션이다. 대표적으로 명령 프롬프트(command prompt), 즉 cmd.exe와 같은 애플리케이션이라고 할 수 있다.

### 링커 스위치(Linker Switch)

사용자가 애플리케이션을 수행하면 운영체제의 로더(loader)는 실행 파일의 헤더를 확인하여 서브시스템 값을 가져온다.

Visual Studio을 이용하면 실행 파일의 형태에 맞는 서브 시스템(subsystem) 타입을 실행 파일에 포함시킬 수 있도록, 다양한 링커 스위치(linker switch)를 설정한다.

- CUI 기반: `/SUBSYSTEM:CONSOLE`
- GUI 기반: `/SUBSYSTEM:WINDOWS`

만약 헤더의 서브시스템 값이 CUI인 경우 운영체제의 로더는 콘솔 윈도우를 사용할 수 있도록 조치한다. 반대로 GUI인 경우 콘솔 윈도우를 생성하지 않고, 애플리케이션을 바로 로드한다. 그리고 운영체제는 더 이상 어떤 형태의 애플리케이션인지 신경쓰지 않는다.

### 진입점 함수

윈도우 애플리케이션은 수행을 시작할 진입점 함수를 반드시 가져야한다. 2가지 형태의 진입점 함수를 가질 수 있다. 아래 예제를 보자.

```c
int WINAPI _tWinMain(
    HINSTANCE hInstanceExe,
    HINSTANCE,
    PTSTR pszCmdLine,
    int nCmdShow);
```

```c
int _tmain(
    int argc,
    TCHAR *argv[],
    TCAHR *envp[]);
```

어떤 진입점 함수를 사용할지는 유니코드 문자열 사용 여부에 달려있다. 운영체제는 사용자가 작성한 진입점 함수를 직접 호출하지 않으며, C/C++ 런타임에 의해 구현된 C/C++ 런타임 시작 함수(C/C++ runtime startup function)를 호출한다.

| 애플리케이션 타입                             | 진입점               | 실행 파일에 포함되는 런타임 시작 함수 |
| --------------------------------------------- | -------------------- | ------------------------------------- |
| ANSI 문자(열)를 사용하는 GUI 애플리케이션     | \_tWinMain(WinMain)  | WinMainCRTStartup                     |
| 유니코드 문자(열)를 사용하는 GUI 애플리케이션 | \_tWinMain(wWinMain) | wWinMainCRTStartup                    |
| ANSI 문자(열)를 사용하는 CUI 애플리케이션     | \_tmain(main)        | mainCRTStartup                        |
| 유니코드 문자(열)를 사용하는 CUI 애플리케이션 | \_tmain(wmain)       | wmainCRTStartup                       |

링커는 실행 파일을 링크하는 단계에서 적절한 C/C++ 런타임 시작 함수를 선택한다. 만약 링커 스위치가 `/SUBSYSTEM:WINDOWS` 로 설정되어 있으면, 링커는 WinMain 이나 wWinMain 함수를 찾는다. 하지만, 이런 함수가 없는 경우 "unresolved external symbol" 에러를 반환한다. 그리고, 함수를 정상적으로 찾은 경우 WinMainCRTStartup 이나 wWinMainCRTStartup 를 호출한다.

반면, `/SUBSYSTEM:CONSOLE` 인 경우 링커는 main 또는 wmain을 찾는다. 마찬가지로 없는 경우 unresolved external symbol 에러를 반환한다. 그리고 함수를 정상적으로 찾은 경우 mainCRTStartup 또는 wmainCRTStartup 함수를 호출한다.

그런데, 프로젝트 설정에서 `/SUBSYSTEM` 링커 스위치를 제거 할 수 있는데, 제거를 하게 되면 링커는 자동적으로 애플리케이션에 적합한 설정 값을 찾아낸다. 그리고 링커는 `WinMain`, `wWinMain`, `main`, `wmain` 중 적합한 하나를 추정하여, 서브시스템으로 설정한다.

조심해야할 점은 프로젝트 생성시 Win32 애플리케이션 프로젝트를 생성하고 진입점 함수로 main 함수를 사용하는 경우다. Win32 애플리케이션은 `/SUBSYSTEM:WINDOWS` 로 링커 스위치가 설정되어 있다. 그렇기 떄문에 WinMain 이나 wWinMain 함수를 이용해야 한다. 그래서, main 함수를 이용한 경우 에러를 반환하게 된다. 이런 경우 개발자는 문제를 해결하기 위한 방법이 4가지가 있다.

1. main 함수를 WinMain으로 바꾼다. (개발자가 만드려고 하는 애플리케이션이 GUI인 경우)
2. 프로젝트를 생성할 때, 콘솔 애플리케이션으로 새로 만든다. (개발자가 만드려고 하는 애플리케이션이 CUI인 경우)
3. 2번처럼 새로 만드는 방법보다는 프로젝트 속성 다이얼로그 박스에서 링크 탭을 눌러 `/SUBSYSTEM` 의 값을 바꿀 수 있다. 새로운 프로젝트를 만들 필요없이 간단히 속성만 바꾸면되서 편하다.
4. 또는 `/SUBSYSTEM:WINDOWS` 링커 스위치 값을 완전히 제거하는 방법이 있다. 위에서 말했듯이 링커는 소스 코드 상에 어떤 함수가 구현되어 있는지 확인하여 적절하게 처리한다.

### C/C++ 런타임 함수 기본 작업

C/C++ 런타임 시작 함수는 진입점 함수가 어떤 것인지 확인하고 ANSI 문자열인지 또는 유니코드 문자열을 처리하는지 확인한다.

그리고 Visual C++에는 C/C++ 런타임 라이브러리의 소스 코드가 포함되어있는데, crtexe.c 파일을 살펴보면 시작함수가 수행하는 작업을 아래 4가지로 요약할 수 있다.

- 새로운 프로세스의 전체 명령행을 가리키는 포인터를 획득한다.
- 새로운 프로세스의 환경변수를 가리키는 포인터를 획득한다.
- C/C++ 런타임 라이브러리의 전역변수를 초기화 한다. 사용자 코드가 StdLib.h 파일을 포함하면 이 변수에 접근할 수 있다.
- C/C++ 런타임 라이브러리의 메모리 할당 함수(malloc과 calloc)와 저수준 입출력 루틴이 사용하는 힙을 초기화한다.
- 모든 전역 오브젝트와 static C++ 클래스 오브젝트의 생성자를 호출한다.

이러한 이러한 초기화 과정이 모두 완료되고 나서 C/C++ 시작 함수는 애플리케이션의 진입점 함수를 호출한다.

#### \_tWinMain 함수를 구현하고 \_UNICODE 인 경우

```c
GetStartupInfo(&StartupInfo);
int nMainRetVal = wWinMain(
    (HINSTANCE) &__ImageBase, // 링커가 정의하는 가상의 변수
    NULL,
    pszCommandLineUnicode, // UNICODE
    (StartupInfo.dwFlags & STARTF_USESHOWWINDOW) ? StartupInfo.wShowWindow : SW_SWOHDEFAULT);
```

- `&__ImageBase`: 링커가 정의하는 가상의 변수. 메모리의 어느 위치에 실행 파일을 로드하였는지 알려주는 값.

#### \_tWinMain 함수를 구현하고 \_UNICODE 가 정의되지 않은 경우

```c
GetStartupInfo(&StartupInfo);
int nMainRetVal = WinMain(
    (HINSTANCE) &__ImageBase,
    NULL,
    pszCommandLineAnsi, // ANSI
    (StartupInfo.dwFlags & STARTF_USESHOWWINDOW) ? StartupInfo.wShowWindow : SW_SWOHDEFAULT);
```

#### \_tmain 함수를 구현하고 \_UNICODE 가 정의된 경우

```c
int nMainRetVal = wmain(argc, argv, envp);
```

#### \_tmain 함수를 구현하고 \_UNICODE 가 정의되지 않은 경우

```c
int nMainRetVal = main(argc, argv, envp);
```

---

Visual Studio의 위저드(wizard)를 통해 애플리케이션을 생성하게 되면, CUI 애플리케이션의 진입점 함수에서 3번째 매개변수는 포함되지 않는다.

```c
int _tmain(int argc, TCHAR* argv[]);
```

만일 프로세스의 환경변수에 접근할 필요가 있다면 `TCHAR* env[]`를 추가한다.

```c
int _tmain(int argc, TCHAR* argv[], TCHAR* env[]);
```

env 매개변수는 '환경변수 이름 = 값'의 형태로 모든 환경변수를 포함하는 배열을 가리키고 있다.

### exit 함수

진입점 함수가 수행되고 반환이 되면, 진입점 함수의 반환값 nMainRetVal을 인자로 하여, C/C++ 런타임 라이브러리의 exit 함수를 호출한다. exit 함수는 아래와 같은 작업을 수행한다.

- `_onexit` 함수를 이용하여 등록해둔 함수를 호출한다.
- 모든 전역 클래스 오브젝트와 static C++ 클래스 오브젝트의 파괴자를 호출한다.
- DEBUG 빌드의 경우, `_CRTDBG_LEAK_CHECK_DF` 플래그가 설정되어 있으면 C/C++ 런타임 메모리에서의 메모리 누수 상황을 `_CrtDumpMemoryLeaks` 함수를 호출하여 나열한다.
- `nMainRetVal` 값을 인자로 하여 `ExitProcess` 함수를 호출한다. 이 함수를 호출하면 운영체제는 프로세스를 종료하고 프로세스의 종료코드(exit code)를 설정한다.

위의 변수들은 보안상의 이유로 모두 사용하지 않도록 설정되어 있다. Windows API의 관련 함수를 직접호출하는 것이 좋다.

### 프로세스 인스턴스 핸들

모든 실행 파일과 DLL 파일은 프로세스가 메모리 공간내에 로드 될 때, **고유의 인스턴스 핸들을 할당 받는다.** 할당 받은 인스턴스 핸들은 `(w)WinMain` 함수의 첫 번째 매개변수 `hInstanceExe`를 통해 전달된다. **이 핸들값은 보통 리소스를 로드할 때 사용**된다.

```c
// 예: 실행파일 이미지에 포함된 아이콘 리소스를 로드하려는 경우
HICON LoadIcon(
  HINSTANCE hInstance,
  PCTSTR pszIcon);
```

많은 애플리케이션에서 `(w)WinMain`의 `hInstanceExe` 매개변수를 전역변수에 저장해 두어 실행 파일의 전체 소스에서 이 값을 쉽게 사용할 수 있도록 한다.

SDK 문서를 살펴보면 HMODULE을 인자로 받는 함수가 있는데, 아래와 같이 `GetModuleFileName` 함수가 있다.

```c
DWORD GetModuleFileName (
  HMODULE hInstance,
  PTSTR pszPath,
  DWORD cchPath);
```

<i class="fas fa-feather-alt"></i> **Note** HMODULE과 HINSTANCE는 동일하다. 예전 16비트 윈도우에서는 HMODULE과 HINSTANCE가 구분되어 사용되었지만 지금은 혼용하여 사용하고 있다.
{: .notice--info}

실제론 hInstanceExe의 값은 시스템이 프로세스의 메모리 주소 공간 상에 실행 파일을 로드할 **시작 메모리 주소(base memory address)다.** 그리고 이 시작 주소는 링커에 의해서 결정된다.

서로 다른 링커는 서로 다른 시작 주소를 가질 수 있다. Visual Studio의 링커는 `0x00400000`을 기본 시작 주소로 사용한다. 그 이유는 윈도우 98에서 실행 파일을 로드할 수 있는 가장 하단의 메모리 주소였기 때문이다.

만약 애플리케이션이 로드되는 시작 주소를 변경하고 싶다면 `/BASE:address` 옵션을 사용하여 변경 할 수 있다.

GetModuleHandle 함수는 실행파일 또는 DLL 파일이 프로세스 메모리 공간상에 어디에 로드되어 있는지를 가리키는 핸들/시작 주소를 반환한다.

```c
HMODULE GetModuleHandle(PCTSTR pszModule);
```

해당 함수를 사용할때는 인자로 프로세스 메모리 공간 상에 로드 된 실행파일 또는 DLL 파일의 이름을 `\0`으로 끝나는 문자열로 전달하면 된다. 그러면 해당 함수는 시작 주소 값(핸들 값)을 반환한다. 반면, 찾을 수 없다면 NULL을 반환한다.

만약 인자(pszModule)로 NULL값을 넘겨주면, GetModuleHandle이 현재 수행중인 실행 파일이 로드된 시작 주소를 반환한다.

해당 함수가 DLL내에서 호출되는 경우, 어떤 모듈에서 실행이되고 있는 알 수 있는 방법은 2가지가 있다.

- \_\_ImageBase가 현재 수행중인 모듈의 시작주소를 가리키고 있다는 점을 활용
- GetModuleHandleEx 함수를 호출하는 방법

```c
extern "C" const IMAGE_DOS_HEADER __ImageBase;

void DumpModule() {
    // 수행 중인 애플리케이션의 시작주소를 가져온다.
    // 수행 중인 코드가 DLL 내에 있는 경우, 다른 값이 얻어질 수 있다.
    HMODULE hModule = GetModuleHandle(NULL);
    _tprintf(TEXT("with GetModuleHandle(NULL) = 0x%x\r\n", hModule));

    // 방법1. 현재 모듈의 시작 주소(hModule/hInstance)를 얻기 위해
    // 가상변수인 __ImageBase를 사용한다.
    _tprintf(TEXT("with __ImageBase = 0x%x\r\n", (HINSTANCE) &__ImageBase));

    // 방법2. 현재 수행 중인 DumpModule의 주소를 GetModuleHandleEx에 매개변수로
    // 전달하여 현재 수행 중인 모듈의 시작 주소(hModule/hInstance)를 얻어온다.
    hModule = NULL;
    GetModuleHandleEx(
        GET_MODULE_HANDLE_EX_FLAG_FROM_ADDRESS,
        (PCTSTR) DumpModule,
        &hModule);
    _tprintf(TEXT("with GetModuleHandleEx = 0x%x\r\n"), hModule);
}

int main(int argc, TCHAR* argv[]) {
    DumpModule();
    return(0);
}
```

`GetModuleHandle` 함수 특징

- GetModuleHandle 함수는 자신을 호출한 프로세스의 주소 공간만을 확인한다
- NULL값을 인자로 넘겨주면, 프로세스 주소 공간에 로드된 실행 파일의 시작 주소를 반환한다 (주의. DLL 내부에 NULL을 인자로 호출하는 경우, 그 DLL의 시작주소가 아닌 실행 파일의 시작 주소를 반환한다)

### 프로세스의 이전 인스턴스 핸들

C/C++ 런타임 시작 코드는 항상 `(w)WinMain`의 `hPrevInstance` 매개변수로 NULL을 전달한다. 이 매개변수는 16비트 윈도우에서 사용되었고, 16비트 윈도우 애플리케이션 포팅의 편의를 위해 유일하게 `(w)WinMain`에 남아있다.

```c
int WINAPI _tWinMain(
    HINSTANCE hInstanceExe,
    HINSTANCE, // 매개변수에 이름이 없다.
    PSTR pszCmdLine,
    int nCmdShow);
```

2번째 인자의 경우 매개변수에 이름이 없기 때문에, 컴파일러는 "매개변수가 참조되지 않았다. parameter not referenced" 라는 경고를 발생시키지 않는다.

위저드로 생성된 C++ GUI 프로젝트인 경우, `UNREFERENCED_PARAMETER` 매크로를 이용하여 컴파일 경고가 발생하지 않도록 하고 있다.

```c
int APIENTRY -tWinMain(
    HINSTANCE hInstance,
    HINSTANCE hPrevInstance,
    LPTSTR lpCmdLine,
    int nCmdShow){
        UNREFERENCED_PARAMETER(hPrevInstance);
        UNREFERENCED_PARAMETER(lpCmdLine);
        ...
}
```

### 프로세스의 명령행

새로운 프로세스가 생성되면 프로세스에 명령행이 전달된다. 명령행(Command Line)은 비어 있는 경우가 거의 없다. 새로운 프로세스를 생성할 때, 최소한 명령행 첫번째 토큰으로 실행 파일의 이름을 전달한다.

`CreateProcess`와 같은 경우는 단 한 글자(`\0`만 가지는 빈 문자열)로 이루어진 명령행을 전달 받을 때도 있다.

시작 코드가 GUI 애플리케이션을 수행하는 경우, 먼저 `GetCommandLine` 윈도우 함수를 이용하여, 프로세스의 명령행 전체를 가져온다. 이 중 실행 파일명을 제외한 나머지 부분은 `WinMain` 함수의 `pszCmdLine` 매개변수를 통해 전달한다.

직접 `pszCmdLine` 매개변수가 가리키는 메모리 버퍼에 값을 쓸 수도 있지만, 잘못하면 버퍼의 끝을 초과하여 쓸 수도 있기 때문에 하지 않는것이 좋다. 버퍼는 읽기 전용으로 다루는 것이 좋다. 만약 명령행의 내용을 바꾸고 싶은 경우, 기존 버퍼를 새로운 버퍼에 복사하 후 복사된 버퍼를 변경한다.

`GetCommandLine` 함수를 호출하여 프로세스의 명령행 전체를 가리키는 포인터를 획득할 수 있다.

```c
PTSTR GetCommandLine();
```

`GetCommandLine` 함수를 호출하면 항상 동일한 버퍼 주소를 반환한다. `pszCmdLine`을 읽기 전용으로 다루어야 하는 이유가 있기 때문이다. `pszCmdLine`이 항상 동일한 버퍼를 가리키기 때문에 내용을 변경하면 최초의 명령행 정보를 알아낼 방법이 없다.

#### 명령행 구분, 토큰

많은 애플리케이션이 명령행으로 전달된 내용을 토큰으로 구분한다. 구분된 토큰에 접근하기 위해 전역 **argc와 **argv(혹은 \_\_wargv) 변수들을 사용한다. (사실 이러한 변수들을 사용하는 것을 권장하지 않는다)

`ShellAPI.h` 파일에 의해 선언되고, `Shell32.dll`에 의해 익스포트된 `CommandLineToArgvW`라는 함수가 있다. 이 함수는 유니코드 문자열을 여러개의 토큰으로 분리한다.

```c
PWSTR* CommandLineToArgvW(
    PWSTR pszCmdLine, // 명령행 문자열 포인터
    int* pNumArgs); // 인자의 개수
```

함수 뒤에 W(wide)가 붙는 것은 이 함수가 유니코드 전용이라는 것을 말한다. `pszCmdLine`으로는 명령행 문자열을 가리키는 포인터를 전달한다. 일반적으로 `GetCommandLineW` 의 반환 값을 사용하면 된다. `pNumArgs`는 정수를 가리키는 포인터이며, 명령행에 포함된 인자의 개수를 반환한다. `CommandLineToArgvW`는 반환 값으로 유니코드 문자열을 가리키는 포인터의 배열을 반환한다.

`CommandLineToArgW`는 내부적으로 메모리를 할당한다. 대부분의 애플리케이션은 이렇게 할당한 메모리를 삭제하지 않는다. 프로세스가 종료되는 시점에 메모리 해제된다. 크게 문제는 없지만, 만약 해제를 하고 싶다면 `HeapFree` 함수를 호출한다. 아래 예시 참고.

```c
int nNumArgs;
PWSTR *ppArgv = CommandLineToArgvW(GetCommandLineW(), &nNumArgs);

// 인자들을 사용한다.
if(*ppArgv[1] == L'x') {
    ...
}

// 메모리 블록을 삭제한다.
HeapFree(GetProcessHeap(), 0, ppArgv);
```

### 프로세스의 환경변수

모드 프로세스는 자기 자신과 연관된 환경블록(environment block)을 가지고 있다. 환경블록은 프로세스의 주소 공간에 할당된 메모리 블록을 의미한다. 이 공간은 아래와 같은 형식의 문자열을 포함한다.

```
=:=::\ ...
VarName1=VarValue1\0
VarName2=VarValue2\0
VarName3=VarValue3\0 ...
VarNameX=VarValueX\0
\0
```

각 문자열의 첫 번째 부분은 환경변수의 이름이다. `'='` 다음으로 할당하고자 하는 변수의 값이 나타난다. `'='`로 시작하는 문자열은 환경변수로 사용되는 문자열이 아니다.

환경블록에 접근하는 방법에는 2가지 방법이 있다. 각각의 방법은 서로 다른 파싱(parsing) 방식을 사용한다.

#### 환경블록 파싱하는 첫번째 방법

전체 환경 변수를 얻기 위해 `GetEnvironmentStrings` 함수를 호출한다. `=`로 시작하는 문자열은 건너뛴다. `=`는 환경변수의 이름과 값을 구분하는 구분자로 사용된다.

GetEnvironmentStrings 함수를 호출하고 더 이상 사용하지 않는 경우, `FreeEnvironmentStrings` 함수를 호출하여 메모리를 반환한다.

```c
BOOL FreeEnvironmentStrings(PTSTR pszEnvironmentBlock);
```

#### 환경블록 파싱하는 두번째 방법

CUI 애플리케이션에서만 활용 가능한 방법으로 `_tmain` 진입점 함수의 매개변수로 `TCHAR *env[]`를 사용하는 방법이다. `env`는 문자열을 가리키는 포인터의 배열로 구성되어 있으며, 각각의 포인터는 `이름=값` 형태의 문자열을 가리킨다. 마지막에는 `NULL` 포인터가 나타난다.

```c
// pEnvBlock에서 이미 =로 시작하는 문자열은 제거된 상태이기 때문에
// 따로 해당 문자열에 대해 처리할 필요가 없다.
void DumpEnvVariables(PTSTR pEnvBlock[]) {
    int current = 0;
    PTSTR* pElement = (PTSTR*)pEnvBlock;
    PTSTR pCurrent = NULL;
    while(pElement != NULL) {
        pCurrent = (PTSTR)(*pElement);
        if (pCurrent == NULL) {
            // 환경변수가 더 이상 없다.
            pElement = NULL;
        } else {
            _tprintf(TEXT("[%u] %s \r\n", current, pCurrent));
            current++;
            pElement++;
        }
    }
}
```

'='는 구분자 역할을 하기 때문에 이름에 '='가 들어가면 안된다. 또한 공백문자도 값으로 취급하기 때문에 공백문자도 중요하다.

```bash
# XYZ와 ABC는 다르다
XYZ= Windows
ABC=Windows

# XYZ_(공백이 들어간 경우)와 XYZ 는 다르다.
XYZ =Home
XYZ=Work
```

사용자가 윈도우에 로그온하면 시스템은 쉘 프로세스를 생성하고 이와 관련된 환경 문자열들을 설정한다. 시스템은 레지스트리의 두 군데 위치에서 초기 환경 문자열 값을 가지고 온다.

```bash
# 시스템 전반에 영량을 주는 환경변수 목록
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment
```

```bash
# 현재 로그온한 사용자에게만 영향을 주는 환경변수 목록
HKEY_CURRENT_USER\Environment
```

이런 환경변수는 제어판에서도 설정이 가능하다.

#### 환경블록 갱신

애플리케이션은 다양한 레지스트리 관련 함수를 이용하여 레지스트리 항목을 수정할 수 있다. 레지스트리의 변경사항이 모든 애플리케이션에 제대로 이루어지려면 사용자는 로그오프 후 다시 로그온을 해야한다.

레지스트리 항목을 수정한 이후에 다른 애플리케이션들이 이러한 변경사항을 기반으로 환경블록을 갱신하도록 하여면 `SendMessage`를 호출하면 된다.

```c
SendMessage(HWND_BROADCAST, WM_SETTINGCHANGE, 0, (LPARAM) TEXT("Environment"));
```

#### 자식 프로세스와 부모 프로세스의 환경변수

자식 프로세스는 부모 프로세스의 환경변수 집합을 그대로 상속한다. 하지만, 자식 프로세스에게 어떤 환경변수를 상속할지 제어할 수 있다.

상속을 통해 환경변수가 자식 프로세스에게 전달이 되면, 부모 프로세스와 같은 환경블록을 공유하는 것이 아니라 새롭게 복사 되어 전달된다. 그래서 자식 프로세스의 환경블록을 수정, 삭제, 추가 하더라도 부모 프로세스의 환경변수에 영향을 주지 않는다.

대부분의 애플리케이션은 사용자들이 애플리케이션의 환경변수를 바꾸기 위해서, GUI 기반의 다이얼로그 박스를 제공해주곤 한다. 이와 같은 방법은 사용자들에게 좀 더 친숙한 방법이다.

#### 환경변수 값 조회 및 존재 여부 확인하는 함수

환경변수를 이용하는 애플리케이션에서 사용할 수 잇는 유용한 함수들이 몇 가지 있는데, 그 중 하나가 `GetEnvironmentVariable`이다.

```c
DWORD GetEnvironmentVariable(
    PCTSTR pszName, // 환경변수 이름을 가리키는 포인터
    PTSTR pszValue, // 환경변수 값을 저장할 버퍼를 가리키는 포인터
    DWORD cchValue); // 버퍼의 크기를 문자 단위로 전달
```

이 함수는 버퍼로 복사된 문자의 개수를 반환하거나, 환경블록에 주어진 환경변수가 존재하지 않을 경우 0을 반환한다. 만약 환경변수의 값을 저장하기위해 얼마만큼의 버퍼가 필요할지 미리 알지 못하는 경우, `cchValue`에 `0`을 넣어준다. 그러면 문자 단위의 버퍼 크기에 문자열 종결 문자(`'\0'`)를 더한 공간만큼 값을 반환한다.

```c
void PrintEnvironmentVariable(PCTSTR pszVariableName) {
    PTSTR pszValue = NULL;
    // 환경변수 값을 저장하는데 필요한 버퍼 크기를 가져온다.
    DWORD dwResult = GetEnvironmentVariable(pszVariableName, pszValue, 0);
    if (dwResult != 0) {
        // 환경변수 값을 저장하기 위한 버퍼를 할당한다.
        DWORD size = dwResult * sizeof(TCHAR);
        pszValue = (PTSTR) malloc(size);
        GetEnvironmentVariable(pszVariableName, pszValue, size);
        _tprintf(TEXT("%s=%s\n"), pszVariableName, pszValue);
        free(pszValue);
    } else {
        _tprintf(TEXT("'%s'=<unknown value>\n"), pszVariableName);
    }
}
```

#### 대체 가능 문자열 설정하는 함수

환경변수 값에는 대체 가능 문자열(replaceable string)이 포함되어 있는 경우가 많다.

```bash
%USERPROFILE%\Documents
```

%로 감싼 부분은 대체 가능 문자열을 뜻한다. USERPROFILE 값이 아래와 같을 때,

```bash
USERPROFILE=C:\Users\jrichter
```

%USERPROFILE%\Documents는 아래와 같다.

```bash
C:\Users\jrichter\Documents
```

대체 가능 문자열에 대한 변경 작업은 ExpandEnvironmentStrings 함수를 이용한다.

```c
DWORD ExpandEnvironmentStrings(
    PTSTR pszSrc, // 대체 가능 문자열의 주소
    PTSTR pszDst, // 변경된 문자열이 저장될 버퍼의 주소
    DWORD chSize); // 버퍼의 최대 크기를 문자 단위로 전달
```

pszSrc는 대체 가능 문자열을 포함한 문자열의 주소를 전달한다. pszDst는 변경될 문자열이 저장될 버퍼의 주소를 전달한다. chSize는 버퍼의 최대 크기를 문자 단위로 전달한다.

만일 `chSize`가 변경될 문자열을 저장하기에 충분하지 않다면 대체 가능 문자열을 변경되지 않는다. 대신 빈 문자열(empty string)로 변경된다. 그래서 보통 `ExpandEnvironmentStrings` 함수를 2번 호출하는 것이 일반적이다.

```c
DWORD chValue = ExpandEnvironmentStrings(TEXT("PATH='%PATH%'"), NULL, 0);
PTSTR pszBuffer = new TCHAR[chValue];
chValue = ExpandEnvironmentStrings(TEXT("PATH='%PATH%'"), pszBuffer, chValue);
_tprintf(TEXT("%s\r\n"), pszBuffer);
delete[] pszBuffer;
```

#### 환경변수를 추가, 삭제, 변경을 위한 함수

환경변수를 추가하거나 삭제, 변경을 하기 위해 `SetEnvironmentVariable` 함수를 사용할 수 있다.

```c
BOOL SetEnvironmentVariable(
    PCTSTR pszName,
    PCTSTR pszValue);
```

`pszName`에는 변수의 이름을, `pszValue`에는 변수의 값을 전달한다. 만약에 동일한 변수의 이름이 있는경우 수정이 되고, 없는 변수의 이름의 경우 생성이 된다. 그리고 `pszValue`에 `NULL`을 넣으면 삭제가 된다.

### 프로세스의 선호도

프로세스의 스레드는 컴퓨터의 어떤 CPU에서도 수행될 수 있다. 가용 CPU 중에서 일부 CPU 에서만 수행되도록 할 수도 있다. 이것을 프로세서 선호도라고 한다.

### 프로세스의 에러 모드

각각의 프로세스는 디스크 에러, 처리되지 않은 예외, 파일 찾기 실패 등등 심각한 에러를 어떻게 처리할지 시스템에게 알려주는 플래그 값을 가지고 있다. 이때 `SetErrorMode` 함수를 호출하여 에러들을 시스템이 어떻게 처리해야하는지 알려줄 수 있다.

```c
UINT SetErrorMode(UINT fuErrorMode);
```

| 플래그                     | 설명                                                                                                                                                                                   |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SEM_FAIL_CRITICALERRORS    | 시스템이 심각한 에러 처리기 메시지 박스를 출력하지 않도록 하고, 발생한 에러를 호출한 프로세스에 전달하도록 한다.                                                                       |
| SEM_NOGPFAULTERRORBOX      | 시스템이 일반 보호 실패 메시지 박스를 출력하지 않도록 한다. 이 플래그는 디버깅 애플리케이션이 일반 보호 실패 자체에 대한 예외 처리기를 가지고 있는 경우에 한해서만 사용하는 것이 좋다. |
| SEM_NOOPENFILEERRORBOX     | 시스템이 파일 찾기 실패 박스를 출력하지 않도록 한다.                                                                                                                                   |
| SEM_NOALIGNMENTFAULTEXCEPT | 시스템이 자동으로 메모리 정렬 실패를 수정하고, 애플리케이션에게는 실패 사실을 알리지 않도록 한다. 이 플래그는 x86/x64 프로세서에서는 아무런 영향을 주지 못한다.                        |

자식 프로세스는 부모 프로세스의 에러 모드 플래그를 상속한다. 하지만 자식 프로세스는 상속을 받앗는지 전달 받지 못했을 수 있다. 이 경우 차일드 프로세스의 스레드에서 실패가 발생하면 아무런 통지도 없이 종료되어 버린다. 이런 상황을 막기 위해 부모 프로세스는 자식 프로세스가 에러 모드를 상속 받지 않도록 할 수 있다. `CreateProcess` 호출 시 `CREATE_DEFAULT_ERROR_MODE` 플래그를 지정하면 된다.

### 프로세스의 현재 드라이브와 디렉터리

CreateFile 함수를 사용하여 파일을 열고자 시도하면 시스템은 현재 드라이브와 디렉터리에서 파일을 찾는다. 시스템은 내부적으로 현재 드라이브와 디렉터리를 저장해두고, 이런 정보는 프로세스 단위로 유지된다. 그렇기 때문에 프로세스 내의 다른 모든 스레드에게 영향을 미친다.

프로세스의 현재 드라이브와 디렉터리를 얻어내거나 설정할때 아래 2개의 함수를 사용한다.

```c
DWORD GetCurrentDirectory(
    DWORD cchCurDir,
    PTSTR pszCurDir);
```

```c
BOOL SetCurrentDirectory(PCTSTR pszCurDir);
```

`GetCurrentDirectory`는 함수 호출시 전달하는 버퍼가 충분하지 않을 경우, 현재 티렉터리명을 저장할 수 있는 문자 단위의 크기에 '\0' 문자를 저장할 공간을 더한 값을 반환한다. 이 경우에는 주어진 버퍼에 어떠한 내용도 복사하지 않기 때문에 NULL을 전달한다. 함수 호출이 성공하면 복사된 문자열의 길이가 문자 단위로 반환되는데, 이때에는 '\0' 문자를 길이에 포함하지 않는다.

### 프로세스의 현재 디렉터리

운영체제는 다수의 드라이브에 대해 현재 디렉터리를 처리할 수 있는 기능을 제공한다.

```
=C:=C:\Utility\Bin
=D:=D:\Program Files
```

C드라이브의 현재 디렉터리가 `Utility\Bin`이고, D드라이브의 현재 디렉터리가 `\Program Files`이다.

환경변수에 지정된 드라이브 문자와 관련된 환경변수가 있는지 확인한다. 있는 경우 시스템은 이 값을 지정된 드라이브의 현재 디렉터리로 간주한다. 반면 없는 경우 지정된 드라이브의 루트 디렉터리를 현재 디렉터리로 간주한다.

예를들어, `=D:=D:\Program Files` 환경변수가 등록되어 있다면, `D:\Program Files`를 현재 디렉터리로 간주한다. 환경변수가 없다면 `D:\`(루트 디렉터리)를 현재 디렉터리로 간주한다.

자식 프로세스의 환경 블록에 부모 프로세스의 련재 디렉토리 설정 정보가 자동으로 상속되지는 않는다. 자식 프로세스는 모든 드라이브의 루트 디렉터리를 현재 디렉터리로 간주한다. 자식 프로세스에게 전달하고 싶다면, 차일드 프로세스가 실행되기 전에 환경블록에 추가해야 한다.

`GetFullPathName` 함수를 이용하면 현재 디렉터리 정보를 얻을 수 있다.

```c
DWORD GetFullPathName(
    PCTSTR pszFile,
    DWORD cchPath,
    PTSTR pszPath,
    PTSTR *ppszFilePart);
```

예를들어, C 드라비으의 현재 디렉터리 정보를 얻어오려면 아래와 같이 사용한다.

```c
TCHAR szCurDir[MAX_PATH];
DWORD cchLength = GetFullPathName(TEXT("C:"), MAX_PATH, szCurDir, NULL);
```

## Reference

- 제프리 리처의 Windows via C/C++ Edition 5th
