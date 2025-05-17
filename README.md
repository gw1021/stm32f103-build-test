# STM32 멀티 보드 프로젝트

이 프로젝트는 여러 STM32 보드에 대한 빌드를 지원합니다. 현재 Board1과 Board2 두 개의 보드 구성이 있으며, CMake를 통해 서로 다른 보드와 빌드 타입(Debug, Release 등)을 손쉽게 선택할 수 있습니다.

## 프로젝트 구조

```
├── App/                  # 애플리케이션 소스 코드
├── Boards/               # 보드별 구성 파일
│   ├── Board1/           # Board1 관련 파일
│   │   ├── Core/         # 메인 소스 코드
│   │   └── STM32F103XX_FLASH.ld  # 링커 스크립트
│   └── Board2/           # Board2 관련 파일
│       ├── Core/         # 메인 소스 코드
│       └── STM32F103XX_FLASH.ld  # 링커 스크립트
├── Drivers/              # STM32 HAL 드라이버
├── cmake/                # CMake 관련 파일
│   ├── gcc-arm-none-eabi.cmake   # 툴체인 파일
│   └── stm32cubemx/      # STM32CubeMX 생성 CMake 파일
├── build/                # 빌드 출력 디렉토리
├── CMakeLists.txt        # 메인 CMake 구성 파일
└── CMakePresets.json     # CMake 프리셋 정의 파일
```

## 빌드 방법

### CMake 프리셋 사용하기

프로젝트는 빌드 타입과 보드 선택을 위한 CMake 프리셋을 제공합니다.

#### 빌드 타입 프리셋

```zsh
# Debug 모드로 빌드 (기본 보드: Board1)
cmake --preset Debug
cmake --build --preset Debug

# Release 모드로 빌드 (기본 보드: Board1)
cmake --preset Release
cmake --build --preset Release

# 기타 빌드 타입
cmake --preset RelWithDebInfo
cmake --build --preset RelWithDebInfo

cmake --preset MinSizeRel
cmake --build --preset MinSizeRel
```

#### 보드 선택 프리셋

```zsh
# Board1 선택 (기본 빌드 타입 사용)
cmake --preset Board1
cmake --build --preset Board1

# Board2 선택 (기본 빌드 타입 사용)
cmake --preset Board2
cmake --build --preset Board2
```

### 명령줄에서 조합하기

프리셋과 명령줄 옵션을 조합하여 더욱 유연하게 빌드할 수 있습니다:

```zsh
# Debug 프리셋 + Board2 선택
cmake --preset Debug -DSELECTED_BOARD=Board2
cmake --build --preset Debug

# Board2 프리셋 + Release 모드 선택
cmake --preset Board2 -DCMAKE_BUILD_TYPE=Release
cmake --build --preset Board2
```

### 직접 CMake 명령어 사용하기

프리셋 없이 직접 CMake 명령어를 사용할 수도 있습니다:

```zsh
# 새 빌드 디렉토리 생성
mkdir -p build/custom && cd build/custom

# Board2, Release 모드로 구성 (Ninja 제너레이터 사용)
cmake -G Ninja -DCMAKE_BUILD_TYPE=Release -DSELECTED_BOARD=Board2 ../..

# 빌드 실행
ninja
# 또는
cmake --build .
```

## 변수 설명

- `CMAKE_BUILD_TYPE`: 빌드 타입을 지정 (Debug, Release, RelWithDebInfo, MinSizeRel)
- `SELECTED_BOARD`: 빌드할 보드를 지정 (Board1, Board2)

## 주의사항

- 기본 보드는 `Board1`입니다.
- 기본 빌드 타입은 `Debug`입니다.
