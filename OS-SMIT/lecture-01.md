# What is OS?

1. 프로세스 관리
2. 메모리 관리
3. 파일시스템 관리
	- 유닉스는 모든 것을 파일로 본다. DVD 라이터도 마찬가지. 파일 디스크립터 인스턴스를 이용한다.
	- 파일시스템으로 디바이스 관리

```
OS is a program that manages computer HW, providing
1. an environment for app programs
2. an interface between computer users and computer HW
```

```
컴퓨터 시스템의 4가지 요소
1. HW
2. OS
3. App
4. User
```

## Role of OS

1. 유저 입장에서는 UI
2. 시스템 입장에서는 resource allocator
	- 프린터의 경우에는 어떤 프로세스가 점유하면 끝날 때까지 다른 프로세스가 못 가져감
	- 리소스: CPU time, Mem space, File-Storage space, IO devices, etc.

## Kernel

The one program running at all times on the com.

* 마이크로 커널: 커널을 가볍게(필요한 디바이스만 로딩) -> 부팅이 빨라짐
* 리눅스 계열의 리얼타임 OS: 디바이스 관련 커널은 다 떼내고 아주 가볍게 돌림
* 우분투 LTS 버전에는 디바이스 관련 커널이 훨씬 많음

---

# OS Structure

```
DOS(Disk OS)
디스크의 프로그램 수행, 멀티프로그래밍이 안됨
```

## Multiprogramming

One of the most important aspects of OS is the ability to treat multiprogramming

## Time Sharing

* 타이머 인터럽트 발생하면 인터럽트 루틴 실행(타이머 인터럽트 발생시마다 프로그램 카운터 변경)
	* 현재 레지스터를 메모리에 저장함(스택)
	* 타이머 인터럽트의 프로그램 카운터 저장하고 실행
	* 지금 큐에 있는 프로세스 중에서 해야만 하는 프로세스 진행
	* 윈도우의 경우 10ms마다 타이머 인터럽트 + 1ms 인터럽트 루틴 실행
	* 소프트웨어 인터럽트(signal)는 하드웨어 인터럽트보다 느림 -> 리얼타임 OS에서는 소프트웨어 인터럽트 루틴 처리 속도가 중요

## Interrupt Driven

* 인터럽트를 쓸 수 있다 -> 프로세스에 priorty를 줄 수 있다
* 동영상을 재생할 떄
	* 사운드 디코딩은 금방하고 비디오 디코딩은 오래 걸림
	* 비디오 디코딩에 priorty 더 줌
	* Quantum Time: 우선권이 아무리 낮아도 얻을 수 있는 CPU time
* sleep 명령: 10ms 주면 CPU가 프로세스 내려감
	* 딱 10ms 이후에 다시 시작하는 것이 아님 - 10ms 이후에 큐에 다시 올라감
	* sleep 0ms(콘텍스트 스위칭): 일단 프로세스 내리지만 빨리 다시 불러줘(큐로 바로 올라감)
* 인터럽트 자체도 priorty가 있다: OS 인터럽트가 훨씬 높음
* 소켓 통신에서도 소켓 포인터가 생김
	* receive from: 도착을 기다림(인터럽트 등록) -> OS가 알려줌
	* send to: 모두 인터럽트 발생시키는 것, 그런데 그냥 막 보내면 OS가 인터럽트 처리 못 함(인터럽트 오버라이드) - 콘텍스트 스위칭을 쓰면 오버라이드가 안되고 큐에 남아있게 됨
	* max mtu 1500바이트

## Dual Mode Support

```
system call은 OS가 제공하는 빌트인 함수
system call을 주로 쓰는 프로그램: Middleware(앱이 OS를 잘 쓸 수 있도록 도와줌)
* SK에서 모바일 미들웨어 만들려다 다 실패
```

* 명령어 중에서 OS용과 일반 프로세스용을 구분함(Mode bit)
	* Intel: 00(OS), 01(VM level1), 10(VM level2), 11(User)
* 일반 프로세스가 OS용 명령어를 쓰고 싶다면 system call
	* 시계 프로그램을 만들자 - 컴퓨터 시계는 OS만 접근 가능하기 때문에 system call(BIOS에 접근하는 명령어)
	* 그냥 일반 프로스세가 BIOS에 접근하려고 하면 - OS 사망할수도
* 컴파일러 안에 시스템 콜을 부르게 하는 명령어가 있고 아닌 것들이 있다.
	* OS가 바뀌면 컴파일러도 바뀌어야 한다. (Hardware Dependent)
* wrapper: 함수를 콜할 때 API와 API를 연결해주는 것
	* 파이썬 모듈의 wrapper는 10개가 넘을 수도 - 그런데 왜 안 느릴까? (교수님도 궁금)

---

# Compenent of OS

1. 프로세스 관리
2. 메모리 관리
3. 스토리지 관리: 파일시스템 관리
4. protection and security
	* protection: 프로세스간 접근 권한
	* security: 유저간 접근 권한

```
프로세스간 통신방법
1. pipe: 프로세스간 통신을 OS가 전달해주는 것
2. shared memory: 프로세스가 할당받은 메모리 중 공유공간을 OS에 알려주는 것
```

## 프로세스 관리

* 어떤 프로세스가 리소스를 선점한 상태에서는 다른 프로세스가 접근 불가(wait queue에서 대기)
	* spolling: 하드디스크의 스풀 영역에 기록해두고 디바이스가 차례로 읽어감(프린터)
* 프로세스 만들기/죽이기
	* `kill`: 프로세스에 어떤 상태를 전달해주는 것(system call)
	* `fork`: 프로세스 만들기
	* 프로세스의 '죽음'이란?
		1. 큐에서 사라짐
		2. 메모리에서 사라짐
	* active queue, wait queue : 리소스 사용 중/대기 중, active queue에서 내려가면 wait queue 중에서 하나가 액티브큐로

## 메모리 관리

* malloc, calloc: 내 프로세스에 메모리 할당(system call)
* free: 메모리 반환(system call)
* 프로세스 죽이기 전에 free하지 않으면 memory leak
	* OS는 malloc, calloc 메모리를 모름(정확하게는 잡혀있는 것은 아닌데 쓰는 메모리인지 안쓰는 메모리인지는 모름) - stack은 알지만 heap 영역의 메모리는 동적이기 때문에 모름
	* 어떤 함수에서 포인터 잡아놓고 free하지 않으면? - 죽음


## 파일시스템 관리

* `ls`: 당연히 system call
* 이번 강의에서는 하지 않음

---

# SYSTEM CALL

프로그래머 입장에서 보이는 OS

```
`open()`: 디바이스를 사용하는 것도 open
프로세스간 통신도 system call
```

## 프로세스 컨트롤

1. 프로세스 만들기
2. 프로세스 죽이기
4. 프로세스 실행(큐로 보내기)
5. 프로세스 속성 지정(priorty 등)
6. 프로세스 대기
7. 프로세스의 메모리 조정
8. 이벤트 관리

## 파일시스템, 디바이스시스템

1. 열고
2. 닫고
3. 쓰고
4. 속성 지정

## 인포메이션 관리

1. 앱과 OS 사이의 정보교환

## Virus

1. system call을 어떻게 쓰다보니 이상한 일이 가능해짐
	* 어떻게 하다보니 프로세스 priorty에 OS와 같은 레벨의 priorty 설정이 가능해진다면?
	* 컴퓨터 끌 때 OS는 부트로더에 지난 컴퓨터 상태를 저장하는데 어떻게 하다보니 부트로더에 프로세스가 들어가진다면?

---

# OS Design

1. simple structure(이젠 없음)
	* 옛날에는 디바이스가 제어 없이 메모리에 막 들어감(MS-DOS)
2. layered approach: 각 레이어에서는 자기 기능과 자기보다 바로 위 레이어에서 제공하는 함수(API)만 사용 가능함
	1. HW
	2. CPU 스케줄링, 세마포어
	3. 메모리 관리
	4. OS
	5. 입출력
	6. APP
	7. Interface

```
현재는 너무 복잡해서 레이어도 단순화됨
```

3. microkernel: 아래 기능만 하고 나머지 기능(App, 파일시스템, IO 등)은 microkernel 위에 올려서 사용하기(윈도우/리눅스는 마이크로커널 정도로 가볍지는 않음, 안드로이드는 마이크로커널)
	* CPU 스케줄링
	* 메모리 관리
	* 프로세스간 통신

```
안드로이드는 리눅스의 마이크로커널만 쓰고 거의 새로 만든 OS
```

---

# BOOT

1. 부트로더를 올리는 프로그램이 BIOS
2. 부트로더는 OS를 메모리에 올린다.
3. 올리고 나서는 메인 프로세스를 실행시킨다.
