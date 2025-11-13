논리 이동(Logical Shift) - 빈 자리는 항상 0으로 채움. // SHR, SHL
산술 이동(Arithmetic Shift) - 부호 비트를 유지하면서 이동. // SAR, SAL
Rotate 명령어 (비트 회전) - 비트가 밀려서 없어지지 않고 반대쪽으로 돌아오는 이동 //ROL,ROR,RCL,RCR
Double-Precision Shift (두 레지스터를 연결한 이동) - 두 개의 레지스터를 하나처럼 연결해서 이동하는 명령어 // SHLD, SHRD

SHL(Shift Left)
비트를 왼쪽으로 이동시키는 명령어
밀려나간 최상위 비트는 CF(Carry Flag) 로 들어감
새로 생긴 오른쪽 비트는 0으로 채움
숫자 연산 효과: 값 × 2ⁿ

SAR / SHR / ROL / ROR
SHR: 오른쪽으로 이동(논리) — 빈자리 0
SAL: SHL과 동일
SAR: 오른쪽 이동(산술) — 부호비트 유지
ROL: 왼쪽 회전(버려지지 않고 반대편으로 돌아감)
ROR: 오른쪽 회전
RCL/RCR: CF 포함해서 회전
SHLD/SHRD: 두 레지스터를 이어붙여 이동

SAR: 오른쪽 산술 이동 → 부호 유지 → 값 ÷ 2ⁿ 효과
Signed division과 sign extension에 사용

SHL과 달리 비트가 사라지지 않고 반대편으로 되돌아온다.
최상위 비트 → CF + 최하위 비트 위치
회전(rotation)이므로 데이터 손실 없음

MUL은 unsigned 곱셈이며, 결과는 항상 2배 크기 레지스터에 저장된다 (AX / DX:AX / EDX:EAX)

idiv는 32비트 부호값을 필요로 하므로, AX에 음수 넣었으면 cwd로 부호 확장을 꼭 해야 한다.

div 하기 전에 divisor가 0인지 검사하고, 0이면 에러 처리 루틴으로 점프한다.
