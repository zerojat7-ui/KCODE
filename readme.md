# Kcode v5.0.0

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

### 구현 단계 진행 현황 (v5.0.0 기준)

| 단계 | 파일 | 작업 내용 | 상태 | 버전 |
|------|------|---------|------|------|
| 1 | klexer.h / klexer.c | 신규 토큰 추가 (헌법/법률/규정/조항/법령끝/법위반끝/규정끝 + 파일 함수 16개) | ✅ 완료 | v1.4.0 |
| 2 | kparser.h / kparser.c | NODE_CONSTITUTION/STATUTE/REGULATION 추가 + 블록 파싱 전환 | ✅ 완료 | v1.4.0 |
| 3 | kinterp.h / kinterp.c | NODE_CONSTITUTION/STATUTE/REGULATION eval + 계층 우선순위 + 파일 내장함수 17종 | ✅ 완료 | v5.0.0 |
| 4 | kcodegen.h / kcodegen.c | 새 계층 노드 C 코드 변환 | ✅ 완료 | v5.0.0 |
| 5 | readme.md | 최종 문서화 | ✅ 완료 | v5.0.0 |

-----

### ⬜ 1단계 예정 — klexer.h / klexer.c

추가할 토큰 목록

| 토큰 상수 | 한글 키워드 | 설명 |
|----------|-----------|------|
| TOK_KW_HUNBEOP | 헌법 | 전역 계약 최상위 |
| TOK_KW_BEOPRYUL | 법률 | 파일 단위 계약 |
| TOK_KW_GYUJUNG | 규정 | 객체 단위 계약 블록 시작 |
| TOK_KW_GYUJUNG_END | 규정끝 | 규정 블록 종료 |
| TOK_KW_JOHANG | 조항 | 블록 내 조건 선언 |
| TOK_KW_BEOPRYEONG_END | 법령끝 | 법령 블록 종료 |
| TOK_KW_BEOPWIBAN_END | 법위반끝 | 법위반 블록 종료 |
| TOK_KW_FILE_CLOSE | 파일닫기 | 파일 닫기 |
| TOK_KW_FILE_READ | 파일읽기 | 전체 내용 읽기 |
| TOK_KW_FILE_READLINE | 파일줄읽기 | 한 줄 읽기 |
| TOK_KW_FILE_WRITE | 파일쓰기 | 내용 쓰기 |
| TOK_KW_FILE_WRITELINE | 파일줄쓰기 | 줄바꿈 포함 쓰기 |
| TOK_KW_FILE_EXISTS | 파일있음 | 존재 여부 논리 반환 |
| TOK_KW_FILE_SIZE | 파일크기 | 바이트 크기 반환 |
| TOK_KW_FILE_LIST | 파일목록 | 폴더 내 목록 배열 반환 |
| TOK_KW_DIR_MAKE | 폴더만들기 | 폴더 생성 |
| TOK_KW_FILE_DELETE | 파일지우기 | 파일 삭제 |
| TOK_KW_FILE_COPY | 파일복사 | 파일 복사 |
| TOK_KW_FILE_MOVE | 파일이동 | 파일 이동/이름변경 |
| TOK_KW_FILE_NAME | 파일이름 | 경로에서 파일명 추출 |
| TOK_KW_FILE_EXT | 파일확장자 | 확장자 추출 |
| TOK_KW_FILE_READALL | 파일전체읽기 | 열기+읽기+닫기 한번에 |
| TOK_KW_FILE_WRITEALL | 파일전체쓰기 | 열기+쓰기+닫기 한번에 |

주의사항
- 기존 TOK_KW_BEOPRYEONG / TOK_KW_BEOPWIBAN 유지 (블록 시작 역할)
- 기존 TOK_KW_JEONYEOK (전역) 제거 → 헌법/법률 토큰으로 대체
- TOK_KW_FILEOPEN 기존 토큰 유지 (파일열기는 이미 존재)
- token_type_name[] 배열 인덱스 순서 반드시 열거형과 일치 확인

-----

### ⬜ 2단계 예정 — kparser.h / kparser.c

추가할 노드 타입

| 노드 | 설명 |
|------|------|
| NODE_CONSTITUTION | 헌법 (인라인) — child[0]=조건, op=제재 |
| NODE_LAW | 법률 (인라인) — child[0]=조건, op=제재 |
| NODE_REGULATION | 규정 (블록) — sval=객체명, child[0]=조항, child[1]=제재 |
| NODE_JOHANG | 조항 — child[0]=조건 표현식 |
| NODE_CONTRACT | 법령/법위반 (블록 방식으로 재정의) |

파싱 변경 사항
- parse_contract() 전면 재작성 → 블록 방식 (법령끝/법위반끝 감지)
- parse_regulation() 신규 추가 (규정끝 감지)
- parse_constitution() / parse_law() 신규 추가 (인라인)
- parse_johang() 신규 추가
- parse_stmt() 분기에 헌법/법률/규정 토큰 처리 추가
- NODE_NAMES[] 배열에 신규 노드 이름 추가

-----

### ⬜ 3단계 예정 — kinterp.h / kinterp.c

계층 우선순위 평가 로직
```c
/* check_contracts_layered() 실행 순서 */
1. 헌법 목록 전부 평가
2. 법률 목록 평가 (현재 파일 일치)
3. 규정 목록 평가 (객체명 일치)
4. 법령 목록 평가 (함수명 일치)
/* 중단 제재 발생 시 즉시 리턴 — 하위 계층 평가 안 함 */
```

추가할 구조체
```c
typedef struct {
    TokenType kind;       /* TOK_KW_HUNBEOP / BEOPRYUL / GYUJUNG */
    char      scope[128]; /* 객체명 (규정만 사용) */
    Node     *cond;       /* 조항 조건 AST */
    TokenType sanction;   /* 제재 토큰 */
    char      alt[128];   /* 대체값/함수/회귀지점 */
    Node     *alt_node;
} LayeredContractEntry;
```

파일 내장 함수 17종 C 구현 매핑

| Kcode 함수 | C 구현 |
|-----------|------|
| 파일열기 / 파일닫기 | fopen / fclose 래핑 |
| 파일읽기 / 파일줄읽기 | fread / fgets 래핑 |
| 파일쓰기 / 파일줄쓰기 | fputs / fprintf 래핑 |
| 파일있음 | access() POSIX / _access() Windows |
| 파일크기 | stat.st_size |
| 파일목록 | opendir / readdir 래핑 |
| 폴더만들기 | mkdir 래핑 |
| 파일지우기 | remove 래핑 |
| 파일복사 | fread + fwrite 조합 |
| 파일이동 | rename 래핑 |
| 파일이름 / 파일확장자 | 경로 문자열 파싱 |
| 파일전체읽기 / 파일전체쓰기 | 편의 래퍼 함수 |

-----

### ⬜ 4단계 예정 — kcodegen.h / kcodegen.c

추가할 변환 케이스
```c
case NODE_CONSTITUTION:
    /* 헤더에 전역 assert 래퍼 삽입 */
    /* 모든 생성 함수 앞에 자동 적용 */

case NODE_LAW:
    /* 현재 번역 단위 내 함수 앞에 assert 삽입 */

case NODE_REGULATION:
    /* sval(객체명) 메서드 탐색 후 assert 삽입 */

case NODE_CONTRACT: /* 블록 방식 재정의 */
    /* 법령끝 도달까지 조항 수집 후 변환 */
    /* 기존 인라인 방식 코드 제거 */

case NODE_FILE_STMT:
    /* 파일 함수 → C stdio / POSIX 호출 변환 */
    /* Windows: _access, _mkdir 등 분기 처리 */
```

-----

### ⬜ 5단계 예정 — 문서화

- readme.md 전 단계 완료 처리
- kcode_raw_System.md 최종 확인

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

| 파일 | 버전 | 상태 |
|------|------|------|
| klexer.h / klexer.c | **v1.4.0** | ✅ 완료 |
| kparser.h / kparser.c | **v1.4.0** | ✅ 완료 |
| kgc.h / kgc.c | **v5.0.0** | ✅ GC_INSTANCE + gc_new_instance + gc_mark_value 동기화 완료 |
| kinterp.h | **v5.0.0** | ✅ 버전 동기화 완료 |
| kcodegen.h | **v5.0.0** | ✅ 버전 동기화 완료 |
| kinterp.h / kinterp.c | **v5.0.0** | ✅ 계약 계층 eval + 파일 내장함수 17종 완료 |
| kcodegen.h / kcodegen.c | **v5.1.0** | ✅ 법위반 래퍼 완전 구현 (kc__fn_impl + kc_fn 쌍) |
| kcode_raw_System.md | **v2.0.0** | ✅ 명세 개편 완료 |
| kinterp_main.c | v2.0.0 | — |
| kcodegen_llvm.h/.c | v3.5.0 | — |
| kcodegen_llvm_main.c | v3.5.0 | — |
| kserver.c | v3.5.0 | — |
| CMakeLists.txt | v3.5.0 | — |

-----

## 알려진 이슈 / 다음 단계

- ~~**v5.1** : 법위반(사후조건) 계층 kcodegen.c 완전 변환~~ ✅ 완료
- **v5.2** : 객체(class) / 예외 처리 LLVM 코드 생성
- **v5.2** : 가속기 블록 CUDA/OpenCL 실제 런타임 연동 (현재 OpenMP pragma 힌트 수준)
