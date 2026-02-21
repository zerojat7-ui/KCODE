# Kcode v3.4.0

## 버전 히스토리

|버전    |내용                                               |
|------|-------------------------------------------------|
|v0.7.1|렉서 완료                                            |
|v0.8.0|파서 완료                                            |
|v0.9.0|인터프리터 완료                                         |
|v1.0.0|C 코드 생성기 완료                                      |
|v1.1.0|스크립트 블록 추가 — Python / Java / JavaScript          |
|v1.2.0|가속기 블록 도입, 끝* → *끝 후위 변경                         |
|v1.2.1|Makefile 완전 재작성 — LLVM 연동 준비                     |
|v2.0.0|글자 ↔ 문자 자료형 명칭 전면 교체                             |
|v2.1.0|LLVM IR 코드 생성기 신규 작성 (kcodegen_llvm.h / .c)      |
|v3.0.0|LLVM 코드 생성기 IDE 완전 호환 — 소스맵 + JSON + IDE 서버 모드 추가|
|v3.1.0|최적화 전/후 IR 동시 제공 (ir_text_unopt — IDE 미리보기용)     |
|v3.1.1|컴파일 결과물에 iOS .ipa 확장자 추가 (문서)                    |
|v3.2.0|kcodegen_llvm_main.c 작성 — LLVM 드라이버 완성 (4단계 완료)  |
|v3.3.0|kserver.c 작성 — 로컬 HTTP 서버 완성 (5단계 완료)            |
|v3.4.0|코드 검증 및 오류 수정 — kserver.c 유니코드 따옴표/대시 전면 수정, JSON 이스케이프 수정, kcodegen_llvm_main.c 백슬래시 문자 리터럴 수정|

-----

## IDE 연동 프로토콜 (kcodegen 과 완전 동일)

### –ide 서버 모드

```
stdin  ← {"action":"compile", "source":"...han 소스..."}
stdout → {
  "success": true|false,
  "ir_text": "LLVM IR 텍스트",
  "ir_text_unopt": "최적화 전 IR (미리보기용)",
  "sourcemap": [{"han_line":N, "han_col":N, "ir_line":N}, ...],
  "errors":   [{"line":N, "col":N, "msg":"..."}, ...],
  "stats":    {"func_count":N, "var_count":N, "ir_line_count":N}
}
```

### 전체 실행 모드

```bash
./kcode_llvm                          # 내장 테스트
./kcode_llvm hello.han                # LLVM IR stdout 출력
./kcode_llvm hello.han -o hello.ll    # IR 파일 저장
./kcode_llvm hello.han -b             # clang으로 네이티브 빌드
./kcode_llvm hello.han -b -o hello    # 실행파일 이름 지정
./kcode_llvm hello.han --bc           # LLVM 비트코드(.bc) 저장
./kcode_llvm hello.han --json         # IDE 연동 JSON 출력
./kcode_llvm hello.han --unopt        # 최적화 전 IR 출력
./kcode_llvm --ide                    # IDE 서버 모드 (stdin JSON)
```

### 소스맵 구조

|필드        |설명                |
|----------|------------------|
|`han_line`|.han 원본 라인 번호     |
|`han_col` |.han 원본 열 번호      |
|`ir_line` |대응하는 LLVM IR 라인 번호|

IDE는 소스맵으로 IR 오류 위치를 한글 소스에 하이라이트한다.

-----

## 파일 구조

|파일                     |버전        |설명                 |
|-----------------------|----------|-------------------|
|klexer.h / klexer.c    |v2.0.0    |렉서                 |
|kparser.h / kparser.c  |v1.2.0    |파서                 |
|kinterp.h / kinterp.c  |v2.0.0    |인터프리터              |
|kinterp_main.c         |v2.0.0    |인터프리터 진입점          |
|kcodegen.h / kcodegen.c|v2.0.0    |C 코드 생성기           |
|kcodegen_main.c        |v1.0.0    |C 생성기 진입점          |
|kcodegen_llvm.h        |v3.1.0    |LLVM 코드 생성기 헤더     |
|kcodegen_llvm.c        |v3.1.0    |LLVM 코드 생성기 구현     |
|kcodegen_llvm_main.c   |v3.4.0    |LLVM 드라이버 (백슬래시 리터럴 수정) |
|**kserver.c**          |**v3.4.0**|**로컬 HTTP 서버 (유니코드/JSON 수정)**|
|ktest_lexer.c          |v0.7.0    |렉서 단위 테스트          |
|Makefile               |v1.2.1    |빌드 파일              |

-----

## kcodegen vs kcodegen_llvm IDE 호환 비교

|항목                |kcodegen (C 생성기)|kcodegen_llvm (LLVM 생성기)|
|------------------|----------------|------------------------|
|–ide 서버 모드        |✅               |✅ v3.0.0                |
|–json 출력          |✅               |✅ v3.0.0                |
|소스맵               |✅ han↔c_line    |✅ v3.0.0 han↔ir_line    |
|오류 위치             |✅               |✅ v3.0.0                |
|통계 (func/var/line)|✅               |✅ v3.0.0                |
|–bc 비트코드 저장       |❌               |✅ v3.2.0                |
|–unopt 최적화 전 IR   |❌               |✅ v3.2.0                |

-----

## 빌드

```bash
# 기존 빌드 (LLVM 없이)
make all

# LLVM 포함 빌드
make LLVM=1

# LLVM 설치 확인
make llvm-check
```

### 수동 빌드

```bash
gcc $(llvm-config --cflags) -DKCODE_LLVM \
    -o kcode_llvm \
    klexer.c kparser.c kcodegen_llvm.c kcodegen_llvm_main.c \
    $(llvm-config --ldflags --libs core analysis native bitwriter)
```

-----

## kcodegen_llvm 지원 범위

|기능                           |상태       |
|-----------------------------|---------|
|정수(i64) / 실수(double) / 논리(i1)|✅        |
|문자(i32) / 글자(i8*, string)    |✅        |
|변수 선언 및 대입                   |✅        |
|산술 / 비교 / 논리 / 비트 연산         |✅        |
|함수 선언 / 정의 / 호출              |✅        |
|내장 출력 → printf 래핑            |✅        |
|만약 / 아니면 조건문                 |✅        |
|동안 반복 / 반복 범위                |✅        |
|반환 / 멈춤 / 건너뜀                |✅        |
|mem2reg + -O2 최적화            |✅        |
|소스맵 (.han ↔ ir_line)         |✅ v3.0.0 |
|IDE JSON 출력                  |✅ v3.0.0 |
|IDE 서버 모드                    |✅ v3.0.0 |
|LLVM 드라이버 (main)             |✅ v3.2.0 |
|배열 / 각각(foreach)             |⬜ v3.3 예정|
|객체(class) / 예외 처리            |⬜ v3.4 예정|

-----

## ✅ 5단계 완료: kserver.c — 로컬 HTTP 서버

```bash
# 빌드
gcc -O2 -o kserver kserver.c

# 실행
./kserver               # 기본 포트 8080
./kserver 3000          # 포트 지정
./kserver 8080 --cgen   # C 코드 생성기 사용

# 테스트
curl http://localhost:8080/health
curl -X POST http://localhost:8080/compile \
     -H 'Content-Type: application/json' \
     -d '{"action":"compile","source":"출력(1+2)"}'
```

|엔드포인트   |메서드    |설명              |
|--------|-------|----------------|
|/compile|POST   |소스 컴파일 → JSON 응답|
|/health |GET    |서버 상태 확인        |
|/version|GET    |컴파일러 버전 반환      |
|*       |OPTIONS|CORS preflight  |

-----

## ★ 6단계 진입 가이드 ← 다음 작업 시작점

### 6단계 목표: 웹 IDE 작성 — GitHub Pages 배포

kserver와 통신하는 브라우저 기반 Kcode IDE.

**구현할 기능:**

```
- 한글 코드 에디터 (CodeMirror 또는 Monaco)
- 컴파일 버튼 → POST /compile → 결과 표시
- IR 뷰어 패널 (최적화 전/후 비교)
- 오류 하이라이트 (소스맵 활용)
- 결과 파일 다운로드
- /health 로 서버 연결 상태 표시
```

**파일 구조:**

```
docs/               ← GitHub Pages 루트
  index.html        ← IDE 메인 페이지
  style.css         ← 스타일
  ide.js            ← IDE 로직 (kserver 통신)
  README.md         ← 사용 방법
```

-----

## 알려진 이슈

- 렉서 테스트 2건 FAIL: `ktest_lexer.c` 의 UTF-8 바이트 오타 (`문자`, `건너뜀`)