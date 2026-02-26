# Kcode v19.0.0

> 한글 키워드 프로그래밍 언어
> 인터프리터 + C코드 생성 + LLVM IR 3중 백엔드

---

## 버전 히스토리

| 버전 | 내용 |
|------|------|
| v0.7.1 | 렉서 완료 |
| v0.8.0 | 파서 완료 |
| v0.9.0 | 인터프리터 완료 |
| v1.0.0 | C 코드 생성기 완료 |
| v1.1.0 | 스크립트 블록 추가 — Python / Java / JavaScript |
| v1.2.0 | 가속기 블록 도입 |
| v1.2.1 | Makefile 완전 재작성 — LLVM 연동 준비 |
| v2.0.0 | 글자 ↔ 문자 자료형 명칭 전면 교체 |
| v2.1.0 | LLVM IR 코드 생성기 신규 작성 (kcodegen_llvm.h / .c) |
| v3.0.0 | LLVM 코드 생성기 IDE 완전 호환 — 소스맵 + JSON + IDE 서버 모드 |
| v3.1.0 | 최적화 전/후 IR 동시 제공 |
| v3.1.1 | iOS .ipa 확장자 추가 (문서) |
| v3.2.0 | kcodegen_llvm_main.c — LLVM 드라이버 완성 |
| v3.3.0 | kserver.c — 로컬 HTTP 서버 완성 |
| v3.4.0 | 코드 검증 및 오류 수정 |
| v3.5.0 | CMake 빌드 시스템 추가 |
| v3.6.0 | AI 수학 내장 함수 7종 추가 |
| v3.7.1 | 수학 기초 함수 9종 추가 |
| v3.8.0 | 통계 함수 13종 + AI 활성함수 3종 + 호감도 함수 추가 |
| v3.9.0 | ★ GC 통합 — kgc.h/kgc.c 신규, 참조카운트 + 마크스윕 |
| v4.0.0 | ★ 법령/법위반 계약 시스템 — 사전/사후조건 + 6종 제재 + 복원지점 |
| v4.1.0 | ★ C 코드 생성기 계약 시스템 변환 |
| v4.1.1 | ★ 법령/법위반 계약 시스템 전 단계 문서화 최종 완료 |
| v4.2.0 | ★ 계약 시스템 전면 개편 — 헌법/법률/규정 계층 + 파일 내장 함수 |
| v5.0.0 | ★ 버그 수정 + 계약 v2.0 구현 완료 |
| v5.1.0 | ★ 법위반(사후조건) 함수 래퍼 완전 구현 |
| v5.2.0 | ★ 객체/예외/GPU/계약 LLVM IR 변환 완전 구현 |
| v6.0.0 | ★ 인터럽트 시스템 3종 전체 구현 — OS시그널/하드웨어간섭/행사 |
| v6.1.0 | ★ #포함/가짐 모듈 시스템 — 8종 확장자 지원 |
| v6.2.0 | ★ 객체(CLASS) vtable 완전 구현 + 문법 명세 전면 동기화 |
| v7.0.0 | ★ 글자 함수 시스템 전면 구현 — 5단계 20종 |
| v8.0.0 | ★ 글자 내장 함수 21종 완전 구현 |
| v8.1.0 | ★ 스크립트 블록 코드 생성 완전 구현 — Python/Java/JavaScript |
| v8.1.1 | readme.md 파일 구조 버전 표 정정 |
| v8.2.0 | ★ 가속기(GPU/NPU/CPU) CUDA C 직접 생성 구현 — 내장연산 5종 |
| v8.2.1 | ★ 프로젝트 전체 점검 — 헤더 버전 불일치 교정 |
| v9.0.0 | ★ 가속기 NPU 완전 구현 — Python onnxruntime / ONNX 그래프 5종 |
| v9.1.0 | ★ kcodegen_llvm.c — NODE_MEMBER / SWITCH / TRY / RAISE 구현 |
| v9.2.0 | ★ kcodegen_llvm.c — NODE_GOTO / LABEL / LAMBDA / DICT_LIT 구현 |
| v9.3.0 | ★ kcodegen_llvm.c — #포함/가짐 LLVM IR 구현 |
| v9.4.0 | ★ kcodegen_llvm.c — 계약 시스템 6종 LLVM IR 구현 |
| v9.5.0 | ★ kcodegen_llvm.c — 인터럽트 시스템 6종 LLVM IR 구현 |
| v9.6.0 | ★ kcodegen.c — 글자 함수 21종 gen_builtin_call() C코드 생성 추가 |
| v9.7.0 | ★ kcodegen_llvm.c — gen_call() 글자 함수 21종 LLVM IR |
| v9.8.0 | ★ kcodegen_llvm.c — gen_call() 파일 17종 + 형변환 3종 LLVM IR |
| v9.9.0 | ★ kcodegen_llvm.c — gen_call() 수학/AI 내장 함수 LLVM IR |
| v10.0.0 | ★ kcodegen_llvm.c — gen_call() 미구현 내장 함수 20종 완전 구현 — 출력없이/입력/추가/통계11종/배열정렬·뒤집기/시그모이드·렐루·쌍곡탄젠트/호감도 |
| v10.1.0 | ★ kcodegen.c — gen_builtin_call() 누락 내장 함수 36종 추가 — 글자/추가/통계13종/활성함수3종/호감도/파일17종 (kcodegen.c + kcodegen.h 2파일) |
| v11.0.0 | ★ 텐서(Tensor) 자료형 전면 구현 — kc_tensor.h/c 신규 + klexer/kparser/kinterp/kcodegen/kcodegen_llvm 6개 파일 연동 |
| v14.0.1 | ★ 오류 수정 — MCP 토큰 8종(klexer) / MCP 노드 4종(kparser) 누락 보완 + kc_tensor 헬퍼 3종 추가 (autograd 연동) |
| v15.0.0 | ★ 자동미분(Autograd) 완전 연동 — kinterp/kcodegen/kcodegen_llvm 3파일에 역전파·기울기초기화·미분추적 + 10종 미분연산 등록 |
| v16.0.0 | ★ 산업/임베디드 확장 — 타이머 블록·ROS2 노드 블록 + GPIO/I2C/SPI/UART/Modbus/CAN/MQTT/ROS2 26종 토큰 + 22종 내장함수 |
| v16.5.0 | ★ WebAssembly 빌드 — kserver 없이 브라우저에서 Kcode 직접 실행 (KVM WASM 경량 + 풀엔진 WASM) |
| v16.5.1 | ★ kc_wasm_api.c 버그 수정 — ast_free→node_free, strdup POSIX 선언 추가 / gcc 문법 검증 에러 0 |
| v17.0.0 | ★ 안전 규격 — IEC 61508 / ISO 26262 기능안전 계약 시스템 — 워치독/결함허용 블록 + 페일세이프/긴급정지/경보발령 내장함수 7종 토큰 |
| v18.0.0 | ★ 온디바이스 AI — AI모델/TinyML/연합학습 블록 + AI불러오기/AI추론/AI학습단계/AI저장 내장함수 토큰 10종 |
| v18.1.0 | ★ 내장함수 31종 추가 — 수학(제곱/라디안/각도/난수/난정수) + 역삼각(4종) + 글자(좌문자/우문자/채우기/코드/붙여씀) + 배열고급(7종) + 시간(4종) + 시스템(4종) + JSON(2종) |
| v18.5.0 | ★ 온톨로지 시스템 1회차 — kc_ontology.h/c 신규: 지식 그래프 자료구조 + CRUD + 전방향체이닝 추론기 + JSON-LD/Turtle/JSON 직렬화 + 학습 거버넌스 |
| v19.0.0 | ★ 온톨로지 3회차 — Turtle/OWL 임포트 파서 + REST+WS 온톨로지 서버 (kc_ont_import 확장 + kc_ont_server.h/c 신규) |
| v18.5.0(4) | ★ 온톨로지 4회차 — kc_ont_remote.h/c 신규(외부서버 클라이언트) + klexer 토큰 10종 + kparser AST 노드 6종 |

---

## ★★★ 현재 상태 (v18.5.0) ★★★
### ← 재시작 시 이 섹션부터 읽을 것

| 파일 | 버전 | 상태 |
|------|------|------|
| klexer.h / klexer.c | **v16.0.0** | ✅ 산업/임베디드 토큰 26종 추가 |
| kparser.h / kparser.c | **v16.0.0** | ✅ NODE_TIMER_BLOCK / NODE_ROS2_BLOCK 추가 |
| kinterp.c / kinterp.h | **v16.0.0 / v15.0.0** | ✅ 산업 내장함수 22종 등록 + 노드 평가 |
| kcodegen.c / kcodegen.h | **v16.0.0 / v15.0.0** | ✅ 산업 C코드 생성 22종 + TIMER/ROS2 블록 |
| kcodegen_llvm.c / kcodegen_llvm.h | **v16.0.0 / v15.0.0** | ✅ 산업 LLVM IR 22종 + TIMER/ROS2 블록 |
| kc_autograd.h / kc_autograd.c | **v13.0.0** | ✅ 역전파 엔진 10종 완료 |
| kc_tensor.h / kc_tensor.c | **v14.0.1** | ✅ + autograd 헬퍼 3종 |
| klexer.h / klexer.c | (이전) | ✅ MCP 토큰 8종 포함 |
| kgc.c / kgc.h | **v13.0.0** | ✅ |
| kc_mcp.c / kc_mcp.h | **v14.0.0** | ✅ |
| **kc_bytecode.h / kc_bytecode.c** | **v16.0.0** | ✅ 바이트코드 명령어 셋 |
| **kc_bcgen.h / kc_bcgen.c** | **v15.0.0 / v16.0.0** | ✅ AST → 바이트코드 컴파일러 |
| **kc_vm.h / kc_vm.c** | **v16.0.2** | ✅ KVM 스택 가상머신 |
| **kbc_main.c** | **v15.0.0** | ✅ 바이트코드 드라이버 |
| **kc_wasm_api.h / kc_wasm_api.c** | **v16.5.1** | ✅ WebAssembly JS 브릿지 5종 |
| **kcode-wasm.js** | **v16.5.0** | ✅ 웹 IDE WASM 로더 + kserver 폴백 |
| **klexer.h / klexer.c** | **v18.0.0** | ✅ v16/v17/v18 토큰 전체 통합 (산업+안전+AI 토큰 27종) |
| **kparser.h / kparser.c** | **v18.0.0** | ✅ NODE_AI_MODEL_BLOCK / NODE_TINYML_BLOCK / NODE_FEDERATED_BLOCK 추가 |
| **kinterp.c** | **v18.1.0** | ✅ 내장함수 31종 추가 (수학/역삼각/글자/배열/시간/시스템/JSON) |
| **kcodegen.c** | **v18.1.0** | ✅ 신규 31종 C코드 생성 추가 |
| **kcodegen_llvm.c** | **v18.1.0** | ✅ 신규 31종 LLVM IR 추가 |
| **kc_ont_remote.h** | **v18.5.0** | ✅ 신규 — 외부 온톨로지 서버 클라이언트 헤더 |
| **kc_ont_remote.c** | **v18.5.0** | ✅ 신규 — HTTP REST + WebSocket 클라이언트 |
| **klexer.h / klexer.c** | **v18.5.0** | ✅ 온톨로지 토큰 10종 + TOK_NAMES 등록 |
| **kparser.h / kparser.c** | **v18.5.0** | ✅ 온톨로지 AST 노드 6종 + 파싱 함수 |

---

## 파일 구조 현황

### 핵심 엔진 파일

| 파일 | 버전 | 상태 | 주요 내용 |
|------|------|------|---------|
| klexer.h / klexer.c | v14.0.1 | ✅ | MCP 토큰 8종 + 인터럽트 + 텐서 키워드 포함 전체 토큰 |
| kparser.h / kparser.c | v14.0.1 / v14.0.0 | ✅ | MCP 노드 4종 + NODE_TENSOR_LIT 포함 전체 AST 노드 |
| kgc.h / kgc.c | v13.0.0 | ✅ | 참조카운트 + 마크스윕 GC |
| kinterp.h / kinterp.c | v11.0.0 | ✅ | 텐서 내장함수 12종 포함 전체 내장 함수 |
| kcodegen.h / kcodegen.c | v11.0.0 | ✅ | 텐서 C코드 생성 포함 전체 내장 함수 생성 |
| kcodegen_llvm.h / kcodegen_llvm.c | v10.0.0 | ✅ | 텐서 LLVM IR 포함 전체 완료 |
| kc_tensor.h / kc_tensor.c | v14.0.1 | ✅ | N차원 텐서 — 생성4+변형3+연산4+행렬곱+축연산2 + autograd 헬퍼 3종 |
| kc_mcp.h / kc_mcp.c | v14.0.0 | ✅ | MCP 서버 런타임 (도구/자원/프롬프트) |
| kc_autograd.h / kc_autograd.c | v13.0.0 | ⚠️ | 역전파 엔진 구현 — kc_tensor 연동 완성 필요 |
| **kc_bytecode.h / kc_bytecode.c** | **v16.0.0** | ✅ | **바이트코드 명령어 셋 (KVM용)** |
| **kc_bcgen.h / kc_bcgen.c** | **v15.0.0 / v16.0.0** | ✅ | **AST → 바이트코드 컴파일러** |
| **kc_vm.h / kc_vm.c** | **v16.0.2** | ✅ | **KVM 스택 가상머신** |

### 드라이버 및 빌드 파일

| 파일 | 버전 | 상태 | 설명 |
|------|------|------|------|
| kinterp_main.c | v13.0.0 | ✅ | 인터프리터 진입점 |
| kcodegen_main.c | v13.0.0 | ✅ | C 코드 생성기 드라이버 |
| kcodegen_llvm_main.c | v13.0.0 | ✅ | LLVM 드라이버 |
| **kbc_main.c** | **v15.0.0** | ✅ | **바이트코드 드라이버 진입점** |
| kserver.c | v13.0.0 | ✅ | 로컬 HTTP 서버 |
| CMakeLists.txt | v11.0.0 | ✅ | CMake 빌드 시스템 |
| Makefile | v11.0.0 | ✅ | 빌드 규칙 |

### 문서 파일

| 파일 | 버전 | 상태 | 설명 |
|------|------|------|------|
| readme.md | v10.0.0 | ✅ | 이 파일 |
| readme_skil.md | v6.2.0 | ✅ | 언어 설계 명세 / 키워드 전체 목록 |
| Kcode_grammar.md | v8.2.0 | ✅ | 문법 레퍼런스 |
| kcode_raw_System.md | v2.0.0 | ✅ | 계약 시스템 5계층 명세 |
| INSTALL_AND_DLL_GUIDE.md | v3.4.0 | 📝 업데이트 필요 | 설치 및 DLL 가이드 |
| KCODE_IDE_UI_제작배경_정리문서.docx | — | 📝 참고용 | IDE UI 설계 배경 |

---

## 구현 완료 기능 목록

| 분류 | 기능 | 상태 |
|------|------|------|
| 기본 | 렉서 / 파서 / 인터프리터 | ✅ |
| 기본 | C 코드 생성 / LLVM IR 생성 | ✅ |
| 자료형 | 정수/실수/글자/문자/논리/없음/배열/사전/행렬 | ✅ |
| 제어문 | 조건/반복/분기(선택)/강제이동(goto) | ✅ |
| 함수 | 함수/정의/람다/가변인수/다중반환 | ✅ |
| 객체 | 객체(CLASS) vtable + 상속 + LLVM IR | ✅ |
| 예외 | 시도/실패시/항상/오류 | ✅ |
| 모듈 | #포함/가짐 — 8종 확장자 지원 | ✅ |
| GC | 참조카운트 + 마크스윕 | ✅ |
| 계약 | 헌법/법률/규정/법령/법위반 5계층 | ✅ |
| 인터럽트 | OS시그널 / 하드웨어간섭 / 행사(이벤트) | ✅ |
| 스크립트 | Python / Java / JavaScript 블록 | ✅ |
| 가속기 | GPU(CUDA) / NPU(ONNX) / CPU(OpenMP) 블록 | ✅ |
| 내장 수학 | 기초 9종 + AI 7종 + 통계 13종 + 활성함수 3종 + 호감도 | ✅ |
| 내장 글자 | 21종 (자르기 ~ 포맷) | ✅ |
| 내장 파일 | 17종 (파일열기 ~ 파일전체쓰기) | ✅ |
| 내장 I/O | 출력 / 출력없이 / 입력 | ✅ |
| **텐서** | N차원 텐서 자료형 — 생성/변형/연산/행렬곱/축연산 | ✅ v11.0.0 완료 |
| **자동미분** | 테이프 방식 역전파 엔진 — 10종 연산 + 인터프리터/C생성/LLVM 연동 | ✅ v15.0.0 완료 |
| **산업/임베디드** | 타이머 블록 + GPIO/I2C/SPI/UART/Modbus/CAN/MQTT/ROS2 26종 | ✅ v16.0.0 완료 |

---

## 작업 로드맵

### ✅ 【v10.1.0】 kcodegen.c 누락 내장 함수 점검 — 완료

| 단계 | 작업 내용 | 상태 |
|------|----------|------|
| 1 | kinterp.c 등록 함수 전체 목록 vs kcodegen.c 구현 목록 대조표 작성 | ✅ |
| 2 | 누락 함수 C코드 생성 추가 36종 (통계 13종 / 활성함수 3종 / 호감도 / 추가 / 글자형변환 / 파일 17종) | ✅ |
| 3 | gcc -fsyntax-only 문법 검증 — 에러 0 | ✅ |
| 4 | readme.md 버전 히스토리 업데이트 | ✅ |

---

### ✅ 【v15.0.0】 자동미분(Autograd) 완전 연동 — 완료 → kinterp / kcodegen / kcodegen_llvm 3단계 연동

| 단계 | 작업 내용 | 상태 |
|------|----------|------|
| 1 | 기존 kc_autograd.h/c 상태 점검 — 역전파 엔진 10종 확인 | ✅ |
| 2 | kinterp.c — autograd 내장함수 13종 구현 및 테이블 등록 | ✅ |
| 3 | kcodegen.c — gen_builtin_call() autograd 13종 C코드 생성 | ✅ |
| 4 | kcodegen_llvm.c — gen_call() autograd 13종 LLVM IR 외부 함수 선언 | ✅ |
| 5 | gcc -fsyntax-only 검증 — kinterp.c / kcodegen.c / kc_autograd.c 에러 0 | ✅ |
| 6 | LLVM IR 블록 독립 구문 검증 (스텁 타입 방식) — 에러 0 | ✅ |
| 7 | readme.md 업데이트 | ✅ |

#### 등록된 autograd 내장함수 (한글 키워드 → C 함수)

| 한글 키워드 | C 함수 | 설명 |
|------------|--------|------|
| `역전파(t)` | `kc_autograd_backward` | 루트 텐서에서 역전파 실행 |
| `기울기초기화(t)` | `kc_autograd_zero_grad` | grad 버퍼 0 초기화 |
| `미분추적(t)` | `kc_tensor_ensure_grad` | requires_grad=1 설정 |
| `미분더하기(a,b)` | `kc_ag_add` | 덧셈 + GradFn |
| `미분곱(a,b)` | `kc_ag_mul` | 원소별 곱 + GradFn |
| `미분행렬곱(a,b)` | `kc_ag_matmul` | 행렬곱 + GradFn |
| `미분렐루(a)` | `kc_ag_relu` | ReLU + GradFn |
| `미분시그모이드(a)` | `kc_ag_sigmoid` | σ(a) + GradFn |
| `미분쌍곡탄젠트(a)` | `kc_ag_tanh_op` | tanh(a) + GradFn |
| `미분로그(a)` | `kc_ag_log` | log(a) + GradFn |
| `미분합산(a)` | `kc_ag_sum` | 전원소 합 → 스칼라 + GradFn |
| `미분평균(a)` | `kc_ag_mean` | 전원소 평균 → 스칼라 + GradFn |
| `미분제곱(a)` | `kc_ag_pow2` | a² + GradFn |

#### 사용 예시

```han
텐서 x = 텐서([2.0], [1])
텐서 w = 텐서([3.0], [1])
미분추적(x)
미분추적(w)

텐서 y = 미분더하기(미분곱(x, x), 미분곱(w, 텐서([2.0],[1])))
역전파(y)

출력(x.기울기)   // dy/dx = 2x = 4.0
출력(w.기울기)   // dy/dw = 2.0

기울기초기화(x)
기울기초기화(w)
```

> **버전 정책**: 3개 파일 수정 → **v15.0.0** (앞자리 올림)

---

### ✅ 【v14.0.1】 빌드 오류 수정 + readme 현행화 — 완료

| 단계 | 작업 내용 | 상태 |
|------|----------|------|
| 1 | 전체 파일 오류/누락 점검 (gcc -fsyntax-only) | ✅ |
| 2 | klexer.h — MCP 토큰 8종 enum 추가 | ✅ |
| 3 | klexer.c — MCP 키워드 테이블 + TOK_NAMES 배열 추가 | ✅ |
| 4 | kparser.h — MCP 노드 4종(SERVER/TOOL/RESOURCE/PROMPT) 추가 | ✅ |
| 5 | kc_tensor.h/c — autograd 연동 헬퍼 3종(sum/mean/from_data) 추가 | ✅ |
| 6 | gcc -fsyntax-only 재검증 — 에러 0 | ✅ |
| 7 | readme.md — 버전 현행화 + 미등재 파일(kc_bytecode/kc_bcgen/kc_vm/kbc_main) 등재 | ✅ |

---

### ✅ 【v11.0.0】 텐서(Tensor) 자료형 구현 — 완료

> N차원 텐서 자료형을 Kcode에 추가한다.
> 기존 `행렬` 자료형을 확장하며, GC 관리 대상으로 통합한다.

#### 문법

```han
텐서 A = 텐서([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], [2, 3])  // 2×3 생성
텐서 B = 영텐서([3, 4, 5])                                // 영텐서
텐서 C = 일텐서([2, 2])                                   // 일텐서
텐서 D = 무작위텐서([128, 256])                           // 랜덤

A.형태      // [2, 3]
A.차원      // 2
A.원소수    // 6

A.모양바꾸기([6])   // reshape
A.전치()            // transpose
A.펼치기()          // flatten

A + B   A - B   A * B   A / B   // 원소별 연산 (브로드캐스팅 자동)
A @ B                            // 행렬곱
합산(A, 축=0)                    // axis 합산
평균(A, 축=1)                    // axis 평균
```

#### 내부 C 구조체

```c
typedef struct KcTensor {
    double   *data;          /* 데이터 버퍼 (1D 연속 배열) */
    int64_t  *shape;         /* 형태 [2, 3, 4] */
    int64_t  *strides;       /* 스트라이드 (브로드캐스팅용) */
    int       ndim;          /* 차원 수 */
    int64_t   numel;         /* 전체 원소 수 */
    int       requires_grad; /* 자동미분 추적 여부 */
    double   *grad;          /* 기울기 버퍼 (v12.0.0 연동) */
    struct GradFn *grad_fn;  /* 역전파 함수 포인터 (v12.0.0 연동) */
    int       ref_count;     /* GC 참조카운트 */
} KcTensor;
```

#### 신규 / 수정 파일

| 구분 | 파일 | 작업 내용 |
|------|------|----------|
| 신규 | kc_tensor.h / kc_tensor.c | KcTensor 구조체 + 생성/변형/연산 + 브로드캐스팅 |
| 수정 | klexer.h / klexer.c | `텐서` / `영텐서` / `일텐서` / `무작위텐서` 토큰 추가 |
| 수정 | kparser.h / kparser.c | NODE_TENSOR_LIT + 속성 접근 파싱 |
| 수정 | kinterp.c | 텐서 내장 함수 12종 등록 |
| 수정 | kcodegen.c | 텐서 C코드 생성 |
| 수정 | kcodegen_llvm.c | 텐서 LLVM IR |

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | kc_tensor.h/c 신규 작성 — 구조체 + 생성 4종 + 변형 3종 + 연산 5종 | ✅ |
| 2 | klexer — 텐서 관련 토큰 4종 추가 | ✅ |
| 3 | kparser — NODE_TENSOR_LIT + 속성 접근 파싱 | ✅ |
| 4 | kinterp.c — 텐서 내장 함수 12종 등록 | ✅ |
| 5 | kcodegen.c — 텐서 C코드 생성 | ✅ |
| 6 | kcodegen_llvm.c — 텐서 LLVM IR | ✅ |
| 7 | gcc -fsyntax-only 문법 검증 — 에러 0 | ✅ |
| 8 | readme.md 업데이트 | ✅ |

> **버전 정책**: 6개 파일 수정 → **v11.0.0** (앞자리 올림)

---

### 【v12.0.0】 자동미분(Autograd) 엔진 구현

> 테이프 방식(PyTorch 스타일) 동적 연산 그래프 + 역전파 엔진.
> v11.0.0 텐서 구현이 선행되어야 한다.

#### 문법

```han
텐서 x = 텐서([2.0], [1], 미분추적=참)
텐서 w = 텐서([3.0], [1], 미분추적=참)

텐서 y = x * x + w * 2.0   // 계산 그래프 자동 기록

역전파(y)                   // 연쇄법칙으로 기울기 자동 계산

출력(x.기울기)   // dy/dx = 2x = 4.0
출력(w.기울기)   // dy/dw = 2.0

기울기초기화(x)  // 다음 역전파 전 초기화
기울기초기화(w)
```

#### 내부 C 구조체

```c
typedef struct GradFn {
    void         (*backward)(struct GradFn*, double *grad_out, int64_t numel);
    KcTensor     *saved[4];      /* 역전파에 필요한 저장 텐서 */
    int           n_saved;
    struct GradFn *next_fns[4]; /* 이전 노드 연결 (계산 그래프) */
    int           n_next;
} GradFn;
```

#### 지원 역전파 연산 10종

| 연산 | 순전파 | 역전파 |
|------|--------|--------|
| 덧셈 | z = a + b | da=1, db=1 |
| 곱셈 | z = a * b | da=b, db=a |
| 행렬곱 | Z = A @ B | dA=G@Bᵀ, dB=Aᵀ@G |
| 렐루 | z = relu(a) | da = (a>0) ? 1 : 0 |
| 시그모이드 | z = σ(a) | da = z*(1-z) |
| 쌍곡탄젠트 | z = tanh(a) | da = 1-z² |
| 로그 | z = log(a) | da = 1/a |
| 합산 | z = sum(a) | da = 1 (브로드캐스트) |
| 평균 | z = mean(a) | da = 1/n |
| 제곱 | z = a² | da = 2a |

#### 신규 / 수정 파일

| 구분 | 파일 | 작업 내용 |
|------|------|----------|
| 신규 | kc_autograd.h / kc_autograd.c | GradFn 구조체 + 역전파 함수 10종 |
| 수정 | kc_tensor.c | 연산 시 GradFn 자동 생성 및 연결 |
| 수정 | kinterp.c | `역전파()` / `기울기초기화()` 내장 함수 등록 |
| 수정 | kcodegen.c | autograd C코드 생성 |
| 수정 | kcodegen_llvm.c | autograd LLVM IR |

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | kc_autograd.h/c 신규 작성 — GradFn 구조체 + 역전파 함수 10종 | ⬜ |
| 2 | kc_tensor.c — 연산 시 GradFn 자동 생성/연결 | ⬜ |
| 3 | kinterp.c — `역전파()` / `기울기초기화()` 등록 | ⬜ |
| 4 | kcodegen.c — autograd C코드 생성 | ⬜ |
| 5 | kcodegen_llvm.c — autograd LLVM IR | ⬜ |
| 6 | 통합 테스트 — MSE 손실 역전파 예제 실행 확인 | ⬜ |
| 7 | gcc -fsyntax-only 문법 검증 | ⬜ |
| 8 | readme.md + Kcode_grammar.md 업데이트 | ⬜ |

> **버전 정책**: 5개 파일 이상 수정 → **v12.0.0** (앞자리 올림)

---

## 전체 작업 순서 요약

| 순서 | 버전 | 작업 | 상태 |
|------|------|------|------|
| ✅ 완료 | **v10.1.0** | kcodegen.c 누락 내장 함수 36종 추가 | ✅ 완료 |
| ✅ 완료 | **v11.0.0** | 텐서 자료형 — kc_tensor.h/c 신규 + 6개 파일 연동 | ✅ 완료 |
| ✅ 완료 | **v15.0.0** | 자동미분 완전 연동 — kinterp/kcodegen/kcodegen_llvm + MCP/바이트코드 VM | ✅ 완료 |
| ✅ 완료 | **v16.0.0** | 산업/임베디드 확장 — 타이머/ROS2 블록 + 26종 토큰 + 22종 내장함수 | ✅ 완료 |
| ✅ 완료 | **v17.0.0** | 안전 규격 — IEC 61508 / ISO 26262 워치독/결함허용/안전함수 | ✅ 완료 |
| ✅ 완료 | **v18.0.0** | 온디바이스 AI — AI모델/TinyML/연합학습 블록 + 추론/학습/저장 내장함수 | ✅ 완료 |
| ✅ 완료 | **v18.1.0** | 내장함수 31종 추가 — 수학/역삼각/글자/배열/시간/시스템/JSON | ✅ 완료 |
| 🔄 진행 | **v18.5.0** | 온톨로지 통합 시스템 — K엔진 3모드 + 학습 거버넌스 | 🔄 4회차 완료 |
| ▶ 다음 | **v19.0.0** | PC 서버화 — HTTP/WebSocket 서버 블록 + REST API 자동생성 | ⬜ 대기 |
| 예정 | **v20.0.0** | 패키지 생태계 — 모듈 레지스트리 + FFI + SDK 공개 | ⬜ 대기 |

---

### ✅ 【v16.0.0】 산업/임베디드 확장 — 완료

> 실시간 타이머 제어 + 산업 통신 프로토콜 + ROS2 로봇 연동

#### 구현된 산업/임베디드 기능

| 분류 | 키워드 | 설명 |
|------|--------|------|
| 타이머 블록 | `타이머 100ms:` ... `타이머끝` | 실시간 주기 제어 루프 |
| GPIO | `GPIO쓰기(핀, 값)` / `GPIO읽기(핀)` | 디지털 핀 제어 |
| I2C | `I2C연결(주소)` / `I2C읽기(주소)` / `I2C쓰기(주소, 데이터)` | I2C 버스 통신 |
| SPI | `SPI전송(데이터)` / `SPI읽기()` | SPI 버스 통신 |
| UART | `UART설정(보드레이트)` / `UART전송(데이터)` / `UART읽기()` | 시리얼 통신 |
| Modbus | `Modbus연결(호스트, 포트)` / `Modbus읽기(레지스터)` / `Modbus쓰기(레지스터, 값)` | PLC 연동 |
| CAN | `CAN필터(ID, 마스크)` / `CAN전송(ID, 데이터)` / `CAN읽기()` | 차량/산업 버스 |
| MQTT | `MQTT연결(브로커, 포트)` / `MQTT발행(토픽, 메시지)` / `MQTT구독(토픽)` / `MQTT연결끊기()` | IoT 통신 |
| ROS2 | `ROS2노드 "이름":` ... `ROS2끝` / `ROS2발행(토픽, 메시지)` / `ROS2구독(토픽)` | 로봇 연동 |

#### 사용 예시

```han
// 100ms 타이머 제어 루프
타이머 100ms:
    정수 온도 = I2C읽기(0x48)
    만약 온도 > 80:
        GPIO쓰기(13, 0)
        MQTT발행("경보/온도", 글자(온도))
타이머끝

// ROS2 노드 블록
ROS2노드 "센서처리":
    정수 값 = I2C읽기(0x48)
    ROS2발행("센서/온도", 글자(값))
ROS2끝
```

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | klexer.h/c — 산업/임베디드 토큰 26종 추가 (타이머/ROS2/GPIO/I2C/SPI/UART/Modbus/CAN/MQTT) | ✅ |
| 2 | kparser.h/c — NODE_TIMER_BLOCK / NODE_ROS2_BLOCK + parse_timer_block / parse_ros2_block + is_callable_kw 추가 | ✅ |
| 3 | kinterp.c — 산업 내장함수 22종 구현 + register_builtins 등록 + NODE_TIMER/ROS2 평가 | ✅ |
| 4 | kcodegen.c — gen_builtin_call() 22종 + NODE_TIMER_BLOCK/NODE_ROS2_BLOCK C코드 생성 | ✅ |
| 5 | kcodegen_llvm.c — gen_call() 22종 LLVM IR + NODE_TIMER_BLOCK/NODE_ROS2_BLOCK LLVM IR | ✅ |
| 6 | gcc -fsyntax-only 검증 — klexer / kparser / kinterp / kcodegen / kcodegen_llvm 에러 0 | ✅ |
| 7 | readme.md 버전 v16.0.0 업데이트 | ✅ |

> **버전 정책**: 5개 이상 파일 수정 → **v16.0.0** (앞자리 올림)

---

---

### 【v16.5.0】 WebAssembly 빌드 — 브라우저 완전 독립 실행

> kserver 로컬 설치 없이 웹 IDE에서 Kcode를 직접 실행한다.
> `kcode.wasm` 을 통해 온라인 데모 / 교육 / 앱 개발자 체험 환경 제공.

#### 핵심 전략 — 2단계

| 단계 | 내용 | 대상 버전 | 빌드 결과 |
|------|------|----------|---------|
| 1단계 | KVM WASM (경량) — klexer + kparser + kc_bcgen + kc_vm | v16.5.0 | `kcode_vm.wasm` (~200KB) |
| 2단계 | 풀 엔진 WASM — kinterp + kgc + kc_tensor + kc_autograd + kc_mcp + kc_ontology | v17+ | `kcode_full.wasm` (~1~2MB) |

#### 웹 IDE 실행 모드

| 모드 | 조건 | 가능한 기능 |
|------|------|-----------|
| **WASM 모드** | kserver 없음 (기본) | 실행 / 렉싱 / 파싱 / 바이트코드 VM |
| **kserver 모드** | kserver 로컬 실행 중 | WASM 전체 + LLVM IR + C코드 생성 + 최적화 |
| **원격 서버 모드** | v19 PC 서버화 이후 | 전체 기능 (원격 접속) |

#### JS ↔ WASM API (kc_wasm_api.c) — 노출 함수 5종

| 함수 | 반환 | 설명 |
|------|------|------|
| `kc_wasm_run(source)` | JSON | 소스코드 실행 |
| `kc_wasm_compile(source)` | JSON | 바이트코드 컴파일 |
| `kc_wasm_exec_bc(bytecode_b64)` | JSON | 바이트코드 실행 |
| `kc_wasm_lex(source)` | JSON | 토큰 목록 (IDE 하이라이팅용) |
| `kc_wasm_parse(source)` | JSON | AST 반환 (IDE 구문 분석용) |

#### 신규 / 수정 파일

| 구분 | 파일 | 내용 |
|------|------|------|
| 신규 | `kc_wasm_api.c` | JS ↔ WASM 브릿지 함수 5종 |
| 신규 | `kc_wasm_api.h` | 브릿지 헤더 |
| 신규 | `web/kcode-wasm.js` | 웹 IDE WASM 로더 + kserver 폴백 로직 |
| 수정 | `CMakeLists.txt` | WASM 빌드 타겟 추가 (`BUILD_WASM` 옵션) |
| 수정 | `kc_vm.c` | `KCODE_WASM_BUILD` 조건부 stdout 캡처 |
| 수정 | `kinterp.c` | `KCODE_WASM_BUILD` 조건부 stdout 캡처 |

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | `kc_wasm_api.c/h` 작성 — JS 브릿지 5종 함수 | ✅ |
| 2 | `kc_vm.c / kinterp.c` — `KCODE_WASM_BUILD` stdout 캡처 분기 | ✅ |
| 3 | `CMakeLists.txt` — WASM 빌드 타겟 추가 | ✅ |
| 4 | Emscripten 빌드 검증 — `kcode_vm.wasm` 생성 확인 | ⬜ (Emscripten 환경 필요) |
| 5 | `web/kcode-wasm.js` — 로더 + kserver 폴백 로직 | ✅ |
| 6 | 웹 IDE 통합 테스트 — kserver 없이 실행 확인 | ⬜ (브라우저 환경 필요) |
| 7 | readme.md / ROADMAP.md 업데이트 | ✅ |

> **버전 정책**: 4개 이상 파일 수정 → **v16.5.0** (앞자리 올림)
> 설계 문서: `kcode_wasm_design.md`

---

### 【v18.5.0】 온톨로지 통합 시스템 — K엔진 3모드

> Kcode에 지식 그래프(온톨로지) 시스템을 통합한다.
> K엔진은 하나다 — 어디서 쓰느냐에 따라 3가지 모드로 동작.

#### K엔진 3가지 사용 모드

| 모드 | 이름 | 설명 |
|------|------|------|
| 모드 1 | 내장 모드 | K엔진 + K서버 — Kcode가 서버까지 직접 운영 (`온톨로지서버 포트=7070`) |
| 모드 2 | 대여 모드 | K엔진 SDK — `libkc_ontology.so` 직접 링크 (Python/Node.js/Java/C) |
| 모드 3 | 접속 모드 | K엔진 → 외부 서버 — Jena / GraphDB / AWS Neptune 연결 |

#### K엔진 핵심 컴포넌트

| 컴포넌트 | 역할 |
|---------|------|
| `KcOntology` | 지식 그래프 저장소 |
| `KcReasoner` | 전방향 체이닝 추론기 |
| `KcOntQuery` | 한글 질의 + SPARQL 1.1 실행기 |
| `KcOntSerial` | JSON-LD / Turtle / OWL 직렬화 |
| `KcOntImport` | 외부 포맷 임포트 (OWL/RDF/TTL/JSON-LD/CSV) |
| `KcOntRemote` | 외부 서버 클라이언트 |

#### 모드 1 K서버 REST API (주요)

| 메서드 | 경로 | 설명 |
|--------|------|------|
| GET/POST/PUT/DELETE | `/개념` | 개념 CRUD |
| GET/POST/PUT/PATCH/DELETE | `/인스턴스` | 인스턴스 CRUD |
| POST | `/질의` | 한글 SPARQL 질의 |
| POST | `/sparql` | 표준 SPARQL 1.1 |
| POST | `/추론` | 규칙 추론 실행 |
| POST/GET | `/가져오기` `/내보내기/{형식}` | OWL·JSON-LD·Turtle 임포트/익스포트 |
| WS | `/구독` `/구독/{개념}` | 실시간 변경 스트림 |

#### 학습 거버넌스 — 민감정보 보호 (3단계 강제)

| 단계 | 내용 |
|------|------|
| 1단계 선언 | 속성에 `민감=참` 태깅 — `이유="개인식별정보"` 등 명시 |
| 2단계 차단 | `kc_learn_filter()` — 민감 속성 자동 제외 + 익명화 |
| 3단계 강제 | 헌법 계약 시스템 — 위반 시 학습 즉시 중단 + 감사 로그 |

> 기본 민감 키워드(이름/이메일/주민번호/신용카드 등) 자동 감지 내장.

#### 신규 파일 (v18.5.0)

| 파일 | 내용 |
|------|------|
| `kc_ontology.h / kc_ontology.c` | 그래프 자료구조 + CRUD + 학습 거버넌스 API |
| `kc_ont_query.c` | 한글 질의 + SPARQL 실행기 |
| `kc_ont_import.c` | JSON-LD / CSV / Turtle / OWL 임포트 |
| `kc_ont_server.c` | REST + SPARQL + WebSocket 서버 (모드 1) |
| `kc_ont_remote.h / kc_ont_remote.c` | 외부 서버 클라이언트 (모드 3) |

#### 구현 단계

| 단계 | 내용 | 모드 | 상태 |
|------|------|------|------|
| 1 | `kc_ontology.h/c` — 그래프 자료구조 + CRUD | 전체 | ⬜ |
| 2 | 추론기 — 전방향 체이닝 | 전체 | ⬜ |
| 3 | `kc_ont_query.c` — 한글 질의 + SPARQL 실행기 | 전체 | ⬜ |
| 4 | `kc_ont_import.c` — JSON-LD / CSV 우선 | 1, 2 | ⬜ |
| 5 | `kc_ont_import.c` — Turtle / OWL 추가 | 1, 2 | ⬜ |
| 6 | `kc_ont_server.c` — REST + SPARQL + WS | 1 | ⬜ |
| 7 | `kc_ont_remote.h/c` — 외부 서버 클라이언트 | 3 | ⬜ |
| 8 | `klexer/kparser` — 온톨로지 토큰/노드 추가 | 전체 | ⬜ |
| 9 | `kinterp.c` — 3모드 블록 실행 | 전체 | ⬜ |
| 10 | MCP 자동 노출 (모드 1 + 3) | 1, 3 | ⬜ |
| 11 | `CMakeLists.txt` — `libkc_ontology.so` 빌드 | 2 | ⬜ |
| 12 | Python / Node.js SDK | 2 | ⬜ |
| 13 | `kcodegen.c / kcodegen_llvm.c` 코드 생성 | 전체 | ⬜ |
| 14 | 통합 테스트 (3모드 + 조합) | — | ⬜ |

> **버전 정책**: 5개 이상 파일 수정 → **v18.5.0** (앞자리 올림)
> 설계 문서: `kcode_ontology_design.md`

---

## 전체 로드맵 요약

```
v16.0.0  산업/임베디드       ✅ 완료 (GPIO/I2C/MQTT/ROS2)
v16.5.1  WebAssembly        ✅ 완료 (kc_wasm_api + CMake WASM 타겟 + 버그 수정)
v17.0.0  안전 규격           ✅ 완료 (워치독/결함허용/페일세이프/긴급정지/경보발령)
v18.0.0  온디바이스 AI       ⬜ 예정 (ONNX + TinyML)
v18.0.0  온디바이스 AI       ⬜ 예정 (ONNX + TinyML)
v18.5.0  온톨로지 시스템     ⬜ 예정 (K엔진 3모드 + 학습 거버넌스)
v19.0.0  PC 서버화           ⬜ 예정 (REST/MCP/WS 자동 생성)
v20.0.0  패키지 생태계        ⬜ 예정 (모듈 레지스트리 + FFI + SDK)
```

---

---

### ✅ 【v17.0.0】 안전 규격 — IEC 61508 / ISO 26262 기능안전 계약 시스템

> 산업/의료/자동차 기능안전 인증 기반 — 워치독 타이머, 결함허용(N중) 블록, 페일세이프 패턴

#### 신규 토큰 7종 (klexer)

| 토큰 | 한글 키워드 | 설명 |
|------|-----------|------|
| `TOK_KW_SIL` | `SIL` | 안전 등급 선언 (SIL=N) |
| `TOK_KW_WATCHDOG` | `워치독` | 워치독 타이머 블록 시작 |
| `TOK_KW_WATCHDOG_END` | `워치독끝` | 워치독 블록 종료 |
| `TOK_KW_FAULT_TOL` | `결함허용` | 결함허용(N중) 블록 시작 |
| `TOK_KW_FAULT_TOL_END` | `결함허용끝` | 결함허용 블록 종료 |
| `TOK_KW_FAILSAFE` | `페일세이프` | 페일세이프 내장함수 |
| `TOK_KW_EMERG_STOP` | `긴급정지` | 긴급정지 내장함수 |

#### 신규 AST 노드 2종 (kparser)

| 노드 | 설명 | ival | sval |
|------|------|------|------|
| `NODE_WATCHDOG_BLOCK` | 워치독 타이머 보호 블록 | 타임아웃(ms) | "500ms" 등 |
| `NODE_FAULT_TOL_BLOCK` | 결함허용 N중 복제 블록 | 중복수(N) | — |

#### 안전 내장함수 3종

| 한글 키워드 | C 함수 | 설명 |
|-----------|--------|------|
| `페일세이프()` | `kc_failsafe()` | 모든 출력 안전 상태 전환 |
| `긴급정지()` | `kc_emergency_stop()` | 시스템 즉시 정지 |
| `경보발령(메시지)` | `kc_alarm(msg)` | 안전 경보 발령 |

#### 문법 예시

```han
헌법 기계안전 SIL=2:
    법률 압력제어:
        법령:
            압력 < 최대압력
            온도 < 위험온도
        법위반:
            긴급정지()
            경보발령("안전위반")
            페일세이프()
    법률끝
헌법끝

// 워치독 타이머 — 500ms 내 완료 강제
워치독 500ms:
    정수 온도 = I2C읽기(0x48)
    GPIO쓰기(13, 온도 > 80 ? 0 : 1)
워치독끝

// 3중 결함허용 블록 — 3개 복제 경로 실행
결함허용 3중:
    정수 값1 = 센서읽기(0)
    정수 값2 = 센서읽기(1)
    정수 값3 = 센서읽기(2)
결함허용끝
```

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | `klexer.h/c` — 안전 규격 토큰 7종 추가 | ✅ |
| 2 | `kparser.h/c` — NODE_WATCHDOG_BLOCK / NODE_FAULT_TOL_BLOCK + is_callable_kw | ✅ |
| 3 | `kinterp.c` — 안전 내장함수 3종 등록 + 블록 평가 | ✅ |
| 4 | `kcodegen.c` — 안전 C코드 생성 (워치독/결함허용/내장함수) | ✅ |
| 5 | `kcodegen_llvm.c` — 안전 LLVM IR (워치독/결함허용/내장함수) | ✅ |
| 6 | gcc -fsyntax-only 검증 — klexer/kparser/kinterp/kcodegen/kcodegen_llvm 에러 0 | ✅ |
| 7 | readme.md 업데이트 | ✅ |

> **버전 정책**: 5개 파일 수정 → **v17.0.0** (앞자리 올림)

---

### ✅ 【v18.0.0】 온디바이스 AI — ONNX + TinyML + 연합학습

> 클라우드 없이 현장에서 AI 추론/학습 — MCU급 엣지 디바이스 지원

#### 신규 토큰 10종 (klexer) — v16/v17 토큰 전체 통합

| 토큰 | 한글 키워드 | 설명 |
|------|-----------|------|
| `TOK_KW_AI_MODEL` | `AI모델` | AI 모델 블록 시작 |
| `TOK_KW_AI_MODEL_END` | `AI모델끝` | AI 모델 블록 종료 |
| `TOK_KW_TINYML` | `TinyML` | TinyML 경량 모델 블록 |
| `TOK_KW_TINYML_END` | `TinyML끝` | TinyML 블록 종료 |
| `TOK_KW_AI_LOAD` | `AI불러오기` | ONNX/TinyML 모델 로드 |
| `TOK_KW_AI_PREDICT` | `AI추론` | 모델 추론 실행 |
| `TOK_KW_AI_TRAIN_STEP` | `AI학습단계` | 온디바이스 1스텝 학습 |
| `TOK_KW_AI_SAVE` | `AI저장` | 모델 가중치 저장 |
| `TOK_KW_FEDERATED` | `연합학습` | 연합학습 블록 시작 |
| `TOK_KW_FEDERATED_END` | `연합학습끝` | 연합학습 블록 종료 |

#### 신규 AST 노드 3종 (kparser)

| 노드 | 설명 |
|------|------|
| `NODE_AI_MODEL_BLOCK` | AI모델 블록 (sval=모델명, ival=0:ONNX/1:TinyML) |
| `NODE_TINYML_BLOCK` | TinyML 경량 모델 블록 |
| `NODE_FEDERATED_BLOCK` | 연합학습 블록 |

#### AI 내장함수 4종

| 한글 키워드 | C 함수 | 설명 |
|-----------|--------|------|
| `AI불러오기(경로)` | `kc_ai_load(path)` | ONNX/TinyML 모델 로드 → 핸들 반환 |
| `AI추론(핸들)` | `kc_ai_predict(h)` | 모델 추론 실행 → 결과 클래스 반환 |
| `AI학습단계(핸들)` | `kc_ai_train_step(h)` | 온디바이스 1스텝 학습 |
| `AI저장(핸들, 경로)` | `kc_ai_save(h, path)` | 가중치 저장 |

#### 문법 예시

```han
헌법 불량감지시스템 SIL=1:
    법률 AI추론제어:
        법령:
            AI모델핸들 > 0
        법위반:
            경보발령("모델 로드 실패")
            페일세이프()
    법률끝
헌법끝

AI모델 "불량감지":
    정수 모델 = AI불러오기("detect.onnx")
    실행:
        정수 결과 = AI추론(모델)
        만약 결과 == 1:
            MQTT발행("불량/감지", "불량품")
            경보발령("불량품 감지")
AI모델끝

TinyML "온도예측":
    정수 모델 = AI불러오기("temp.tflite")
    실행:
        정수 온도예측 = AI추론(모델)
        AI학습단계(모델)
        AI저장(모델, "temp_updated.tflite")
TinyML끝

연합학습:
    정수 로컬모델 = AI불러오기("local.bin")
    AI학습단계(로컬모델)
    AI저장(로컬모델, "local_v2.bin")
연합학습끝
```

#### 구현 단계

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | `klexer.h/c` — AI 토큰 10종 추가 + v16/v17 토큰 전체 통합 | ✅ |
| 2 | `kparser.h/c` — NODE_AI_MODEL/TINYML/FEDERATED_BLOCK + 파싱 + is_callable_kw | ✅ |
| 3 | `kinterp.c` — 산업/안전/AI 내장함수 전체 통합 등록 + 블록 평가 | ✅ |
| 4 | `kcodegen.c` — 산업/안전/AI C코드 생성 전체 통합 | ✅ |
| 5 | `kcodegen_llvm.c` — 산업/안전/AI LLVM IR 전체 통합 | ✅ |
| 6 | gcc -fsyntax-only 검증 — 전체 5파일 에러 0 | ✅ |
| 7 | readme.md 업데이트 | ✅ |

> **버전 정책**: 5개 파일 수정 → **v18.0.0** (앞자리 올림)

---

### ✅ 【v18.1.0】 내장함수 31종 추가 — 완료

> 명세(readme_skil.md) 미구현 함수 + 실용 함수군을 인터프리터/C생성/LLVM IR 3백엔드에 동시 추가

| 단계 | 작업 내용 | 상태 |
|------|----------|------|
| 1 | 기존 내장함수 전체 대조 — 명세 미구현/누락 31종 목록 확정 | ✅ |
| 2 | kinterp.c — 31종 함수 구현 + register_builtins 등록 | ✅ |
| 3 | kcodegen.c — gen_builtin_call() 31종 C코드 생성 추가 | ✅ |
| 4 | kcodegen_llvm.c — gen_call() 31종 LLVM IR 외부함수 선언 추가 | ✅ |
| 5 | gcc -fsyntax-only 검증 — 3파일 에러 0 | ✅ |
| 6 | readme.md 버전 히스토리 + 현재 상태 업데이트 | ✅ |

#### 추가된 함수 목록

| 분류 | 함수 목록 |
|------|---------|
| 수학 추가 | `제곱` `라디안` `각도` `난수` `난정수` |
| 역삼각함수 | `아크사인` `아크코사인` `아크탄젠트` `아크탄젠트2` |
| 글자 추가 | `좌문자` `우문자` `채우기` `코드` `붙여씀` |
| 배열 고급 | `배열삭제` `배열찾기` `배열포함` `배열합치기` `배열자르기` `유일값` `배열채우기` |
| 시간/날짜 | `현재시간` `현재날짜` `시간포맷` `경과시간` |
| 시스템 | `환경변수` `종료` `명령실행` `잠깐` |
| JSON | `JSON생성` `JSON파싱` |

> **버전 정책**: 3개 파일 수정 → **v18.1.0** (중간 자리 올림)

---

## 🔄 【v18.5.0】 온톨로지 통합 시스템 — 진행 중

> K엔진 3모드 (내장/대여/접속) + 학습 거버넌스 + MCP 자동 노출

### 전체 회차 계획

| 회차 | 단계 | 내용 | 상태 |
|------|------|------|------|
| **1회차** | 1~2 | `kc_ontology.h/c` 그래프 자료구조 + CRUD + 추론기 + 직렬화 + 거버넌스 | ✅ 완료 |
| **2회차** | 3~4 | `kc_ont_query.c` 한글/SPARQL 질의 + `kc_ont_import.c` JSON-LD/CSV | ✅ 완료 |
| **3회차** | 5~6 | `kc_ont_import.c` Turtle/OWL + `kc_ont_server.c` REST+WS | ✅ 완료 |
| **4회차** | 7~8 | `kc_ont_remote.h/c` 외부서버 클라이언트 + klexer/kparser 토큰/노드 | ⬜ 대기 |
| **5회차** | 9~10 | `kinterp.c` 3모드 블록 실행 + MCP 자동 노출 | ⬜ 대기 |
| **6회차** | 11~12 | `CMakeLists.txt` libkc_ontology.so + Python SDK | ⬜ 대기 |
| **7회차** | 13~14 | Node.js SDK + `kcodegen.c/kcodegen_llvm.c` 코드 생성 | ⬜ 대기 |
| **8회차** | 15 | 통합 테스트 (3모드 + 조합) | ⬜ 대기 |

### ✅ 1회차 완료 내역 (단계 1~2)

| 단계 | 내용 | 상태 |
|------|------|------|
| 1 | `kc_ontology.h` — 자료구조 전체 선언 (KcOntClass/Property/Relation/Rule/Instance/Ontology) | ✅ |
| 2 | `kc_ontology.c` — CRUD 구현 + 전방향체이닝 추론기 + JSON-LD/Turtle/JSON 직렬화 + 학습 거버넌스 | ✅ |
| - | `gcc -fsyntax-only` 검증 — 경고 0, 오류 0 | ✅ |

### 신규 파일 현황

| 파일 | 버전 | 역할 | 상태 |
|------|------|------|------|
| `kc_ontology.h` | v18.5.0 | K엔진 헤더 — 17개 섹션 자료구조 + API 선언 | ✅ 완료 |
| `kc_ontology.c` | v18.5.0 | K엔진 구현 — CRUD + 추론기 + 직렬화 + 거버넌스 | ✅ 완료 |

> **버전 정책**: 신규 파일 2개 → **v18.5.0** (중간 자리 올림)

---

*Kcode v18.5.0 — 온톨로지 K엔진 구축 중*

### ✅ 2회차 완료 내역 (단계 3~4)

| 단계 | 내용 | 상태 |
|------|------|------|
| 3 | `kc_ont_query.h/c` — 한글 질의 파서 + SPARQL 1.1 서브셋 실행기 + 결과 JSON 직렬화 | ✅ |
| 4 | `kc_ont_import.h/c` — CSV / JSON-LD / 내부 JSON 임포트 + 파일/문자열 진입점 | ✅ |
| - | `gcc -fsyntax-only` 검증 — 경고 0, 오류 0 (전체 4파일) | ✅ |

---

### ✅ 3회차 완료 내역 (단계 5~6) — v19.0.0

| 단계 | 내용 | 상태 |
|------|------|------|
| 5 | `kc_ont_import.h/c` — Turtle (TTL) 파서 + OWL/XML 파서 추가 | ✅ |
| 6 | `kc_ont_server.h/c` 신규 — REST+WebSocket 온톨로지 서버 | ✅ |
| - | `gcc -fsyntax-only` 검증 — 경고 0, 오류 0 (전체 4파일) | ✅ |

#### Turtle 임포트 지원 문법
- `@prefix ns: <URI> .` 선언
- `subject predicate object .` 삼중 트리플
- `subject a ClassName .` (rdf:type 단축)
- `subject pred1 obj1 ; pred2 obj2 .` 복수 술어
- `subject pred obj1, obj2 .` 복수 목적어
- `rdfs:subClassOf` → 클래스 상속, 일반 술어 → 엣지/속성 자동 분류

#### OWL/XML 임포트 지원 요소
- `<owl:Class rdf:about="...">` + `<rdfs:subClassOf>`
- `<owl:ObjectProperty>` / `<owl:DatatypeProperty>`
- `<owl:NamedIndividual>` + `<rdf:type>` + 속성/엣지

#### REST API 엔드포인트

| 메서드 | 경로 | 설명 |
|--------|------|------|
| GET | `/health` | 서버 상태 |
| GET | `/api/classes` | 전체 클래스 목록 |
| GET | `/api/classes/:name` | 클래스 상세 |
| POST | `/api/classes` | 클래스 추가 |
| GET | `/api/instances` | 인스턴스 목록 |
| GET | `/api/instances/:id` | 인스턴스 상세 |
| POST | `/api/instances` | 인스턴스 추가 |
| GET | `/api/relations` | 관계 타입 목록 |
| POST | `/api/relations` | 관계 타입 추가 |
| GET | `/api/query?q=...` | 한글 질의 실행 |
| POST | `/api/import` | 포맷별 임포트 (csv/json/jsonld/turtle/owl) |
| GET | `/api/export?fmt=...` | 직렬화 내보내기 |
| GET | `/api/infer` | 전방향체이닝 추론 실행 |
| WS | `/ws` | WebSocket 실시간 변경 이벤트 스트림 |

#### 신규 파일 현황

| 파일 | 버전 | 역할 | 상태 |
|------|------|------|------|
| `kc_ont_import.h` | v19.0.0 | Turtle/OWL 선언 추가 | ✅ |
| `kc_ont_import.c` | v19.0.0 | Turtle/OWL 파서 구현 | ✅ |
| `kc_ont_server.h` | v19.0.0 | REST+WS 서버 헤더 | ✅ 신규 |
| `kc_ont_server.c` | v19.0.0 | REST+WS 서버 구현 | ✅ 신규 |

> **버전 정책**: 4개 파일 수정/신규 → **v19.0.0** (앞자리 올림)

---

### ✅ 4회차 완료 내역 (단계 7~8) — v18.5.0

| 단계 | 내용 | 상태 |
|------|------|------|
| 7 | `kc_ont_remote.h/c` 신규 — 외부 온톨로지 서버 HTTP 클라이언트 (모드 3) | ✅ |
| 8 | `klexer.h/c` — 온톨로지 토큰 10종 추가 + TOK_NAMES 등록 | ✅ |
| 8 | `kparser.h/c` — 온톨로지 AST 노드 6종 + 파싱 함수 구현 | ✅ |
| - | `gcc -fsyntax-only` 검증 — 경고 0, 오류 0 (전체 6파일) | ✅ |

#### kc_ont_remote — 모드 3 클라이언트 기능

| 분류 | 함수 | 설명 |
|------|------|------|
| 연결 | `kc_ont_remote_connect(url)` | 원격 서버 TCP 연결 (http://host:port) |
| 연결 | `kc_ont_remote_disconnect(rem)` | 연결 종료 및 핸들 해제 |
| 연결 | `kc_ont_remote_health(rem)` | GET /health 서버 상태 확인 |
| 동기화 | `kc_ont_remote_pull(rem, onto)` | 원격 → 로컬 온톨로지 전체 가져오기 |
| 동기화 | `kc_ont_remote_push(rem, onto)` | 로컬 → 원격 온톨로지 업로드 |
| 클래스 | `kc_ont_remote_list_classes()` | GET /api/classes |
| 클래스 | `kc_ont_remote_get_class()` | GET /api/classes/:name |
| 클래스 | `kc_ont_remote_add_class()` | POST /api/classes |
| 인스턴스 | `kc_ont_remote_list_instances()` | GET /api/instances |
| 인스턴스 | `kc_ont_remote_add_instance()` | POST /api/instances |
| 관계 | `kc_ont_remote_add_relation()` | POST /api/relations |
| 질의 | `kc_ont_remote_query(rem, 한글질의)` | GET /api/query?q=... |
| 질의 | `kc_ont_remote_sparql()` | SPARQL 질의 실행 |
| 추론 | `kc_ont_remote_infer()` | GET /api/infer 원격 추론 |
| 임포트 | `kc_ont_remote_import_str()` | POST /api/import (turtle/owl/json/csv) |
| 익스포트 | `kc_ont_remote_export()` | GET /api/export?fmt=... |
| WS | `kc_ont_remote_ws_subscribe(cb)` | WebSocket /ws 실시간 이벤트 구독 |
| WS | `kc_ont_remote_ws_tick()` | 논블로킹 이벤트 처리 (select 폴링) |
| WS | `kc_ont_remote_ws_unsubscribe()` | WebSocket 연결 종료 |

#### 신규 온톨로지 토큰 10종 (klexer)

| 토큰 | 한글 키워드 | 설명 |
|------|-----------|------|
| `TOK_KW_ONTOLOGY` | `온톨로지` | 온톨로지 블록 시작 |
| `TOK_KW_ONTOLOGY_END` | `온톨로지끝` | 온톨로지 블록 종료 |
| `TOK_KW_ONT_CONCEPT` | `개념` | 클래스(개념) 정의 블록 시작 |
| `TOK_KW_ONT_CONCEPT_END` | `개념끝` | 개념 블록 종료 |
| `TOK_KW_ONT_PROP` | `속성` | 클래스 속성 선언 |
| `TOK_KW_ONT_RELATE` | `관계` | 관계 타입 정의 |
| `TOK_KW_ONT_QUERY_BLOCK` | `질의` | 온톨로지 질의 실행 |
| `TOK_KW_ONT_INFER_BLOCK` | `추론` | 추론기 실행 블록 |
| `TOK_KW_ONT_SENSITIVE` | `민감` | 속성 민감 지정 |
| `TOK_KW_ONT_ANON` | `익명화` | 속성 익명화 지정 |

#### 신규 AST 노드 6종 (kparser)

| 노드 | 설명 |
|------|------|
| `NODE_ONT_BLOCK` | 온톨로지 블록 (val.ival: 0=내장, 1=대여, 2=접속) |
| `NODE_ONT_CONCEPT` | 개념 정의 블록 (sval=이름, child[0]=부모 개념) |
| `NODE_ONT_PROP` | 속성 선언 (sval=이름, val.ival=타입, op=민감/익명화) |
| `NODE_ONT_RELATE` | 관계 정의 (sval=이름, child[0]=from, child[1]=to) |
| `NODE_ONT_QUERY` | 질의 실행 (sval=질의문, child[0]=결과변수) |
| `NODE_ONT_INFER` | 추론 실행 블록 |

#### 문법 예시 (4회차 신규 문법)

```han
// ── 모드 1: 내장 온톨로지 ──────────────────────────────
온톨로지 "내장":
    개념 "제품":
        속성 "이름" 글자
        속성 "가격" 실수
        속성 "고객번호" 정수 민감
        속성 "계좌" 글자 익명화
    개념끝

    개념 "전자제품" 이어받기 "제품":
        속성 "전압" 실수
    개념끝

    관계 "구매함" 사람 제품
    관계 "포함함" 제품 제품

    추론:
        출력("추론 실행 완료")
    추론끝
온톨로지끝

// ── 모드 3: 접속 (원격 서버) ──────────────────────────
온톨로지 "접속" "http://192.168.1.10:8765":
    질의 "전자제품 찾기" => 결과
    출력(결과)
온톨로지끝
```

#### 신규 파일 현황

| 파일 | 버전 | 역할 | 상태 |
|------|------|------|------|
| `kc_ont_remote.h` | v18.5.0 | 외부 서버 클라이언트 헤더 | ✅ 신규 |
| `kc_ont_remote.c` | v18.5.0 | HTTP REST + WebSocket 클라이언트 구현 | ✅ 신규 |
| `klexer.h` | v18.5.0 | 온톨로지 토큰 10종 추가 | ✅ 수정 |
| `klexer.c` | v18.5.0 | 키워드 매핑 10종 + TOK_NAMES 등록 | ✅ 수정 |
| `kparser.h` | v18.5.0 | 온톨로지 AST 노드 6종 추가 | ✅ 수정 |
| `kparser.c` | v18.5.0 | 온톨로지 파싱 함수 구현 | ✅ 수정 |

> **버전 정책**: 6개 파일 수정/신규 → **v18.5.0** 유지 (회차 내 작업)

### 전체 회차 계획 (업데이트)

| 회차 | 단계 | 내용 | 상태 |
|------|------|------|------|
| **1회차** | 1~2 | `kc_ontology.h/c` 그래프 자료구조 + CRUD + 추론기 + 직렬화 + 거버넌스 | ✅ 완료 |
| **2회차** | 3~4 | `kc_ont_query.c` 한글/SPARQL 질의 + `kc_ont_import.c` JSON-LD/CSV | ✅ 완료 |
| **3회차** | 5~6 | `kc_ont_import.c` Turtle/OWL + `kc_ont_server.c` REST+WS | ✅ 완료 |
| **4회차** | 7~8 | `kc_ont_remote.h/c` 외부서버 클라이언트 + klexer/kparser 토큰/노드 | ✅ 완료 |
| **5회차** | 9~10 | `kinterp.c` 3모드 블록 실행 + MCP 자동 노출 | ⬜ 대기 |
| **6회차** | 11~12 | `CMakeLists.txt` libkc_ontology.so + Python SDK | ⬜ 대기 |
| **7회차** | 13~14 | Node.js SDK + `kcodegen.c/kcodegen_llvm.c` 코드 생성 | ⬜ 대기 |
| **8회차** | 15 | 통합 테스트 (3모드 + 조합) | ⬜ 대기 |

---

*Kcode v18.5.0 — 온톨로지 K엔진 4회차 완료 (kc_ont_remote + klexer/kparser 온톨로지 토큰)*
