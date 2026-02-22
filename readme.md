# Kcode v4.0.0

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
|v4.1.0|★ C 코드 생성기 계약 시스템 변환 — kcodegen.c/h v2.1.0 업그레이드     |

-----

## ★★★ 법령/법위반 시스템 구현 진행도 ★★★
### ← 재시작 시 반드시 이 섹션부터 읽을 것

### 구현 단계 진행 현황

| 단계 | 파일                     | 작업 내용                                   | 상태       | 버전     |
|----|------------------------|------------------------------------------|----------|--------|
| 1  | klexer.h / klexer.c    | 계약 키워드 9개 토큰 추가                         | ✅ **완료** | v1.3.0 |
| 2  | kparser.h / kparser.c  | NODE_CONTRACT/SANCTION/CHECKPOINT + 파싱   | ✅ **완료** | v1.3.0 |
| 3  | kinterp.h / kinterp.c  | ContractRegistry + call_function 훅 구현   | ✅ **완료** | v4.0.0 |
| 4  | kcodegen.h / kcodegen.c| NODE_CONTRACT -> C assert 변환              | ✅ **완료** | v2.1.0 |
| 5  | readme.md              | 최종 문서화                                   | 🔄 매회갱신 | v4.1.0 |

-----

### ✅ 4단계 완료 — kcodegen.h / kcodegen.c → v2.1.0

#### 변경 파일
| 파일 | 변경 내용 |
|------|---------|
| `kcodegen.h` | 버전 v2.1.0, Codegen 구조체에 `has_contracts` 필드 추가 |
| `kcodegen.c` | 버전 v2.1.0, 계약 시스템 변환 로직 전체 추가 |

#### 추가된 함수 (kcodegen.c)
| 함수 | 역할 |
|------|------|
| `detect_contracts()` | AST 1패스 순회로 계약 노드 존재 여부 탐지 |
| `gen_cond_to_buf()` | 조건 노드를 임시 버퍼에 렌더링 (assert 삽입용) |
| `gen_contract()` | NODE_CONTRACT -> C 코드 변환 (법령/법위반 분기) |
| `gen_checkpoint()` | NODE_CHECKPOINT -> 복원지점 주석 출력 |

#### 법령/법위반별 C 코드 변환 결과

| 계약 종류 | 제재 | 변환 결과 |
|---------|------|---------|
| 법령 (사전조건) | 중단/회귀 | `assert((조건) && "[Kcode 법령 위반: 범위]");` |
| 법령 (사전조건) | 보고 | `if (!(조건)) { fprintf(stderr, ...); }` |
| 법령 (사전조건) | 대체 | `if (!(조건)) { return 대체값; }` |
| 법령 (사전조건) | 경고 | `if (!(조건)) { fprintf(stderr, ...); }` |
| 법위반 (사후조건) | 모두 | 상세 주석 출력 (v3.0 래퍼 방식 예정) |
| 복원지점 | — | 주석 출력 (C 런타임 상태 복원 미지원) |

#### 핵심 구현 포인트
- **has_contracts 선탐지**: `detect_contracts()`로 AST 전체를 1회 순회, 계약 노드 있을 때만 `#include <assert.h>` 자동 삽입
- **gen_cond_to_buf()**: 버퍼 롤백 방식으로 조건식을 임시 문자열로 추출 → assert 메시지에 삽입
- **법위반 C 코드 한계**: C에서 사후조건 완전 구현은 함수 래퍼 리팩토링 필요 → v3.0 예정, 현재 주석으로 대체
- **전방 선언 추가**: `gen_contract`, `gen_checkpoint`를 전방 선언 블록에 추가 (gen_stmt에서 호출하므로 필수)
- **문법 검증**: `gcc -std=c11 -Wall -Wextra -fsyntax-only` 경고 0개 통과 ✓

-----

### ✅ 3단계 완료 — kinterp.h / kinterp.c → v4.0.0

#### 추가된 구조체 (kinterp.h)
```c
ContractEntry   — kind(법령/법위반) / scope(범위명) / cond(조건AST)
                  sanction(제재토큰) / alt_name(회귀지점/대체함수) / alt_node(대체값AST)
CheckpointEntry — name(지점명) / snapshot(전역Env deep-copy)
Interp 필드 추가  — contracts[], contract_count/cap / checkpoints[], checkpoint_count/cap
```

#### 추가된 함수 (kinterp.c)
| 함수 | 역할 |
|------|------|
| `env_snapshot()` | 전역 Env deep-copy (복원지점 저장용) |
| `env_restore_from()` | 스냅샷으로 전역 Env 복원 (회귀 제재) |
| `contract_register()` | 계약 레지스트리에 항목 추가 |
| `checkpoint_register()` | 복원지점 등록 (동명이면 스냅샷 갱신) |
| `checkpoint_find()` | 복원지점 스냅샷 검색 |
| `apply_sanction()` | 제재 실행 — 경고/보고/중단/회귀/대체 |
| `check_contracts()` | 범위 일치 계약 순서대로 평가 |

#### 제재별 동작 검증 결과 ✓
```
경고  → stderr 출력 후 계속 실행                           ✓
보고  → kcode_contract.log 기록 후 계속 실행               ✓
중단  → val_error() 반환 → 실패시 블록으로 전달             ✓
회귀  → 복원지점 스냅샷으로 전역 상태 복원 후 오류 반환        ✓
대체값 → 지정값(-1 등) 반환, 함수 본문 나머지 실행 안 함       ✓
대체함수 → 전역에서 함수 찾아 호출                           ✓
```

#### 핵심 구현 포인트
- **외부 법령**: 등록만 → call_function 진입 후 매개변수 바인딩 완료 후 평가
- **내부 법령**: 등록 + 즉시 평가 (current != global 조건으로 함수 스코프에서만)
- **대체 제재 내부**: `val_return(대체값)` 으로 감싸 함수 본문 나머지 중단
- **법위반(사후조건)**: 실행 결과를 `"반환값"` 이름으로 전역에 임시 등록 후 평가
- **gc_mark_roots**: global + current 스코프 모두 마킹 (기존 버그 수정)
- **kgc.h**: `struct Env` 전방 선언 + `gc_mark_env` 선언 추가

-----

### ✅ 1,2단계 완료 요약

**1단계 klexer → v1.3.0**: 계약 키워드 9개 (법령/법위반/복원지점/전역/회귀/대체/경고/보고/중단)
- token_type_name[] 배열에서 계약 이름을 PP 전처리기 **앞에** 배치 (열거 인덱스 일치)

**2단계 kparser → v1.3.0**: NODE_CONTRACT/SANCTION/CHECKPOINT 파싱
- parse_contract(): 법령/법위반 [범위],[조건식],[제재][추가인수] 전체 파싱
- parse_checkpoint(): 복원지점 [이름] 파싱
- parse_stmt() 분기: TOK_KW_BEOPRYEONG/BEOPWIBAN/BOKWON 처리 추가
- NODE_NAMES[] 배열: SCRIPT_PYTHON/JAVA/JS 누락분 + CONTRACT/SANCTION/CHECKPOINT 추가

-----

### ⬜ 4단계 작업 내용 — kcodegen.c → v2.1.0

**재시작 시 필요 파일**: kcodegen.h, kcodegen.c (업로드)
**outputs에서 사용**: klexer.h/c, kparser.h/c, kinterp.h/c, kgc.h 모두 완료본

#### 작업 내용
```c
case NODE_CONTRACT:
    // 법령 → 함수 앞에 assert(조건); 삽입
    // 법위반 → 함수 뒤 래퍼 방식
    // #include <assert.h> 상단 자동 삽입 (최초 1회)

case NODE_CHECKPOINT:
    // 주석으로 처리 (C 런타임 복원 미지원)
    // /* [복원지점: 이름] */

case NODE_SANCTION:
    // NODE_CONTRACT 내부에서 처리, 독립 case 불필요
```

-----

## 파일 구조 현황

| 파일                    | 버전        | 상태                      |
|-----------------------|-----------|-------------------------|
| klexer.h / klexer.c   | **v1.3.0**| ✅ 계약 키워드 9개 추가 완료       |
| kparser.h / kparser.c | **v1.3.0**| ✅ 계약 파싱 완료              |
| kgc.h / kgc.c         | **v3.9.0**| ✅ GC 완료 + gc_mark_env 선언 추가|
| kinterp.h / kinterp.c | **v4.0.0**| ✅ 계약 시스템 완료             |
| kcodegen.h / kcodegen.c| **v2.1.0**| ✅ 계약 시스템 C 변환 완료        |
| kinterp_main.c        | v2.0.0    | —                       |
| kcodegen_llvm.h/.c    | v3.1.0    | —                       |
| kcodegen_llvm_main.c  | v3.5.0    | —                       |
| kserver.c             | v3.5.0    | —                       |
| CMakeLists.txt        | v3.5.0    | —                       |

-----

## 알려진 이슈

- 객체(class) / 예외 처리 LLVM 코드 생성 미구현 (v5.0 예정)
- 가속기 블록: NODE_GPU_BLOCK case 미구현 — B방식(CuPy) 구현 예정
