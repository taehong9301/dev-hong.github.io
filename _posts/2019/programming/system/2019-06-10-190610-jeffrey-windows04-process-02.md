---
title: "프로세스 (Windows system programming) Section02"
excerpt: "제프리 리처의 Windows via C/C++, 4장 프로세스 Process, Section02"
search: true
categories:
  - Programming
  - System
tags:
  - Process
  - Windows System Programming
last_modified_at: 2019-06-10T18:10:00+09:00
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

<i class="fas fa-feather-alt"></i> **이전 글** <a href="/programming/system/190609-jeffrey-windows04-process-01/" target="_blank">Section01. 첫 번째 윈도우 애플리케이션 작성</a>
{: .notice--info}

## Section02 CreateProcess 함수

CreateProcess 함수를 이용하면 새로운 프로세스를 생성할 수 있다.

```c
BOOL CreateProcess (
    PCTSTR pszApplicationName, // 실행 파일명
    PTSTR pszCommandLine, // 명령행 인자
    PSECURITY_ATTRIBUTES psaProcess, // 보안 특성
    PSECURITY_ATTRIBUTES psaThread, // 보안 특성
    BOOL bInheritHandles, // 상속
    DWORD fdwCreate, // 새로운 프로세스를 어떻게 생성할지?
    PVOID pvEnvironment,
    PCTSTR pszCurDir,
    PSTARTUPINFO psiStartInfo,
    PROCESS_INFORMATION ppiProcInfo);
```

스레드가 `CreateProcess`를 호출하면 시스템은 사용 카운트가 1인 프로세스 커널 오브젝트를 생성한다. 이때 프로세스 커널 오브젝트는 프로세스 자체가 아니다. 운영체제가 프로세스를 관리하기 위한 목적으로 생성한 조그마한 데이터 구조체이다.

#### CreateProcess에 의해 프로세스가 생성되는 과정

1. CreateProcess에 의해 프로세스 커널 오브젝트(사용 카운트 1)가 생성된다.
2. 프로세스 커널 오브젝트가 생성되면 프로세스를 위한 가상 주소 공간을 생성한다.
3. 실행 파일의 코드와 데이터 및 수행에 필요한 추가적인 DLL 파일들을 프로세스의 주소 공간 상에 로드한다.
4. 생성된 프로세스의 주 스레드(primary thread)를 위한 스레드 커널 오브젝트(사용 카운트 1)를 생성한다.
5. 주 스레드는 링커에 의해 C/C++ 런타임 진입점 함수를 실행한다. (WinMain, wWinMain, main, wmain)
6. 시스템이 성공적으로 프로세스를 생성하고 주 스레드를 생성하면 CreateProcess는 TRUE를 반환한다.

`CreateProcess` 함수는 새로 생성된 프로세스가 완전히 초기화 되기전에 `TRUE`를 반환한다. 운영체제의 로더가 새로 생성된 프로세스가 필요로 하는 모든 DLL을 로드하기 전에 `CreateProcess`가 반환 될 수 있다는 의미다. DLL 파일이 없거나 올바르게 초기화가 되지 않으면 프로세스는 바로 종료되지만, 이미 `TRUE`를 반환한 상태이기 때문에 부모 프로세스는 자식 프로세스가 어떤 문제가 발생했는지 알 수 없다.

### pszApplicationName 과 pszCommandLine

`pszApplicationName`은 프로세스를 생성할 실행 파일명을 입력, `pszCommandLine`은 새로운 프로세스에게 전달한 명령행 문자열을 입력한다.

`pszCommandLine`의 자료형은 PTSTR인데, CreateProcess 내부에서 값을 변경할 수 있는 형태로 전달해야한다. 내부적으로 변경작업을 수행하기 때문이다. 하지만, 함수 반환 직전에 값을 원래의 값으로 돌려놓는다.

만약 아래 예제처럼 "NOTEPAD" 라는 문자열을 그대로 넘기면, C/C++ 컴파일러는 읽기 전용 메모리에 배치하기 때문에 접근 위반을 유발한다.

```c
STARUPINFO si = { sizeof(si) };
PROCESS_INFOMATION pi;
CreateProcess(NULL, TEXT("NOTEPAD"), NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi);
```

이러한 문제를 해결하기 읽기와 쓰기가 모두 가능한 메모리에 배치하도록 하는 것인데, 문자열 상수를 임시 버퍼에 복사한 후 CreateProcess를 호출한다.

```c
STARUPINFO si = { sizeof(si) };
PROCESS_INFOMATION pi;
TCHAR szCommandLine[] = TEXT("NOTEPAD");
CreateProcess(NULL, szCommandLine, NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi);
```

가장 좋은 방법은 `/GF` 컴파일러 스위치를 사용하되 임시버퍼를 이용하는 것이다. `/Gf` 와 `/GF`는 마이크로소프트 C/C++ 컴파일러에서 중복 문자열을 제거하고 문자열을 읽기 전용 섹션에 배치시킬 수 있도록 제공해주는 컴파일러 스위치다.

윈도우 비스타에서는 `ANSI` 버전의 `CreateProcess`를 호출하면 접근 위반이 발생하지 않는다. 그 이유는 유니코드로 변경하는 과정에서 문자열에 대한 복사본을 내부적으로 만들기 때문이다.

pszCommandLine을 이용하면 CreateProcess가 새로운 프로세스를 생성하기 위해 필요한 추가정보를 제공할 수 있다. 문자열의 첫 번째 토큰은 실행하고자 하는 프로그램의 파일명으로 간주된다. 확장자가 없는 경우 기본적으로 `.exe`로 가정한다.

시스템이 실행 파일을 찾으면 실행 파일의 코드와 데이터는 새로운 프로세스의 주소공간에 매핑되고, 링커에 의해 C/C++ 런타임 시작 함수를 호출한다. 실행 파일명 다음으로 전달되는 첫 번째 인자를 가리키는 포인터를 (w)WinMain의 pszCmdLine 매개변수를 통해 전달된다.

pszApplicationName 매개변수로 파일명을 지정하는 경우 파일명의 확장자를 `.exe`로 가정하지 않기 때문에, 반드시 확장자 명을 지정해야한다.

pszApplication에 NULL이 아닌 파일명을 지정하는 경우라도 pszCommandLine 매개변수를 통해 새로운 프로세스를 위한 명령행 문자열을 전달 할 수 있다.

```c
TCHAR szPath[] = TEXT("WORDPAD README.TXT");
CreateProcess(TEXT("C:\\WINDOWS\\SYSTEM32\\NOTEPAD.EXE"), szPath, ...);
```

위 함수가 실행이 되는 이유는 `CreateProcess`가 윈도우의 `POSIX` 서브 시스템을 지원하도록 하기 위해 포함한 것이다.

#### psaProcess, psaThread 그리고 bInheritHandles

시스템은 새로운 프로세스 커널 오브젝트와 스레드 커널 오브젝트를 생성해야한다. 보안 특성을 지정할 수도 있어야한다. `CreateProcess` 에는 `psaProcess`, `psaThread` 매개변수를 통해 원하는 보안 특성을 지정할 수 있다.

만약 기본 보안 디스크립터를 사용하고 싶을때는 NULL값을 넣어준다. 적절한 보안 권한을 설정하고 싶은 경우 SECURITY_ATTRIBUTES 구조체를 이용한다.

이렇게 SECURITY_ATTRIBUTES 구조체를 이용하는 이유는 커널 오브젝트 핸들을 상속받아 사용할 수 있도록 하기 위함이다.

#### fdwCreate

fdwCreate 매개변수는 새로운 프로세스를 어떻게 생성할지를 결정하게 된다. 플래그 값을 비트 OR 연산으로 결합하여 전달할 수 있다.

- DEBUG_PROCESS: 자식 프로세스와 자식 프로세스가 생성한 모든 자식 프로세스를 디버깅
- DEBUG_ONLY_THIS_PROCESS: 오직 자식 프로세스에 대해서만 디버깅
- CREATE_SUSPENDED: 새로운 프로세스를 생성한 후 주 스레드를 정지 상태로 만든다. 자식 프로세스가 코드를 수행할 수 있도록 하고 싶으면 ResumeThread 함수를 이용한다.
- DETACHED_PROCESS: CUI 기반의 자식 프로세스가 자신의 부모 프로세스의 콘솔 윈도우에 접근하는 것을 막는다.
- CREATE_NEW_CONSOLE: 새로운 콘솔 윈도우를 생성한다.
- CREATE_NO_WINDOW: 콘솔 윈도우를 생성하지 않는다.
- CREATE_NEW_PROCESS_GROUP: `Ctrl` + `C` 나 `Ctrl` + `Break` 를 눌렀을 때, 통보할 프로세스 목록을 수정한다. 운영체제는 동일 그룹 내의 모든 프로세스에게 사용자가 현재 수행중인 동작을 정지하고자 함을 알려준다.
- CREATE_DEFAULT_ERROR_MODE: 부모 프로세스가 사용하는 에러모드를 새로 생성하는 프로세스가 상속 받지 않도록 한다.
- CREATE_SEPERATE_WOW_VDM: 운영체제가 새로운 가상 도스 머신(VDM, Virtual Dos Machine)을 생성하고 그 안에서 16비트 윈도우 애플리케이션을 수행하도록 해준다. 모든 16비트 애플리케이션은 하나의 VDM을 공유한다. 만약 오동작을 하더라도 VDM만 종료되기 때문에 다른 애플리케이션에 영향을 주지 않는다.(정상동작한다) 하지만 단점으로 여러개의 VDM을 사용하면 VDM별로 상당량의 물리적 저장소를 사용한다.
- CREATE_SHARED_WOW_VDM: 단일 VDM 내에서 애플리케이션을 수행한다.
- CREATE_UNICODE_ENVIRONMENT: 자식 프로세스의 환경블록 내에 유니코드 문자들이 사용되도록 한다. (기본적으로 ANSI를 사용)
- CREATE_FORCEDOS: MS-DOS 애플리케이션이 16비트 OS/2 애플리케이션 내에서 수행되도록 한다.
- CREATE_BREAKAWAY_FROM_JOB: 프로세스가 해당 잡과 연결되지 않는 프로세스를 생성하고 싶을 때 사용한다.
- EXTENDED_STARTUPINFO_PRESENT: psiStartInfo 변수를 통해 STARTUPINFOEX를 전달 할 것을 운영체제에게 알려준다.

#### pvEnvironment

새로운 프로세스가 사용할 환경변수 문자열을 포함하고 있는 메모리 블록을 가리키는 포인터를 지정한다. 대부분의 경우 자식 프로세스가 부모 프로제스의 환경블록을 상속받아 사용할 수 있도록 NULL 값을 지정한다.

만약 NULL값을 넣기 싫은경우 `GetEnvironmentStrings` 함수를 활용하면 된다.

```c
PVOID GetEnvironmentStrings();
```

해당 함수는 현재 프로세스가 사용중인 환경블록의 주소를 반환한다. 사실 위 작업은 `pvEnvironment` 에 NULL 값을 지정하는 것과 같다. 더 이상 메모리 블록이 필요하지 않은 경우, `FreeEnvironmentStrings` 함수를 이용하여 메모리 블록을 삭제한다.

```c
BOOL FreeEnvironmentStrings(PTSTR pszEnvironmentBlock);
```

#### pszCurDir

부모 프로세스가 자식 프로세스의 현재 드라이브와 디렉터리를 설정할 수 있도록 한다. NULL 값을 넣으면 애플리케이션의 현재 디렉터리와 동일하게 설정된다.

#### psiStartInfo

매개변수에 2개의 구조체 포인터를 넣어줘야 함

- `STARTUPINFO`

```c
typedef struct _STARTUPINFO {
    DWORD cb; // 구조체의 바이트 크기를 담고 있다. (윈도우, 콘솔)
    PSTR lpReserved; // 예약된 멤버, NULL로 초기화 (윈도우, 콘솔)
    PSTR lpDesktop; // 애플리케이션이 실행되는 데스크톱의 이름 (윈도우, 콘솔)

    // 화면 상에서 애플리케이션 윈도우의 X, Y 좌표(픽셀 단위) 지정 (윈도우, 콘솔)
    DWORD dwX;
    DWORD dwY;

    // 애플리케이션의 넓이와 높이(픽셀 단위) 지정 (윈도우, 콘솔)
    DWORD dwXSize;
    DWORD dwYSize;

    DWORD dwFillAttribute; // 차일드 콘솔 윈도우의 글자색과 배경색 (콘솔)
    DWORD dwFlags; // 차일드 프로세스를 어떻게 생성할지 지정하는 플래그 (윈도우, 콘솔)
    WORD wShowWindow; // 윈도우가 어떨게 나타날지를 지정 (윈도우)
    WORD cbReserved2; // 예약된 멤버, 0으로 초기화 (윈도우, 콘솔)
    PBYTE lpReserved2; // 예약된 멤버, NULL로 초기화 (윈도우, 콘솔)

    // 콘솔 입출력에 사용할 버퍼를 가리키는 핸들을 지정 (콘솔)
    HANDLE hStdInput;
    HANDLE hStdOutput;
    HANDLE hStdError;
} STARTUPINFO, *LPSTARTUPINFO;
```

- `STARTUPINFOEX`

```c
typedef struct _STARTUPINFOEX {
    STARTUPINFO StartupInfo;
    struct _PROC_THREAD_ATTRIBUTE_LIST *lpAttributeList;
} STARTUPINFOEX, *LPSTARTUPINFOEX;
```

윈도우 새로운 프로세스를 생성할 때, 이 구조체의 멤버들을 이용한다. 기본값 프로세스를 생성할 때,최소한 구조체의 멤버를 0으로 초기화하고 cb 멤버를 구조체의 크기로 설정하는 작업을 수행해야 한다.

```c
STARTUPINFO si = { sizeof(si) };
CreateProcess(..., &si, ...);
```

초기값 0을 설정하지 않으면, 각 멤버는 **쓰레기값을 가지게 된다.** 그래서 간혹 프로세스가 생성되지 못하기도 한다. 즉, `CreateProcess` 가 정상적으로 동작하기 위해서 각 멤버 값을 0으로 초기화 하는 작업은 매우 중요하다. (이러한 초기화 절차의 누락은 개발자들이 흔히 저지르는 실수 중 하나)

- dwFlags에 지정할 수 있는 플래그

| 플래그                  | 의미                                                              |
| ----------------------- | ----------------------------------------------------------------- |
| STARTF_USESIZE          | dwSize와 dwYSize 멤버를 사용한다.                                 |
| STARTF_USEHOWWINDOW     | wShowWindow 멤버를 사용한다.                                      |
| STARTF_USEPOSITION      | dwX 와 dwY 멤버를 사용한다.                                       |
| STARTF_USECOUNTCHARS    | dwXCountChars 와 dwYCountChars 멤버를 사용한다.                   |
| STARTF_USEFILLATTRIBUTE | dwFillAttribute 멤버를 사용한다.                                  |
| STARTF_USETDHANDLES     | hStdout, hStdError 멤버를 사용한다.                               |
| STARTF_RUNFULLSCREEN    | x86 컴퓨터에서 콘솔 애플리케이션이 전체 화면으로 시작되도록 한다. |

윈도우는 선점형 멀티테스킹을 지원하기 때문에 새로 생성한 프로세스가 초기화 되는 동안 다른 프로그램을 수행할 수 있다. 새로 생성한 프로그램이 초기화 작업중인것을 비주얼로 보여주기 위해 `CreateProcess`는 임시로 운영체제의 화살표 커서를 기다리는 모양(애플리케이션 시작 커서)으로 변경한다.

- `STARTF_FORCEONFEEDBACK` 플래그: 시작 커서로 변경하지 않는다.
- `STARTF_FORCEOFFFEEDBACK` 플래그: 프로세스 초기화 과정에 맞게 적합한 커서로 변경한다. 시작 커서로 변경한 뒤 2초가 지나도 새로운 프로세스에서 GUI 함수를 호출하지 않으면 커서를 원래 모양으로 재설정한다.

새로 생성된 프로세스가 2초 이내에 GUI 함수를 호출하면 커서 모양을 변경하지 않고 기다린다. 5초 동안 GUI 화면이 안띄어지면 `CreateProcess` 함수는 화살표 모양으로 재설정한다. 반대로 5초 이내에 화면이 나오면 시작 커서 형태를 유지한다. 그리고 `GetMessage` 함수를 호출하면 초기화가 끝난것으로 판단하여 화살표 모양으로 재설정한다.

마이크로소프트는 `Win32` 가 사용 가능해진 이후로 한 번도 변경되지 않았다. 함수를 변경하지 않고 기능을 확장할 수 있도록 했다. 그래서 `lpAttributeList`라는 필드를 만들어서, `STARTUPINFOEX` 구조체를 작성했다.

```c
// ANSI
typedef struct _STARTUPINFOEXA {
    STARTUPINFOA StartupInfo;
    struct _PROC_THREAD_ATTRIBUTE_LIST *lpAttributeList;
} STARTUPINFOEXA, *LPSTARTUPINFOEXA;

// Unicode
typedef struct _STARTUPINFOEXW {
    STARTUPINFOW StartupInfo;
    struct _PROC_THREAD_ATTRIBUTE_LIST *lpAttributeList;
} STARTUPINFOEXW, *LPSTARTUPINFOEXW;
```

특성 리스트를 이용하여 여러 개의 특성값을 나타내기 위해 키/값 쌍을 지정할 수 있다. 비어있는 특성 리스트를 만들기 위해서는 아래 함수를 2번 호출해야 한다.

```c
BOOL InitializeProcThreadAttributeList (
    PROC_THREAD_ATTRIBUTE_LIST pAttributeList,
    DWORD dwAttributeCount,
    DWORD dwFlags,
    PSIZE_T pSize);
```

1. 윈도우가 여러개의 특성들의 저장하려면 얼마만큼의 메모리 블럭이 필요한지를 알기 위해 아래와 같이 호출한다.

```c
SIZE_T dbAttributeListSize = 0;
BOOL bReturn = InitializeProcThreadAttributeList (
    NULL, 1, 0, &cbAttributeListSize);
// bReturn은 FALSE이고, GetLastError()는 ERROR_INSUFFICIENT_BUFFER를 반환
```

2. 메모리 블럭이 얼마나 필요한지 알았으니 특성 리스트를 만든다.

```c
pAttributeList = (PROC_THREAD_ATTRIBUTE_LIST)
    HeapAlloc(GetProcessHeap(), 0, cbAttributeListSize);
```

3. `InitializeProcThreadAttribute` 함수를 재호출하여, 내용을 초기화한다.

```c
bReturn = InitializeProcThreadAttributeList(
    pAttributeList, 1, 0, &cbAttributeListSize);
```

4. `UpdateProcThreadAttribute` 함수를 이용하여 필요한 키/값 쌍을 특성에 추가한다.

```c
BOOL UpdateProcThreadAttribute (
    PROC_THREAD_ATTRIBUTE_LIST pAttributeList,
    DWORD dwFlags,
    DWORD_PTR Attribute,
    PVOID pValue,
    SIZE_T cbSize,
    PVOID pPreviousValue,
    PSIZE_T pReturnSize);
```

#### ppiProcInfo

`PROCESS_INFORMATION` 구조체를 가리키는 포인터로 지정되며, 함수 호출에 앞ㅇ서 반드시 메모리 할당을 해야한다. `CreatProcess` 함수는 반환직전 이 구조체의 멤버를 초기화 한다.

```c
typedef struct _PROCESS_INFORMATION {
    HANDLE hProcess;
    HANDLE hThread;
    DWORD dwProcessId;
    DWORD dwThread;
} PROCESS_INFORMATION
```

새로운 프로세스가 생성되는 과정은 아래와 같다.

1. 프로세스를 만들면 새로운 프로세스 커널 오브젝트와 스레드 커널 오브젝트를 생성 (사용 카운트는 1이 된다)
2. `CreateProcess` 는 내부에서 커널 오브젝트들(프로세스와 스레드)을 최대 권한으로 다시 한번 열기 때문에 사용 카운트는 2가 됨
3. 그리고, `PROCESS_INFORMATION` 구조체 내의 `hProcess`와 `hThread` 멤버에 호출한 프로세스에서 사용할 수 있도록 커널 오브젝트의 핸들 값을 할당

프로세스 커널 오브젝트가 파괴할때는

1. 프로세스 종료 (사용 카운트 1 감소)
2. 부모 프로세스에서 `CloseHandle`를 호출 (다시 사용 카운트 1 감소되어, 0이 됨)

마찬가지로, 스레드 커널 오브젝트를 파괴할때는

1. 스레드 종료
2. 부모 프로세스에서 스레드 커널 오브젝트 핸들을 삭제

자식 프로세스의 핸들과 주 스레드 핸들을 반드시 삭제해야만 애플리케이션이 수행되는 동안, 리소스 누수를 막을 수 있다. (물론 프로세스가 종료되면 시스템에서 리소스들을 삭제함)

프로세스 커널 오브젝트와 스레드 커널 오브젝트는 생성되면 고유의 ID 값을 부여 받게 된다. 프로세스 ID와 스레드 ID는 동일한 숫자 POOL을 공유하기 때문에, 동일한 ID 값을 가질 수 없다, 또한 0을 가질 수 없다.

작업 관리자에는 "시스템 유휴 프로세스" 라는 것이 존재하고, 0번 ID를 가진 프로세스를 보여주지만 가상의 프로세스가 있는 것처럼 보여줄 뿐 실제로는 존재하지 않는다.

고유 ID값은 보통 시스템 내의 프로세스와 스레드를 구분하기 위한 용도로 사용하고, 일반적인 애플리케이션에서는 사용하지 않는다. 만약 프로세스와 스레드의 ID값 추적하면서 사용하는 경우, **ID 값이 즉각적으로 재사용된다는 사실**을 알아야한다. 예를들어 이미 수행중인 프로세스의 ID 값이 124일때, 프로세스가 종료되면 124 ID 값은 다음번에 생성되는 프로세스가 할당 받을 수 있다. 즉, 앞서 할당된 프로세스가 종료되면, 종료된 프로세스의 ID를 새로 생성된 프로세스가 (동일한 ID 값) 할당 받을 수 있다는 의미다.

ID값을 얻을 수 있는 함수들

- `GetCurrentProcessId`: 현재 수행중인 프로세스 ID를 가져옴
- `GetCurrentThreadId`: 현재 수행중인 스레드 ID를 가져옴
- `GetProcessId`: 커널 오브젝트 핸들을 이용하여 프로세스 ID를 가져옴
- `GetThreadId`: 커널 오브젝트 핸들을 이용하여 스레드 ID를 가져옴
- `GetProcessIdOfThread`: 해당 스레드를 소유하고 있는 프로세스의 ID를 가져옴

만약 자신을 생성한 "생성자"와 통신이 필요한 경우, 프로세스 ID값을 사용하는 것보다는 커널 오브젝트 또는 윈도우 핸들을 사용하는 것이 좋다. (ID값은 즉각적으로 재사용될 수 있으므로)

프로세스 또는 스레드의 ID값이 재사용되는 것을 막고 싶다면, 커널 오브젝트 핸들을 삭제하지 않는 방법 밖에 없다.

## Reference

- 제프리 리처의 Windows via C/C++ Edition 5th
