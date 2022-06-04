# 리눅스 명령어와 vim 에디터에서 매크로 관련하여 조사

## 1. 리눅스 명령어 : top, ps, jobs, kill 명령어 조사하기

### 1-1. 리눅스 명령어 top
top 명령어 실행시 다음과 같은 사진이 출력됩니다.

![top](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Frxlg4%2FbtqYfV2LE3L%2FSW5SbyO65ZUa5PggM3KI8K%2Fimg.png)

**1) 요약 영역**\
사진 상단에 위치하는 요약 영역은 전체 프로세스가 OS에 대해서 리소스를 어느정도 차지하고 있는지를 알려줍니다. 요약 영역에 나타나는 대표적인 값은 시간, 유저, 로드 에버리지(Load Average), 테스크(Tasks), CPU, 메모리(memory)로 아래의 이미지를 보시면 각 영역에 대해 나태내느 값이 어디에 위치하는지 알 수 있습니다.

![top2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcIwHTf%2FbtqYiCOXiQk%2F0wpKbRc7uKG8mo9vfKLWiK%2Fimg.png)

>***System Time***
>>+ 이미지의 가장 왼쪽 위를 보시면 시스템 현재 시간, OS가 살아있는 시간, 그리고 유저의 세션수가 표시되는 영역이 있습니다. 가장 먼저 보이는 숫자가 시스템의 현재 시간입니다. 이 시간은 GMT 기준으로 표시됩니다. 위 예제 기준으로 GMT 16:58:55 이라는 것입니다. 이것은 한국시간으로 보면 +9를 한 00시 58분 55초와 동일합니다. 다음으로 표시되는 것이 OS가 얼마나 살아있는지 나타냅니다. days와 시간으로 표시되며 위 예제로보면 7일과 1시 15분 동안 서버가 살아있었다는 것을 알 수 있습니다. 그리고 다음 나타나는것이 현재 접속중인 유저 세션 수입니다.

>***Load Average***
>>+ 해당 영역은 CPU Load의 이동 평균를 표시합니다. 앞에서 부터 1분, 5분, 그리고 15분에 대한 평균값입니다. CPU Load란 CPU가 수행하는 작업의 양 입니다. 리눅스에서는 실행되거나 대기중인 프로세스의 평균입니다. 싱글 코어일 경우 1.0의 값이 CPU 100%를 사용하고 있다는 의미입니다. 멀티 코어라면 해당 코어수 만큼 * N을 한 값이 CPU 100%를 사용한다는 의미가 됩니다. 만약 100%를 넘어간다면 CPU에서 처리하지 못하고 대기하고 중인 프로세스가 있다고 보시면됩니다.

>***Tasks***
>>+ Tasks는 현재 프로세스들의 상태를 나태내주는 영역입니다. Total은 전체 프로세스, running은 running 상태인 프로세스, sleeping은 대기상태인 process, stopped는 종료된 프로세스, zombies는 좀비상태인 프로세스의 수를 나타냅니다.

>***CPU***
>>+ 이 영역은 CPU가 어떻게 사용되고 있는지 그 사용율을 보여주는 영역입니다. 모든 값의 총 합은 100% 이며 이를 퍼센테이지로 나누어서 보여줍니다.

>***Memory***
>>+ 첫번째 줄은 RAM의 메모리 영역으로 Mem이라 표시되어있는 부분입니다. 그리고 아랫줄은 디스크를 메모리 처럼 이용하는 Swap 메모리 영역입니다. 일반적으로 Mem의 사용량이 거의 가득 찼을때 Swap 메모리 영역을 사용합니다. 이 영역은 디스크이기 때문에 RAM 메모리보다 속도가 많이 느린 단점을 가집니다. buff/cache에서 buff는 buffers의 약자입니다. 이 값은 커널 버퍼에서 사용되는 메모리를 뜻합니다. cache는 Disk의 페이지 캐시를 말합니다. 즉, buff/cache는 IO와 관련되어 사용되는 버퍼에 사용되는 메모리를 말합니다. 이 메모리가 있으므로써 IO에 상대적으로 빠른 속도를 가질 수 있습니다. avail Mem은 swap 메모리를 사용하지 않고 사용할 수 있는 메모리의 크기를 말합니다.


**2) 디테일 영역**\
아래의 이미지 부분이 디테일 부분입니다. 각 요소에 대해서 하나씩 보도록하겠습니다.

![top3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblAjpn%2FbtqYoZ9VPTv%2FYE1lrKawKfL21NjYsn3RlK%2Fimg.png)

>***PID***
>>+ PID는 프로세스 ID이며 프로세스를 구분하기 위한 겹치지않는 고유한 값입니다.

>***USER***
>>+ 해당 프로세스를 실행한 USER 이름 또는 효과를 받는 USER의 이름입니다.

>***PR & NI***
>>+ PR : 커널에 의해서 스케줄링되는 우선순위입니다.
>>+ NI : PR에 영향을 주는 nice라는 값입니다.

>***VIRT, RES, SHR, %MEM***
>>+ 해당 필드들은 프로세스의 메모리와 관련있습니다.
>>+ VIRT : 프로세스가 소비하고 있는 총 메모리입니다. 프로그램이 실행중인 코드, heap, stack과 같은 메모리, IO buffer 메모리를 포함합니다.
>>+ RES : RAM에서 사용중인 메모리의 크기를 나타냅니다.
>>+ SHR : 다른 프로세스와의 공유메모리(Shared Memory)를 나타냅니다.
>>+ %MEM : RAM에서 RES가 차지하는 비율을 나타냅니다.

>+ S : 프로세스의 현재 상태를 나타냅니다.
>+ TIME+ : 프로세스가 사용한 토탈 CPU 시간
>+ COMMAND : 해당 프로세스를 실행한 커맨드를 나타냅니다.

top 명령어를 실행하려면 다음과 같이 입력하면 됩니다.\
`top -b -n 1`

top 실행 옵션으로는 
|옵션|정보|
|:--:|:--:|
|-b|순간의 정보를 확인(batch 모드)|
|-n|top 실행 주기 설정(반복 횟수)|

이 있습니다.

---
### 1-2. 리눅스 명령어 ps
현재 실행중인 프로세스의 목록을 보는 명령어입니다. 

+ 실행 옵션

|옵션|정보|
|:--:|:--:|
|-e|실행중인 모든 프로세스의 정보를 출력|
|-f|프로세스에 대한 자세한 정보룰 출력( PPID 확인 가능 )|
|-u [사용자이름]|특정 사용자에 대한 모든 프로세스의 정보를 출력|
|-p pid|pid로 지정한 프로세스의 정보를 출력|
|u|프로세스 소유자의 이름, CPU 사용량, 메모리 사용량 등 상세 정보를 출력|
|a|터미널에서 실행한 프로세스의 정보를 출력|
|x|실행 중인 모든 프로세스의 정보를 출력|



1. ps명령을 옵션 없이 사용하면 현재 셸이나 터미미널에서 실행한 사용자 프로세스에 대한 정보를 출력합니다. 

+ PID : 프로세스번호
+ TTY : 터미널 번호
+ TIME : 해당 프로세스가 사용한  CPU 시간의 양
+ CMD는 프로세스가 실행중인 명령이 무엇인지 알려준다.
`ps`
![ps](https://postfiles.pstatic.net/20160606_285/jsky10503_1465185698271RGSQS_PNG/K-001.png?type=w2)


2. ps u 명령을 이용하여 사용량 등을 포함한 상세정보를 확인
`ps u`
![ps2](https://postfiles.pstatic.net/20160606_259/jsky10503_1465186789087wFne0_PNG/K-001.png?type=w2)


3. ps x 명령으로 실행중인 모든 프로세스를 출력
`ps x`
![ps3](https://postfiles.pstatic.net/20160606_18/jsky10503_1465186992449tVBAH_PNG/K-001.png?type=w2)


4. ps -u [사용자명] 옵션을 이용하여 user1 계정의 ps를 확인 / 다른 옵션도 함께사용 가능
`ps u user`
![ps4](https://postfiles.pstatic.net/20160606_197/jsky10503_1465187702509nCbpG_PNG/K-003.png?type=w2)


5. ps 명령어에 -p 옵션을 이용하여 1번 프로세스 출력
`ps -p 1`
![ps5](https://postfiles.pstatic.net/20160606_209/jsky10503_1465185731824IoYKz_PNG/K-006.png?type=w2)


6. ps -f 명령으로 ps명령에 비해서 보다 상세한 정보를 확인
`ps -f`

>+ -f옵션들의 항목
>>+ PPID : 부모 프로세스의 PID
>>+ UID : 프로세스를 실행한 사용자 ID
>>+ PID : 프로세스 번호
>>+ PPID : 부모프로세스 번호
>>+ C : CPU 사용량( %값 )
>>+ STIME : 프로세스의 시작 시간
>>+ TTY : 프로세스가 실행된 터미널의 종류와 번호
>>+ TIME : 프로세스 실행 시간 ( 실행되기전에는 0)
>>+ CMD : 이런 명령어를 사용했다.

![ps6](https://postfiles.pstatic.net/20160606_122/jsky10503_1465185732004TVF3v_PNG/K-007.png?type=w2)


7. ps a 명령으로 터미널에서 실행한 프로세스 정보를 출력
`ps a`

>+ a옵션들의 항목
>>+ STAT : 프로세스의 상태를 나타냄. (ex - R , S , D 등등)
>>+ R : 실행중
>>+ S : 인터럽트 가능한 대기(sleep)상태
>>+ T : 작업 제어에 의해 정지된(stopped) 상태
>>+ Z : 좀비프로세스

![ps6](https://postfiles.pstatic.net/20160606_102/jsky10503_1465185732139EMRIC_PNG/K-008.png?type=w2)

---
### 1-3. 리눅스 명령어 jobs
test3

---
### 1-4. 리눅스 명령어 kill
test4

---

## 2. vim 에디터에서 매크로 사용방법에 대하여 조사하기 (q , @)
test2222
