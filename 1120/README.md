Stack Frame(스택 프레임) 정리

스택 프레임은 함수(서브루틴)가 실행될 때
함수의 인자, 복귀 주소, 지역 변수, 저장된 레지스터 등을 보관하기 위해
스택에 만들어지는 독립된 공간

스택 프레임 생성 과정
Push Arguments — 인자(push) -> Call Subroutine — 함수 호출 -> Push EBP — 이전 프레임 포인터 저장 -> Set EBP to ESP — 새 스택 프레임 기준 설정 -> Allocate Local Variables — 지역 변수 공간 확보 -> Save Registers — 레지스터 백업

Register Parameters의 단점 (Disadvantages of Register Parameters)
레지스터를 함수 인자로 사용하면 빠를 것 같지만, 실제로는 오히려 번거롭고 덜 효율적일 때가 많다.
✔ 레지스터를 보존(push)
✔ 함수 인자로 레지스터 설정
✔ 함수 호출(call)
✔ 레지스터 복원(pop)       → 이런 과정 때문에 코드가 길어지고 느려질 수 있음.

Value Arguments vs Reference Arguments (값 전달 vs 참조 전달)
subroutine(서브루틴, 함수) 호출 시
어떤 방식으로 인자를 전달할지 선택해야 하는 이유를 보여주는 그림이다.

Value Arguments (값 전달, Call by Value)                      Reference Arguments (참조 전달, Call by Reference)
➡ 함수는 전달받은 값만 사용하고,                               ➡ 함수 내부에서 array 내용이 직접 바뀔 수 있다.
원본 변수는 절대 바뀌지 않는다.

Passing by Value (값으로 전달)

“값 전달(Call by Value)”은
인자의 실제 값(value)이 복사되어 스택에 push 되는 방식을 말한다.
즉, 원본 변수는 절대 변경되지 않는다.

Passing by Reference (참조 전달)                                      📌 Passing Arrays (배열 전달)                                    📌 Accessing Stack Parameters (스택에 있는 인자 접근)
참조 전달은 함수에 값을 직접 전달하는 것이 아니라,                           배열은 크기가 크기 때문에, 값 전체를 복사하지 않고                  함수가 호출되면 스택 프레임이 만들어지고,
값이 저장된 메모리 주소(OFFSET) 를 전달하는 방식이다.                        무조건 주소를 전달(pass by reference) 한다.                       함수의 인자는 EBP 기준으로 접근한다.

✔ 핵심 특징                                                           ✔ 핵심 특징                                                      ✔ EBP 기준 오프셋 규칙
스택에는 값이 아니라 주소가 push된다.                                   스택에는 배열의 시작 주소만 push된다.                               [EBP + 4] : return address
함수는 이 주소를 따라가서 원래 변수의 실제 메모리에 접근한다.              함수는 이 주소를 기반으로 배열 내용을 읽거나 수정한다.               [EBP + 8] : 첫 번째 인자
따라서 함수 내부에서 값을 변경하면 원본 값이 바뀐다.                      고급 언어(C, C++, Java 등)에서도 배열은 항상 참조로 전달된다.        [EBP + 12] : 두 번째 인자
큰 데이터(배열, 구조체)를 효율적으로 다룰 수 있다.                                                                                         그 이후도 각각 +4씩 증가

Explicit Stack Parameters = [EBP + offset]처럼 인자 위치를 직접 지정하는 방식
인자의 오프셋을 숫자로 명시하기 때문에 Explicit
가독성을 위해 보통 EQU로 이름을 붙여 사용한다
함수 인자는 EBP 기준으로 +8, +12, +16 … 순서로 증가한다

스택 프레임은 EBP를 기준점으로 만들고,
로컬 변수는 [EBP - n]에 저장하며,
함수 종료 시 mov esp, ebp로 로컬 변수를 한 번에 제거한다.

LEA는 실제 메모리를 읽지 않고 “주소만” 계산해서 레지스터에 넣는 명령어
이 예제에서는 myString 배열의 시작 주소를 얻기 위해 사용됨
그 주소를 ESI에 넣고, 반복문으로 배열을 '*'로 채움
마지막에 ESP 복구하여 로컬 배열 제거
C 코드의 동작과 1:1 대응되는 Assembly 구현

재귀 함수는 종료 조건이 필수
ECX에는 현재 더할 값이 저장됨
EAX는 누적 합을 저장하는 레지스터
재귀를 통해 ECX를 감소시키며 EAX에 계속 더함
ECX가 0이 되면 종료하고 EAX에 최종 합이 남음
