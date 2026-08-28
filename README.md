# 일본어 동영상 → 한국어 자막 변환기

OpenAI Whisper로 일본어(또는 다른 언어) 음성을 인식하고, Google Translate로 한국어로 번역하여 `.srt` 자막 파일을 생성하는 CLI 도구입니다.

## 설치

```bash
pip install openai-whisper ffmpeg-python deep-translator tqdm
```

`ffmpeg` 바이너리도 필요합니다.

```bash
apt install ffmpeg   # Debian/Ubuntu
brew install ffmpeg  # macOS
```

## 사용법

```bash
python video_to_srt.py 영상.mp4
```

### 옵션

| 옵션 | 설명 |
|---|---|
| `-m`, `--model` | Whisper 모델 크기 (`tiny`~`large-v3`, 기본값 `medium`) |
| `-o`, `--output` | 출력 `.srt` 경로 (기본값: 입력과 동일 경로) |
| `-l`, `--language` | 음성 언어 직접 지정 (예: `ja`, `en`). 생략 시 자동 감지 |
| `--force` | 캐시를 무시하고 음성 인식부터 재실행 |

## 동작 방식

1. ffmpeg으로 동영상에서 16kHz 모노 오디오 추출
2. Whisper로 음성 인식 (결과를 `.whisper.json`에 캐시, 재실행 시 재사용)
3. Google Translate로 한국어 번역
4. `.srt` 자막 파일 생성

캐시는 완료 여부와 동영상 길이 대비 커버리지를 검증해, 불완전한 캐시는 자동으로 무시하고 재인식합니다.
