# Results

## 1. 실험 요약
- 저장소: exp-deckgl-webgpu-readiness
- 커밋 해시: 99c8c37
- 실험 일시: 2026-05-20T15:41:08.705Z -> 2026-05-20T15:41:14.921Z
- 담당자: ai-webgpu-lab
- 실험 유형: `graphics`
- 상태: `success`

## 2. 질문
- deck.gl WebGPU readiness baseline으로 넘기기 전에 map layer scene load와 frame pacing 보고 경로를 먼저 고정할 수 있는가
- viewport update, layer count, tile count, picking metadata와 fallback state가 graphics 결과 문서에 같이 남는가
- 실제 deck.gl WebGPU renderer 교체 전 deterministic map visualization harness로 반복 검증이 가능한가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 32 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: navigator.gpu available
- backend: `webgpu`
- fallback triggered: `false`
- worker mode: `main`
- cache state: `warm`
- required features: ["texture-compression-bc","timestamp-query"]
- limits snapshot: {"maxTextureDimension2D":8192,"maxVertexAttributes":16,"maxBindGroups":4}

## 4. 워크로드 정의
- 시나리오 이름: Deck.gl Readiness
- 입력 프로필: 2048-points-96-arcs-24-polygons-18-tiles
- 데이터 크기: pointCount=2048; arcCount=96; polygonCount=24; layerCount=4; tileCount=18; pickingSamples=128; attributeBufferMB=0.0353; avgViewportUpdateMs=0.0131; backend=webgpu; fallback=false; automation=playwright-chromium, pointCount=2048; arcCount=96; polygonCount=24; layerCount=4; tileCount=18; pickingSamples=128; attributeBufferMB=0.0353; avgViewportUpdateMs=0.0131; backend=webgpu; fallback=false; realAdapter=fallback(The requested module '/@deck.gl/core@^9.0.0?target=es2022' does not provide an export named 'gouraudLighting'); automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: deckgl-webgpu-readiness
- 양자화/정밀도: -
- resolution: 960x540
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 1623.8 ~ 2840.6 ms
- init_ms: 38.2 ms
- success_rate: 1
- peak_memory_note: 32 GB reported by browser
- error_type: -

### Graphics / Blackhole
- avg_fps: 60.37 ~ 60.6
- p95_frametime_ms: 17.5 ~ 17.6 ms
- scene_load_ms: 38.2 ms
- ray_steps: 0
- taa states: false
- fallback states: false
- backends: webgpu

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Deck.gl Readiness | webgpu | warm | 60.37 | 17.6 | scene_load=38.2 ms, fallback=false |
| 2 | Deck.gl Readiness | webgpu | warm | 60.6 | 17.5 | scene_load=38.2 ms, fallback=false |

## 7. 관찰
- Deck.gl readiness baseline은 backend=webgpu, fallback_triggered=false로 기록됐다.
- graphics summary는 avg_fps=60.37, p95_frametime_ms=17.6, scene_load_ms=38.2였다.
- viewport/layer/picking metadata는 pointCount=2048; arcCount=96; polygonCount=24; layerCount=4; tileCount=18; pickingSamples=128; attributeBufferMB=0.0353; avgViewportUpdateMs=0.0131; backend=webgpu; fallback=false; automation=playwright-chromium로 남았다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. Real Adapter vs Deterministic
- adapter: real=not-connected (no real adapter registered — falling back to deterministic), deterministic=deterministic-three-style
- avg_fps: real=60.6, deterministic=60.37, delta=+0.23
- p95_frametime: real=17.5 ms, deterministic=17.6 ms, delta=-0.1 ms
- scene_load_ms: real=38.2 ms, deterministic=38.2 ms, delta=0 ms

## 9. 결론
- deck.gl 계열 WebGPU readiness 실험으로 넘어가기 전 viewport/layer/picking readiness baseline과 결과 문서가 연결됐다.
- 다음 단계는 deterministic canvas surface를 실제 deck.gl WebGPU renderer로 교체하되 viewport/layer/tile/picking metadata와 graphics metric 구조를 유지하는 것이다.
- 이후 luma.gl baseline과 geospatial visualization renderer 비교의 입력 baseline으로 재사용할 수 있다.

## 10. 첨부
- 스크린샷: ./reports/screenshots/01-deckgl-readiness.png, ./reports/screenshots/02-deckgl-webgpu-real-deckgl.png
- 로그 파일: ./reports/logs/01-deckgl-readiness.log, ./reports/logs/02-deckgl-webgpu-real-deckgl.log
- raw json: ./reports/raw/01-deckgl-readiness.json, ./reports/raw/02-deckgl-webgpu-real-deckgl.json
- 배포 URL: https://ai-webgpu-lab.github.io/exp-deckgl-webgpu-readiness/
- 관련 이슈/PR: -
