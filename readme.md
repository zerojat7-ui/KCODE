# Kcode v6.2.0

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
|v3.2.0|kcodegen_llvm_main.c 작성 — LLVM 드라이버 완성            |
|v3.3.0|kserver.c 작성 — 로컬 HTTP 서버 완성                     |
|v3.4.0|코드 검증 및 오류 수정                                    |
|v3.5.0|CMake 빌드 시스템 추가                                   |
|v3.6.0|AI 수학 내장 함수 7종 추가                                 |
|v3.7.1|수학 기초 함수 9종 추가                                    |
|v3.8.0|통계 함수 13종 + AI 활성함수 3종 + 호감도 함수 추가               |
|v3.9.0|★ GC 통합 — kgc.h/kgc.c 신규, 참조카운트 + 마크스윕           |
|v4.0.0|★ 법령/법위반 계약 시스템 — 사전/사후조건 + 6종 제재 + 복원지점       |
|v4.1.0|★ C 코드 생성기 계약 시스템 변환 — kcodegen.c/h v2.1.0 업그레이드|
|v4.1.1|★ 법령/법위반 계약 시스템 전 단계 문서화 최종 완료                  |
|v4.2.0|★ 계약 시스템 전면 개편 명세 확정 — 헌법/법률/규정 계층 + 파일 내장 함수 |
|v5.0.0|★ 버그 수정 + 계약 v2.0 구현 완료 — kgc.h/kinterp.c/kcodegen.c 3파일 수정|
|v5.1.0|★ 법위반(사후조건) 함수 래퍼 완전 구현 — kcodegen.h/kcodegen.c|
|v5.2.0|★ 객체/예외/GPU/계약 LLVM IR 변환 완전 구현 — kcodegen_llvm.h/kcodegen_llvm.c|
|v6.0.0|★ 인터럽트 시스템 3종(A:OS시그널/B:하드웨어간섭/C:행사) 전체 구현 — 6파일 수정|
|v6.1.0|★ #포함/가짐 모듈 시스템 — .han/.hg/.c/.cpp/.py/.js/.ts/.java 8종 확장자 지원|
|v6.2.0|★ 객체(CLASS) vtable 완전 구현 — forward typedef + vtable init + _new 팩토리 / LLVM IR CLASS 변환 추가|
|v6.2.0|★ 문법 명세 전면 동기화 — Kcode_grammar.md/readme_skil.md v6.2.0 업데이트 (자료형 오류 수정 + 계약/인터럽트/파일I/O/모듈 섹션 추가)|

-----

## ★★★ 계약 시스템 v2.0 개편 구현 진행도 ★★★
### ← 재시작 시 반드시 이 섹션부터 읽을 것

### 개편 배경

기존 인라인 문법(법령 [범위],[조건],[제재])을 블록 방식으로 전환하고,
실제 법률 체계와 동일한 5계층 구조를 도입합니다.

```
헌법    ← 모든 파일 전역 (인라인)        헌법 조건, 제재
법률    ← 현재 파일 전체 (인라인)        법률 조건, 제재
규정    ← 특정 객체 전체 메서드 (블록)    규정 객체명: / 조항 / 제재 / 규정끝
법령    ← 특정 함수 사전조건 (블록)       법령 함수명: / 조항 / 제재 / 법령끝
법위반  ← 특정 함수 사후조건 (블록)       법위반 함수명: / 조항 / 제재 / 법위반끝
```

우선순위: 헌법 → 법률 → 규정 → 법령 → 법위반 순 평가
상위 계층에서 중단 제재 발생 시 하위 계층은 평가하지 않습니다.

-----

### 구현 단계 진행 현황 (v6.0.0 기준)

| 단계 | 파일 | 작업 내용 | 상태 | 버전 |
|------|------|---------|------|------|
| 1 | klexer.h / klexer.c | 신규 토큰 추가 (헌법/법률/규정/조항/법령끝/법위반끝/규정끝 + 파일 함수 16개) | ✅ 완료 | v1.4.0 |
| 2 | kparser.h / kparser.c | NODE_CONSTITUTION/STATUTE/REGULATION 추가 + 블록 파싱 전환 | ✅ 완료 | v1.4.0 |
| 3 | kinterp.h / kinterp.c | NODE_CONSTITUTION/STATUTE/REGULATION eval + 계층 우선순위 + 파일 내장함수 17종 | ✅ 완료 | v5.0.0 |
| 4 | kcodegen.h / kcodegen.c | 새 계층 노드 C 코드 변환 | ✅ 완료 | v5.0.0 |
| 5 | readme.md | 최종 문서화 | ✅ 완료 | v6.0.0 |

-----

## ★★★ 객체(CLASS) vtable v6.2.0 구현 완료 기록 ★★★

### 구현 완료 현황 (v6.2.0)

| 단계 | 파일 | 작업 내용 | 상태 |
|------|------|----------|------|
| 1 | kcodegen.h | 버전 v6.2.0 업데이트 | ✅ 완료 |
| 2 | kcodegen.c | NODE_CLASS_DECL — forward typedef + vtable_t + struct + 전방선언 + vtable_init + _new 팩토리 (v6.2.0) | ✅ 완료 |
| 3 | kcodegen_llvm.h | 버전 v6.2.0 + LLVMClassEntry 구조체 + class_reg[64] / class_count 필드 추가 | ✅ 완료 |
| 4 | kcodegen_llvm.c | gen_class_decl() — struct타입/vtable타입/메서드함수IR/vtable_init/new_IR 생성 | ✅ 완료 |
| 5 | kcodegen_llvm.c | gen_program() — 1단계에서 CLASS_DECL 처리, 2단계 main에서 건너뜀 | ✅ 완료 |
| 6 | kcodegen_llvm.c | gen_stmt() — NODE_CLASS_DECL case 추가 | ✅ 완료 |

### C 코드 생성 패턴 (kcodegen.c v6.2.0)
```c
typedef struct kc_동물 kc_동물;           /* ① forward typedef */
typedef struct kc_동물_vtable_t {         /* ② vtable 타입 */
    kc_value_t (*kc_소리내기)(kc_동물*);
} kc_동물_vtable_t;
struct kc_동물 { kc_동물_vtable_t *kc__vt; long long kc_나이; };  /* ③ 구조체 */
kc_value_t kc_동물_소리내기(kc_동물*);   /* ④ 전방선언 */
static kc_동물_vtable_t kc_동물_vtable;  /* ⑤ 전역 vtable */
/* ⑥ 메서드 구현, ⑦ vtable_init, ⑧ _new 팩토리 */
```

### LLVM IR 생성 구조
```llvm
%kc_동물_vtable_t = type { i8*, ... }        ; 메서드 포인터 배열
%kc_동물 = type { %kc_동물_vtable_t*, i64 } ; vtable* + 필드
@kc_동물_vtable = internal global %kc_동물_vtable_t zeroinitializer
define internal void @kc_동물_vtable_init() { ... }
define internal %kc_동물* @kc_동물_new() { malloc + vtable 주입 }
```

-----

## ★★★ #포함/가짐 모듈 시스템 v6.1.0 구현 완료 기록 ★★★

### 구현 완료 현황 (v6.1.0)

| 단계 | 파일 | 작업 내용 | 상태 |
|------|------|----------|------|
| 1 | kparser.c | parse_pp_stmt() — TOK_PP_INCLUDE 파일명 파싱 (따옴표/꺽쇠/식별자) | ✅ 완료 |
| 2 | kinterp.c | NODE_PP_STMT(#포함) — 확장자별 8종 로더 구현 | ✅ 완료 |
| 3 | kinterp.c | NODE_IMPORT(가짐) — 내장 모듈 레지스트리 + 외부 파일 자동 탐색 | ✅ 완료 |
| 4 | kcodegen.c | NODE_PP_STMT(#포함) — 확장자별 #include / 힌트 주석 변환 | ✅ 완료 |
| 5 | kcodegen.c | NODE_IMPORT(가짐) — 내장 모듈 → C 표준 헤더 매핑 | ✅ 완료 |

### 지원 확장자 및 처리 전략

| 확장자 | 언어 | 인터프리터 | C 코드 생성 |
|--------|------|-----------|------------|
| `.han` | Kcode 소스 | 파싱 후 현재 환경에서 실행 | `/* 주석 */` |
| `.hg`  | Kcode 헤더 | 동일 | `/* 주석 */` |
| `.c`   | C | 경고 출력 후 무시 | `#include "파일"` |
| `.h`   | C 헤더 | 경고 출력 후 무시 | `#include "파일"` |
| `.cpp` | C++ | 경고 출력 후 무시 | `#ifdef __cplusplus #include "파일" #endif` |
| `.hpp` | C++ 헤더 | 경고 출력 후 무시 | 동일 |
| `.py`  | Python | python3 실행 → stdout 반환 | `#ifdef KCODE_EMBED_PYTHON #include <Python.h>` |
| `.js`  | JavaScript | node 실행 → stdout 반환 | `/* 힌트 주석 */` |
| `.ts`  | TypeScript | ts-node / tsc+node 실행 → stdout 반환 | `/* 힌트 주석 */` |
| `.java`| Java | javac 컴파일 후 java 실행 → stdout 반환 | `/* JNI 힌트 주석 */` |

### 가짐(import) 내장 모듈 → C 헤더 매핑

| Kcode 모듈 | C 헤더 |
|-----------|--------|
| 수학 / 수학함수 | `<math.h>` |
| 파일시스템 | `<stdio.h>` + `<dirent.h>` + `<sys/stat.h>` |
| 문자열 | `<string.h>` |
| 시간 | `<time.h>` |
| 난수 | `<stdlib.h>` |

-----

## ★★★ 인터럽트 시스템 v6.0.0 구현 완료 기록 ★★★

### 구현 완료 현황 (v6.0.0 기준)

| 단계 | 파일 | 작업 내용 | 상태 | 버전 |
|------|------|---------|------|------|
| 1 | klexer.h / klexer.c | 인터럽트 키워드 15종 토큰 추가 (신호/간섭/행사 관련) | ✅ 완료 | v6.0.0 |
| 2 | kparser.h / kparser.c | NODE_SIGNAL_HANDLER/CTRL, NODE_ISR_HANDLER/CTRL, NODE_EVENT_HANDLER/CTRL 추가 | ✅ 완료 | v6.0.0 |
| 3 | kinterp.h / kinterp.c | SignalEntry / IsrEntry / EventEntry 구조체 + eval 구현 + signal()/kill() 연동 | ✅ 완료 | v6.0.0 |
| 4 | kcodegen.h / kcodegen.c | 신호→signal()+핸들러 / 간섭→ISR 매크로 / 행사→이벤트루프 C코드 변환 | ✅ 완료 | v6.0.0 |
| 5 | kcodegen_llvm.h / kcodegen_llvm.c | 버전 상속 (v6.0.0) | ✅ 완료 | v6.0.0 |

#### 인터럽트 3종 동작 검증 ✓
```
A: OS 시그널  → signal(SIGINT, ...) 등록 / kill(pid, SIG) 발송       ✓
B: 하드웨어 간섭 → AVR ISR() 매크로 / cli()/sei() 잠금 제어          ✓
C: 행사(이벤트) → kc_event_register() 테이블 / 행사시작(루프) 진입   ✓
```

#### 추가된 토큰 목록

| 토큰 상수 | 한글 키워드 | 설명 |
|----------|-----------|------|
| TOK_KW_SINHOBATGI | 신호받기 | OS 시그널 핸들러 등록 |
| TOK_KW_SINHOMUSI | 신호무시 | SIG_IGN |
| TOK_KW_SINHOGIBON | 신호기본 | SIG_DFL |
| TOK_KW_SINHOBONEGI | 신호보내기 | kill(pid, sig) |
| TOK_KW_GANSEOB | 간섭 | ISR 블록 등록 |
| TOK_KW_GANSEOB_JAMGEUM | 간섭잠금 | cli() / noInterrupts() |
| TOK_KW_GANSEOB_HEOYONG | 간섭허용 | sei() / interrupts() |
| TOK_KW_HAENGSA_REG | 행사등록 | 이벤트 핸들러 등록 |
| TOK_KW_HAENGSA_START | 행사시작 | 이벤트 루프 진입 |
| TOK_KW_HAENGSA_STOP | 행사중단 | 이벤트 루프 종료 |
| TOK_KW_HAENGSA_EMIT | 행사발생 | 이벤트 수동 발생 |
| TOK_KW_HAENGSA_OFF | 행사해제 | 핸들러 제거 |

-----

## ★★★ 계약 시스템 v1.0 구현 완료 기록 ★★★

### 구현 완료 현황 (v4.1.1 기준)

| 단계 | 파일 | 작업 내용 | 상태 | 버전 |
|------|------|---------|------|------|
| 1 | klexer.h / klexer.c | 계약 키워드 9개 토큰 추가 | ✅ 완료 | v1.3.0 |
| 2 | kparser.h / kparser.c | NODE_CONTRACT/SANCTION/CHECKPOINT + 파싱 | ✅ 완료 | v1.3.0 |
| 3 | kinterp.h / kinterp.c | ContractRegistry + call_function 훅 구현 | ✅ 완료 | v4.0.0 |
| 4 | kcodegen.h / kcodegen.c | NODE_CONTRACT → C assert 변환 | ✅ 완료 | v2.1.0 |
| 5 | readme.md | 최종 문서화 | ✅ 완료 | v4.1.1 |

-----

## 파일 구조 현황

### 핵심 엔진 파일

| 파일 | 버전 | 상태 | 주요 변경 |
|------|------|------|---------|
| klexer.h / klexer.c | **v6.0.0** | ✅ 완료 | 인터럽트 키워드 추가 (신호받기/간섭/행사 등 15종) |
| kparser.h / kparser.c | **v6.1.0** | ✅ 완료 | #포함 파일명 파싱 추가 (꺽쇠/따옴표/식별자) |
| kgc.h / kgc.c | **v5.0.0** | ✅ 완료 | GC_INSTANCE + gc_new_instance + gc_mark_value |
| kinterp.h / kinterp.c | **v6.1.0** | ✅ 완료 | #포함 .han/.hg/.c/.cpp/.py/.js/.ts/.java 8종 로더 + 가짐 모듈 시스템 |
| kcodegen.h / kcodegen.c | **v6.2.0** | ✅ 완료 | 객체 vtable 완전 구현 (forward typedef + vtable_t + vtable_init + _new 팩토리) |
| kcodegen_llvm.h / kcodegen_llvm.c | **v6.2.0** | ✅ 완료 | CLASS IR 변환 — struct/vtable/메서드/vtable_init/_new 팩토리 IR 생성 |
| kcode_raw_System.md | **v2.0.0** | ✅ 완료 | 계약 시스템 5계층 명세 |

### 드라이버 및 빌드 파일

| 파일 | 버전 | 상태 | 설명 |
|------|------|------|------|
| kinterp_main.c | **v6.0.0** | ✅ 완료 | 인터프리터 진입점 (버전 상속) |
| kcodegen_main.c | **v6.0.0** | ✅ 완료 | C 코드 생성기 드라이버 (버전 상속) |
| kcodegen_llvm_main.c | **v6.0.0** | ✅ 완료 | LLVM 드라이버 (버전 상속) |
| kserver.c | **v6.0.0** | ✅ 완료 | 로컬 HTTP 서버 (버전 상속) |
| CMakeLists.txt | **v6.0.0** | ✅ 완료 | CMake 빌드 시스템 (버전 상속) |
| Makefile | **v6.0.0** | ✅ 완료 | 빌드 규칙 (버전 상속) |

### 테스트 / 문서 파일

| 파일 | 버전 | 상태 | 설명 |
|------|------|------|------|
| klexer_main.c | v0.7.1 | ✅ 유지 | 렉서 단독 테스트 드라이버 |
| kparser_main.c | v0.8.0 | ✅ 유지 | 파서 단독 테스트 드라이버 |
| ktest_lexer.c | v0.7.1 | ✅ 유지 | 렉서 단위 테스트 (CTest 연동) |
| INSTALL_AND_DLL_GUIDE.md | v3.4.0 | 📝 업데이트 필요 | 설치 및 공유 라이브러리 가이드 |
| readme_skil.md | **v6.2.0** | ✅ 업데이트 | 언어 설계 명세 — 키워드 전체 목록 동기화 (계약/인터럽트/파일I/O 추가) |
| Kcode_grammar.md | **v6.2.0** | ✅ 업데이트 | 문법 레퍼런스 — 자료형 명칭 수정 + 계약/파일I/O/인터럽트/모듈 섹션 추가 |
| KCODE_IDE_UI_제작배경_정리문서.docx | — | 📝 참고용 | IDE UI 설계 배경 문서 |

-----

## ★★★ 미구현 항목 우선순위 로드맵 ★★★
### ← 재시작 시 이 섹션도 반드시 확인

### 전체 미구현 현황 (v6.2.0 기준)

| 항목 | 인터프리터 | kcodegen.c | kcodegen_llvm.c | 우선순위 |
|------|-----------|-----------|----------------|---------|
| **#포함 / 가짐** | ✅ 완료 | ✅ 완료 | ❌ 없음 | **완료** |
| **객체(CLASS_DECL)** | ✅ 완료 | ✅ vtable 완료 | ✅ IR 완료 | **완료** |
| **스크립트 블록** | ✅ 완료 | ❌ case 없음 | ❌ 없음 | **1순위** |
| **가속기(GPU)** | ❌ 무시 | ⚠️ OpenMP 힌트만 | ❌ 없음 | **2순위** |
| **kcodegen_llvm 일괄** | — | — | ❌ 예외/선택/goto/계약/인터럽트 등 | **3순위** |

### 우선순위 상세 계획

#### ~~1순위 — #포함 / 가짐 (v6.1.0)~~ ✅ 완료

#### ~~2순위 — 객체(CLASS) vtable / LLVM (v6.2.0)~~ ✅ 완료
```
  kcodegen.c      — forward typedef + vtable 타입 + 객체 구조체 + vtable_init + _new 팩토리
  kcodegen_llvm.c — gen_class_decl(): struct/vtable IR, 메서드 함수, vtable_init, _new
  kcodegen_llvm.h — LLVMClassEntry + class_reg[LLVM_CLASS_MAX] 추가
```

#### 1순위 — 스크립트 블록 코드 생성 (v6.3.0)
```
  kcodegen.c      — NODE_SCRIPT_PYTHON/JAVA/JS: system() 호출 C코드 생성
  kcodegen_llvm.c — 외부 프로세스 호출 IR 변환
```

#### 2순위 — 가속기(GPU) 실제 구현 (v6.4.0)
```
  kinterp.c       — GPU 블록 CuPy/NumPy 위임 실행
  kcodegen_llvm.c — NVPTX 타깃 PTX 코드 생성
```

#### 3순위 — kcodegen_llvm 미구현 노드 일괄 (v7.0.0)
```
  예외(시도/실패시), 선택(switch), goto/label,
  계약 시스템, 인터럽트 시스템, 스크립트/GPU
  → 앞자리 올라가 v7.0.0
```

-----

## ★★★ 객체(CLASS) vtable 시스템 v6.2.0 구현 완료 기록 ★★★

### 구현 완료 현황 (v6.2.0)

| 단계 | 파일 | 작업 내용 | 상태 |
|------|------|----------|------|
| 1 | kcodegen.h / kcodegen.c | forward typedef + vtable_t 구조체 + 객체 struct + 전방선언 + vtable_init + _new 팩토리 | ✅ 완료 |
| 2 | kcodegen_llvm.h | LLVMClassEntry 구조체 + class_reg[64] + class_count 필드 추가 | ✅ 완료 |
| 3 | kcodegen_llvm.c | gen_class_decl() 신규 — struct/vtable IR, 메서드 함수 IR, vtable_init IR, _new IR | ✅ 완료 |
| 4 | kcodegen_llvm.c | gen_program() 1단계에 CLASS_DECL 처리 추가 / 2단계 main에서 건너뜀 | ✅ 완료 |
| 5 | kcodegen_llvm.c | gen_stmt()에 NODE_CLASS_DECL case 추가 | ✅ 완료 |

### C 코드 생성 패턴 (kcodegen.c v6.2.0)
```c
/* ① forward typedef */
typedef struct kc_동물 kc_동물;

/* ② vtable 타입 */
typedef struct kc_동물_vtable_t {
    kc_value_t (*kc_소리내기)(kc_동물*);
} kc_동물_vtable_t;

/* ③ 객체 구조체 (vtable* 첫 필드) */
struct kc_동물 {
    kc_동물_vtable_t *kc__vt;
    long long kc_나이;
};

/* ④ 메서드 전방선언 */
kc_value_t kc_동물_소리내기(kc_동물 *kc_자신);

/* ⑤ 전역 vtable 인스턴스 */
static kc_동물_vtable_t kc_동물_vtable;

/* ⑥ 메서드 구현 */
/* ⑦ vtable_init — 포인터 채움 */
/* ⑧ _new 팩토리 — malloc + vtable 주입 + 생성자 호출 */
```

-----

## 알려진 이슈 / 다음 단계

- ~~**v5.1** : 법위반(사후조건) 계층 kcodegen.c 완전 변환~~ ✅ 완료
- ~~**v5.2** : 객체(class) / 예외 처리 LLVM 코드 생성~~ ✅ 완료
- ~~**v5.2** : 가속기 블록 LLVM IR 변환~~ ✅ 완료 (메타데이터 마킹)
- ~~**v5.3 / v6.0** : 인터럽트 시스템 3종 추가~~ ✅ 완료
- ~~**v6.1** : #포함 / 가짐 — .han/.hg/.c/.cpp/.py/.js/.ts/.java 확장자 지원~~ ✅ 완료
- ~~**v6.2** : 객체(CLASS) vtable + kcodegen_llvm 완전 구현~~ ✅ 완료
- **v6.3** : 스크립트 블록 kcodegen.c / kcodegen_llvm.c 구현
- **v6.4** : 가속기(GPU) 인터프리터 + NVPTX 실제 구현
- **v7.0** : kcodegen_llvm 미구현 노드 일괄 완성
