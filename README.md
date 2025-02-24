Download Link :https://programming.engineering/product/cse4116-embedded-system-software-assignment-1/



# CSE4116-Embedded-System-Software-Assignment-1
CSE4116 Embedded System Software – Assignment 1
목표

디바이스 컨트롤과 IPC를 이용하여 Simple Key-Value Store를 만든다.

Key-Value Store 아키텍처

Key-Value Store(KVS)는 NoSQL 데이터베이스에서 주로 사용되며 Key를 고유한 식별자로 Key-Value 쌍으로 데이터를 저장한다.

그림1. KVS Architecture

위 그림은 아주 간단한 KVS의 아키텍처를 나타낸다.

이번 과제에서 구현할 KVS의 구성요소와 Operation은 다음과 같다.

2.1. 구성요소

Memory Table : Put 으로부터 들어온 Key-Value 쌍이 Append-only manner 로 Memory 에 저장된다.

Storage Table : Memory Table 크기가 지정된 한계에 도달하면 파일을 생성해 Storage 에 저장된다.

2.2. Operation

Put : (Key,Value) 쌍으로 데이터를 입력받는다.

Get : Key 를 입력받아 현재 KVS 에 해당 Key 를 가진 Value 가 있으면 반환한다.

Flush : Memory Table 크기가 지정된 한계에 도달한 경우 파일을 생성하여 Storage 에 저장한다.

Merge : Storage Table 개수가 지정된 개수에 도달한 경우 전체 Storage Table 을 확인한다. 그리고 Merge 할 Storage Table 을 선택하여 그 중 중복된 Key 를 갖는 Value 를 제거하고

Key 를 기준으로 정렬하여 새로운 Storage Table 을 만들어 저장한다.

Embedded System Software 2 / 9

Embedded System Software 3 / 9

Embedded System Software

4 / 9

4.3. Put 모드

Key-Value 쌍을 설정하여 Put 요청을 전송한다. Key-Value쌍은 Memory Table에 저장된다. Memory Table 이 꽉 찬 경우 Main Process를 호출하여 Memory Table 공간을 확보하기 전까지(Flush동작 전까지)

Put은 정지된다.

FND : 설정할 Key 값을 출력한다.

입력되는순서에따라왼쪽에서부터한숫자씩출력한다.

Ex) 1234 가 출력 되는 과정

1000 1200 1230 1234

숫자 0 입력 버튼이 없으므로 0 이 중간에 있는 테스트는 수행하지 않음. Ex) 2024, 7080 Key 값은 고정적으로 4 자리 숫자이다.

Text LCD : 첫번째 줄에는 현재 모드를, 두번째 줄에는 설정할 Value 값을 출력한다.

Put 완료 시 KVS 에서 저장순서, Key, Value 를 순서대로 출력한다. (양쪽 괄호 포함)

첫번째 저장하는 Key-Value 쌍이 (1000, A) 인 경우 다음과 같다.



PUT Mode

(1, 1000,A)

이후 두번째로 (2000, B) 를 저장(Put)하면 다음과 같다.



PUT Mode

(2, 2000,B)

LED : Put Mode 의 초기 상태는 ①번 LED 에만 불이 들어온 상태이다.

Key 입력을 시작하면 ③번 ④번 LED 가 1 초에 하나씩 번갈아 가면서 불이 들어오게 한다.

Value 입력을 시작하면 ⑦번 ⑧번 LED 가 1 초에 하나씩 번갈아 가면서 불이 들어오게 한다.

Put 이 완료되면 전체 LED 에 불이 들어왔다가 이후 다시 ①번 LED 에만 불이 들어오게 한다.

Reset : Key 와 Value 입력을 전환한다. 최초에는 Key 입력으로 시작한다.



Embedded System Software 5 / 9



Embedded System Software

6 / 9



4.4. Get 모드

Key를 설정하여 Get 요청을 전송한다.

FND : 입력할 Key 값을 출력한다.

LCD : 첫번째 줄에는 현재 모드를, 두번째 줄에는 Get 결과 값을 출력한다.

Get 성공 시 KVS 에서 저장순서, Key, Value 값을 다음과 같이 출력한다. (양쪽 괄호 포함)



GET Mode

(1, 1000,A)

요청한 Key 값이 없을 경우 Error 를 출력한다.



GET Mode

Error

LED : 초기상태는 ⑤번 LED 에만불이들어온상태이다.

Key 입력을 시작하면 ③번 ④번 LED 가 1 초에 하나씩 번갈아 가면서 불이 들어오게 한다.

Get 이 완료되면 전체 LED 에 불이 들어왔다가 이후 다시 ⑤번 LED 에만 불이 들어오게 한다.

Swtich : 숫자를 입력(Put 과 동일) 받는 버튼이다. 숫자를 입력해서 Key 값을 설정하고 FND 에 출력한다.

Reset : Get 요청을 보낸다.

4.5. Merge 모드 & Background Job

Merge는 이 모드에서 직접 수행하거나 I/O Process로부터 호출되는 두 가지 경우가 가능하다.

Merge 수행은 가장 오래된 2개의 Storage Table에서 1)중복된 Key를 갖는 Key-Value 쌍들을 가장 최근에 입력된 Key-Value 쌍 하나로 merge하고 2)Key를 기준으로 오름차순으로 정렬한 다음 3)새로운 Storage Table을 만들어 저장한다.

아래는 Merge 모드에서 직접 수행할 때의 기능이다.

단, Motor는 I/O Process로부터 호출되어 Merge가 수행되는 경우에도 회전한다.

LCD : 첫번째 줄에는 현재 모드(MERGE Mode)를, 두번째 줄에는 Merge 수행 결과 값을 출력한다.

Merge 성공 시 생성된 Storage Table 의 이름과 Key-Value 쌍의 개수를 출력한다. (두번째줄출력형식자유)

Reset : Merge 요청을 보낸다.

Motor : Merge 수행 시 1-2 초(적당히)간 회전한다.



Embedded System Software

7 / 9



Key-Value Store 상세 구현 설명

5.1. Memory Table

최대 Key-Value 쌍 저장 개수는 3 개이다.

Key-Value 쌍이 3 개 이후 Put 이 발생하면 먼저 입력된 3 개의 Key-Value 를 Storage Table 로 저장한다.

반드시 메모리에 저장해야 한다 (파일 사용 x)

프로그램 종료 시 Memory Table 에 남아있는 데이터는 Key-Value 쌍 개수에 상관 없이 반드시 Storage Table 로 저장해야 한다.

5.2. Storage Table

확장자는 .st 를 사용한다.

1 개의 Key-Value 쌍은 파일에서 1 줄을 차지한다.

Key-Value 쌍은 KVS 에서 순서, Key, Value 가 공백으로 구분되어 순서대로 저장된다.

첫번째로 (1000,A), 두번째로 (2000,B), 세번째로 (3000,C) 가 저장된 Storage Table 의 경우 다음과 같다.

1 1000 A

2 2000 B

3 3000 C



5.3. Get/Put

Get 은 반드시 가장 최근에 입력된 순서대로 탐색해야 한다 (Memory Table Storage Table).

5.4. Merge

Merge 가 시작되는 Storage Table 개수는 3 개이다.

Merge 수행시에도 중복된 Key 가 없어 Key-Value 개수를 줄일 수 없는 경우 정렬만 하여 3 개 이상의

Key-Value 쌍을 갖는 Storage Table 을 생성한다.

Merge 예시 1 (수행전)



2.st

1.st

4 3000 F

1 1000 C

5 4000 E

2 1000 B

6 3000 D

3 1000 A

(과정)

2.st

1.st

4 3000 F (중복제거)

1 1000 C (중복제거)

5 4000 E

2 1000 B (중복제거)

6 3000 D

3 1000 A

(수행후)



3.st (파일명은 가장 마지막 순서)



1 1000 A

2 3000 D

3 4000 E



Embedded System Software

8 / 9



Merge 예시 2 (수행전)



2.st

1.st

4 5000 E

1 4000 D

5 1000 A

2 2000 B

6 6000 F

3 3000 C



(수행후)



3.st



1 1000 A

2 2000 B

3 3000 C

4 4000 D

5 5000 E

6 6000 F

5.5. 기타

프로그램 재실행 시 KVS 의 Storage Table 의 Database 는 유지되어야 한다. 구현(Metadata 관리 등)은 자유롭게하되명세서에상세히작성해야한다.

명세서에나와있지않은예외처리는자유롭게구현하되명세서에상세히작성해야한다.



Embedded System Software 9 / 9



제출 방법

아래제출파일을사이버캠퍼스에업로드

[프로그램제출란]

소스코드, Makefile, Readme 파일을 포함한 [HW1]학번.tar.gz Ex) [HW1]20240000.tar.gz

파일압축방법 : 학번폴더를생성하고, 그안에숙제관련파일을저장한뒤, 학번폴더가있는위치 에서 tar –cvzf [HW1]학번.tar.gz ./학번폴더

[보고서제출란]

[HW1]학번_reprt.pdf

Ex) [HW1]20240000_report.pdf

제공되는보고서형식참고. 세부목차/내용추가가능.

보고서는반드시 PDF로제출

평가내용

프로그램 100점(주석점수포함), 보고서 20점.

Late : 하루에 10% 감점

Due Date 기준 프로그램은 +5 일까지, 보고서는 +3 일까지.

제출형식틀린경우 10% 감점.

Copy 적발 시 0 점.

Copy 의심자는 조교와 면담을 통해 소명해야 함.

참고

Input process 작성 시 “readkey”를 다음과 같이 non-blocking 방식으로 사용 할 수 있습니다.

실습 자료에 있는 readkey.c 의 코드에서 파일 open 하는 부분을 아래와 같이 수정하여 사용하시길 바랍니다.

char* device = “/dev/input/event0”;

if((fd = open (device, O_RDONLY|O_NONBLOCK)) == -1) { printf (“%s is not a vaild device\\n”, device);
