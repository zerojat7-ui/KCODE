# Kcode v3.4.0 — 설치 경로 & 공유 라이브러리(DLL) 통합 가이드

---

## 1. 각 파일의 역할과 권장 설치 경로

### 1-1. Linux / macOS 표준 경로

```
/usr/local/
├── bin/
│   ├── kcode              ← 인터프리터 실행파일
│   ├── kcode_gen          ← C 코드 생성기 실행파일
│   ├── kcode_llvm         ← LLVM IR 생성기 실행파일
│   └── kserver            ← HTTP 서버 실행파일
│
├── lib/
│   └── libkcode.so        ← 공유 라이브러리 (핵심 엔진)
│       libkcode.so.3       (soname 링크)
│       libkcode.so.3.4.0   (실제 파일)
│
├── include/
│   └── kcode/
│       ├── klexer.h       ← 렉서 공개 API
│       ├── kparser.h      ← 파서 공개 API
│       ├── kinterp.h      ← 인터프리터 공개 API
│       ├── kcodegen.h     ← C 코드 생성기 공개 API
│       └── kcodegen_llvm.h← LLVM 생성기 공개 API
│
└── share/
    └── kcode/
        ├── README.md
        └── examples/
```

### 1-2. Windows 표준 경로

```
C:\Program Files\Kcode\
├── bin\
│   ├── kcode.exe
│   ├── kcode_gen.exe
│   ├── kcode_llvm.exe
│   ├── kserver.exe
│   └── kcode.dll          ← Windows DLL (실행파일 옆에 배치)
│
├── include\
│   └── kcode\
│       ├── klexer.h
│       ├── kparser.h
│       ├── kinterp.h
│       ├── kcodegen.h
│       └── kcodegen_llvm.h
│
└── lib\
    ├── kcode.lib          ← import library (링커용)
    └── kcode.dll          ← 또는 lib\ 에도 복사
```

### 1-3. 파일별 역할 요약

| 파일 | 역할 | 설치 위치 |
|---|---|---|
| `klexer.h` / `klexer.c` | 렉서 (토큰화) | `include/kcode/`, lib 에 포함 |
| `kparser.h` / `kparser.c` | 파서 (AST 생성) | `include/kcode/`, lib 에 포함 |
| `kinterp.h` / `kinterp.c` | 인터프리터 (실행) | `include/kcode/`, lib 에 포함 |
| `kcodegen.h` / `kcodegen.c` | C 코드 생성기 | `include/kcode/`, lib 에 포함 |
| `kcodegen_llvm.h` / `kcodegen_llvm.c` | LLVM IR 생성기 | `include/kcode/`, lib 에 포함 |
| `kinterp_main.c` | 인터프리터 진입점 | → `bin/kcode` 빌드 후 제거 |
| `kcodegen_main.c` | C 생성기 진입점 | → `bin/kcode_gen` 빌드 후 제거 |
| `kcodegen_llvm_main.c` | LLVM 드라이버 | → `bin/kcode_llvm` 빌드 후 제거 |
| `kserver.c` | HTTP 서버 | → `bin/kserver` 빌드 후 제거 |
| `ktest_lexer.c` | 렉서 단위 테스트 | 개발 환경에만 보관 |

---

## 2. 공유 라이브러리(DLL) 만들기 — 3가지 방법

핵심 엔진인 렉서·파서·인터프리터·코드생성기를 **하나의 라이브러리 파일**로 묶는 방법입니다.

### 방법 A. Linux — 공유 라이브러리 (.so)

#### 빌드 단계

```bash
# 1단계: 위치독립 코드(PIC)로 각 .c 파일을 오브젝트 파일로 컴파일
gcc -fPIC -Wall -std=c11 -c klexer.c        -o klexer.o
gcc -fPIC -Wall -std=c11 -c kparser.c       -o kparser.o
gcc -fPIC -Wall -std=c11 -c kinterp.c       -o kinterp.o
gcc -fPIC -Wall -std=c11 -c kcodegen.c      -o kcodegen.o
gcc -fPIC -Wall -std=c11 -c kcodegen_llvm.c -o kcodegen_llvm.o  # LLVM 있을 때만

# 2단계: 공유 라이브러리 생성
gcc -shared -Wl,-soname,libkcode.so.3 \
    klexer.o kparser.o kinterp.o kcodegen.o \
    -lm \
    -o libkcode.so.3.4.0

# 3단계: 심볼릭 링크 생성
ln -sf libkcode.so.3.4.0 libkcode.so.3
ln -sf libkcode.so.3.4.0 libkcode.so

# 4단계: 시스템에 설치
sudo cp libkcode.so.3.4.0 /usr/local/lib/
sudo ln -sf /usr/local/lib/libkcode.so.3.4.0 /usr/local/lib/libkcode.so.3
sudo ln -sf /usr/local/lib/libkcode.so.3.4.0 /usr/local/lib/libkcode.so
sudo ldconfig   # 라이브러리 캐시 갱신

# 헤더 설치
sudo mkdir -p /usr/local/include/kcode
sudo cp klexer.h kparser.h kinterp.h kcodegen.h kcodegen_llvm.h \
        /usr/local/include/kcode/
```

#### 실행파일을 라이브러리로 링크하는 방법

```bash
# 인터프리터 빌드 — 라이브러리 사용
gcc -Wall -std=c11 kinterp_main.c \
    -L/usr/local/lib -lkcode \
    -Wl,-rpath,/usr/local/lib \
    -o kcode

# LLVM 버전 빌드
gcc -Wall -std=c11 kcodegen_llvm_main.c \
    -L/usr/local/lib -lkcode \
    $(llvm-config --ldflags --libs core analysis native bitwriter) \
    -Wl,-rpath,/usr/local/lib \
    -o kcode_llvm

# kserver는 라이브러리 불필요 (독립 단일 파일)
gcc -O2 -o kserver kserver.c
```

#### 다른 프로그램에서 사용

```c
#include <kcode/klexer.h>
#include <kcode/kparser.h>

// 컴파일:
// gcc myapp.c -lkcode -o myapp
```

---

### 방법 B. macOS — 동적 라이브러리 (.dylib)

```bash
# 1단계: PIC 오브젝트 컴파일
gcc -fPIC -Wall -std=c11 -c klexer.c   -o klexer.o
gcc -fPIC -Wall -std=c11 -c kparser.c  -o kparser.o
gcc -fPIC -Wall -std=c11 -c kinterp.c  -o kinterp.o
gcc -fPIC -Wall -std=c11 -c kcodegen.c -o kcodegen.o

# 2단계: .dylib 생성
gcc -dynamiclib \
    -install_name /usr/local/lib/libkcode.3.dylib \
    -current_version 3.4.0 \
    -compatibility_version 3.0.0 \
    klexer.o kparser.o kinterp.o kcodegen.o \
    -lm \
    -o libkcode.3.4.0.dylib

# 3단계: 심볼릭 링크
ln -sf libkcode.3.4.0.dylib libkcode.3.dylib
ln -sf libkcode.3.4.0.dylib libkcode.dylib

# 4단계: 설치
sudo cp libkcode.3.4.0.dylib /usr/local/lib/
sudo ln -sf /usr/local/lib/libkcode.3.4.0.dylib /usr/local/lib/libkcode.3.dylib
sudo ln -sf /usr/local/lib/libkcode.3.4.0.dylib /usr/local/lib/libkcode.dylib

# 실행파일 빌드
gcc kinterp_main.c -L/usr/local/lib -lkcode -o kcode
```

---

### 방법 C. Windows — DLL (MinGW / MSVC)

#### C. MinGW-w64 (gcc) 사용

```bash
# 1단계: 오브젝트 파일 컴파일
gcc -Wall -std=c11 -c klexer.c        -o klexer.o
gcc -Wall -std=c11 -c kparser.c       -o kparser.o
gcc -Wall -std=c11 -c kinterp.c       -o kinterp.o
gcc -Wall -std=c11 -c kcodegen.c      -o kcodegen.o

# 2단계: DLL + import library 동시 생성
gcc -shared \
    klexer.o kparser.o kinterp.o kcodegen.o \
    -lm \
    -Wl,--out-implib,libkcode.lib \
    -o kcode.dll

# 3단계: 실행파일 빌드 (DLL 사용)
gcc kinterp_main.c -L. -lkcode -o kcode.exe

# → kcode.exe 와 kcode.dll 을 같은 폴더에 배치
```

#### C-2. MSVC (cl.exe) 사용

헤더에 export 선언이 필요합니다. `klexer.h` 상단에 아래를 추가하거나
별도 `kcode_export.h` 를 작성합니다.

```c
// kcode_export.h
#pragma once
#ifdef _WIN32
  #ifdef KCODE_BUILD_DLL
    #define KCODE_API __declspec(dllexport)
  #else
    #define KCODE_API __declspec(dllimport)
  #endif
#else
  #define KCODE_API
#endif
```

각 공개 함수 앞에 `KCODE_API` 를 붙인 뒤:

```bat
REM DLL 빌드
cl /LD /DKCODE_BUILD_DLL klexer.c kparser.c kinterp.c kcodegen.c ^
   /Fe:kcode.dll /link /OUT:kcode.dll

REM 실행파일 빌드
cl kinterp_main.c kcode.lib /Fe:kcode.exe
```

---

## 3. Makefile 통합 — 한 번에 빌드하기

현재 `Makefile`에 아래 타겟을 추가하면 됩니다.

```makefile
# ── 라이브러리용 소스 ──────────────────────────────────────
LIB_SRC = klexer.c kparser.c kinterp.c kcodegen.c
LIB_OBJ = $(LIB_SRC:.c=.pic.o)
LIB_NAME = libkcode

# PIC 오브젝트 빌드 규칙
%.pic.o: %.c
	$(CC) $(CFLAGS) -fPIC -c $< -o $@

# ── Linux .so ──────────────────────────────────────────────
.PHONY: shared
shared: $(LIB_OBJ)
	$(CC) -shared -Wl,-soname,$(LIB_NAME).so.3 \
	    $(LIB_OBJ) -lm -o $(LIB_NAME).so.3.4.0
	ln -sf $(LIB_NAME).so.3.4.0 $(LIB_NAME).so.3
	ln -sf $(LIB_NAME).so.3.4.0 $(LIB_NAME).so
	@echo "[공유 라이브러리 생성] $(LIB_NAME).so.3.4.0"

# ── 정적 라이브러리 .a (선택) ─────────────────────────────
.PHONY: static
static: $(LIB_OBJ)
	ar rcs $(LIB_NAME).a $(LIB_OBJ)
	@echo "[정적 라이브러리 생성] $(LIB_NAME).a"

# ── 설치 ──────────────────────────────────────────────────
PREFIX ?= /usr/local
.PHONY: install
install: shared
	install -d $(PREFIX)/lib $(PREFIX)/bin $(PREFIX)/include/kcode
	install -m 755 $(LIB_NAME).so.3.4.0 $(PREFIX)/lib/
	ln -sf $(PREFIX)/lib/$(LIB_NAME).so.3.4.0 $(PREFIX)/lib/$(LIB_NAME).so.3
	ln -sf $(PREFIX)/lib/$(LIB_NAME).so.3.4.0 $(PREFIX)/lib/$(LIB_NAME).so
	install -m 644 klexer.h kparser.h kinterp.h kcodegen.h kcodegen_llvm.h \
	    $(PREFIX)/include/kcode/
	ldconfig
	@echo "[설치 완료] $(PREFIX)"

.PHONY: uninstall
uninstall:
	rm -f $(PREFIX)/lib/$(LIB_NAME).so*
	rm -rf $(PREFIX)/include/kcode
	rm -f $(PREFIX)/bin/kcode $(PREFIX)/bin/kcode_gen \
	      $(PREFIX)/bin/kcode_llvm $(PREFIX)/bin/kserver
	ldconfig
	@echo "[제거 완료]"
```

### 사용 예시

```bash
make shared          # .so 생성
make static          # .a 생성
sudo make install    # /usr/local 에 설치
sudo make install PREFIX=/opt/kcode  # 경로 지정
sudo make uninstall  # 제거
```

---

## 4. 정적 라이브러리(.a) vs 공유 라이브러리(.so/.dll) 비교

| 항목 | 정적 (.a / .lib) | 공유 (.so / .dll / .dylib) |
|---|---|---|
| 파일 크기 | 실행파일에 포함되어 커짐 | 실행파일은 작고 .so 별도 존재 |
| 배포 편의성 | 실행파일 하나로 완결 | .so/.dll 함께 배포 필요 |
| 메모리 공유 | 각 프로세스 별도 로드 | 여러 프로세스가 메모리 공유 |
| 업데이트 | 재컴파일 필요 | .so만 교체하면 즉시 반영 |
| 권장 용도 | 단일 실행파일 배포 | IDE 플러그인, 다중 도구 공유 |

**Kcode 프로젝트에서는:**
- `kserver` → 단독 실행이므로 **정적 링크** 또는 독립 빌드 권장
- `kcode`, `kcode_gen`, `kcode_llvm` → **공유 라이브러리** 권장 (엔진 공유)
- IDE 플러그인 개발 시 → **.so / .dll** 필수

---

## 5. kserver.c 단독 패키지 방법 (라이브러리 불필요)

`kserver.c` 는 렉서/파서를 직접 포함하지 않고 자식 프로세스(`kcode_llvm -ide`)를 실행하는 방식이므로 **단일 파일**로 독립 빌드가 가능합니다.

```bash
# Linux / macOS
gcc -O2 -o kserver kserver.c
strip kserver            # 심볼 제거로 크기 최소화

# Windows (MinGW)
gcc -O2 -o kserver.exe kserver.c -lws2_32
# ※ Windows는 소켓 라이브러리 ws2_32 추가 필요
#   + kserver.c의 헤더를 Windows 소켓 API로 포팅 필요
```

---

## 6. 전체 빌드·설치 순서 요약

```bash
# ① 공유 라이브러리 빌드
make shared

# ② 실행파일 빌드 (라이브러리 사용)
make interp gen          # kcode, kcode_gen
make LLVM=1              # kcode_llvm (LLVM 있을 때)
gcc -O2 -o kserver kserver.c   # kserver

# ③ 시스템 설치
sudo make install

# ④ 실행파일 설치
sudo install -m 755 kcode kcode_gen kcode_llvm kserver /usr/local/bin/

# ⑤ 동작 확인
kcode --version
kserver &
curl http://localhost:8080/health
```
