# AI 캐릭터 비서

Open-LLM-VTuber의 캐릭터 화면을 바탕으로 만든 Windows용 개인 AI 비서입니다.
본인의 ChatGPT 구독으로 로그인하고, 한국어 음성 대화와 화면 이해, 먼저 말하기, 데스크톱 펫 모드를 사용할 수 있습니다.

**v0.1.0은 소스 기반 공개 테스트 버전입니다.** 제작자의 로그인 정보나 대화 기록은 들어 있지 않습니다.
처음 실행할 때 사용자가 직접 로그인해야 합니다. 다른 구독형 AI 제공자는 아직 지원하지 않습니다.

## 기능

- 한국어 마이크 입력 → 로컬 Faster-Whisper 인식 → ChatGPT 구독 AI 응답
- Edge TTS 한국어 음성 출력, Live2D 표정·입 움직임, 응답 중단
- 선택한 창의 화면을 이미지로 전달해 질문하기
- 대기 후 먼저 말하기: 기본 꺼짐, 대기 시간 설정 가능
- Electron 데스크톱 펫 모드와 창 모드
- 로컬 대화 기록 저장·불러오기
- 마크다운 강조 기호를 자막과 음성에서 정리

## 설치와 실행

자세한 순서는 **[실행방법.txt](실행방법.txt)**를 메모장으로 열어 확인하세요.

필요한 프로그램은 Node.js 22 이상, pnpm 11, Python 3.10 이상(음성 환경 설치용),
`app-server`를 지원하는 공식 Codex CLI입니다. 음성 실행 환경은 프로젝트 내부에 Python 3.12로 따로 만듭니다.
GitHub ZIP을 받은 경우 먼저 압축을 풀고 해당 폴더에서 PowerShell을 여세요.

```powershell
npm install --global pnpm@11
npm install --global @openai/codex
.\setup.ps1
.\start.ps1
```

브라우저에서 `http://127.0.0.1:12393`을 열고 **ChatGPT로 로그인**을 선택합니다.
이 앱은 전용 `.runtime/codex` 폴더를 사용하므로 기존 Codex 로그인과 분리됩니다.
인증이 끝나면 **상태 확인 → 캐릭터와 대화하기** 순서로 진행하세요.
구독 권한과 사용량 제한은 본인 계정에 적용됩니다. API 키 인증은 사용하지 않습니다.

데스크톱 앱은 서버를 켜둔 상태에서 별도 PowerShell로 실행합니다.

```powershell
.\start-desktop.ps1
```

모드 메뉴의 **Pet Mode**로 전환하고, 트레이의 **Window Mode**로 돌아올 수 있습니다.
완성된 설치형 EXE를 제공하는 릴리즈는 아닙니다. 최초 설치에는 인터넷 연결과 다운로드 공간이 필요합니다.

## 개인정보와 저장 위치

- 로그인·설정·캐시: `.runtime/` 내부. Git 업로드와 배포 ZIP에서 제외합니다.
- 대화 기록: `.runtime/history/`. 로컬에 저장합니다.
- 마이크 음성 인식: 로컬 실행. 처음에는 Whisper 모델을 다운로드합니다.
- AI 답변 생성: 로그인한 사용자의 ChatGPT 서비스로 질문과 대화 문맥을 전송합니다.
- 음성 합성: 답변 텍스트를 Edge 온라인 음성 서비스에 전송합니다.
- 화면: 사용자가 공유할 창을 고른 뒤, 질문이나 먼저 말하기 실행 시 이미지를 전송합니다.
- 기존 ChatGPT 대화와 메모리를 자동으로 가져오지는 않습니다.

공유를 멈추려면 Screen 미리보기를 다시 누르세요. 먼저 말하기는 테스트 후 직접 끌 수 있습니다.
자세한 배포 제외 항목은 [PRIVACY.md](PRIVACY.md)에 정리했습니다.

## 테스트와 현재 제한

자동 테스트 10개와 웹/Electron 빌드를 통과했습니다. 한국어 합성·인식·구독 AI 음성 왕복,
가상 마이크 입력/VAD/재생, 실제 선택 창의 화면 인식, 5초 자동 발화, 투명 펫 표시를 확인했습니다.

물리 마이크·스피커의 품질, 여러 모니터를 가로지르는 드래그, 카메라 입력,
장시간 실행과 설치형 패키지는 추가 확인이 필요합니다.
원본 Live2D SDK의 엄격한 전체 TypeScript 검사 오류는 남아 있으며 Vite 실행 빌드는 통과합니다.

```powershell
node --test tests/assistant.test.mjs
```

위 자동 테스트는 실제 계정 로그인이나 AI 호출이 필요하지 않습니다.

## 출처와 라이선스

- [Open-LLM-VTuber](https://github.com/Open-LLM-VTuber/Open-LLM-VTuber)
- [Open-LLM-VTuber-Web](https://github.com/Open-LLM-VTuber/Open-LLM-VTuber-Web)
- [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper)
- [Edge TTS](https://github.com/rany2/edge-tts)

이 프로젝트 전체를 단일 MIT 라이선스로 배포하는 것은 아닙니다.
서버 원본, 프런트엔드, Live2D SDK·샘플 모델에는 각각 다른 조건이 적용됩니다.
원문과 저작권 표시는 `licenses/`, `frontend/LICENSE`, Live2D SDK 폴더와 모델 ReadMe에 보존했습니다.
상업 이용·재배포 시 해당 조건을 확인하세요. [LICENSE.md](LICENSE.md)를 참고하세요.
