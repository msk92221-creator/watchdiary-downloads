# WatchDiary - 내 넷플릭스 시청 기록 (크롬/엣지 확장)

공유 넷플릭스 계정에서 **내가 본 작품의 진도만 자동으로 기록**. 앱 없이 확장 하나만으로 작동.

## ✨ 특징
- 넷플릭스 재생 시 **자동으로 진도/재생 위치 기록**
- 확장 아이콘 클릭 → 내 시청 목록 팝업
- 회차별 상세 위치 (mm:ss) 표시
- 검색 / 데이터 내보내기(JSON) / 전체 삭제
- 수동 업데이트 확인 / 기존 중복 작품 목록 안전 정리
- 모든 데이터는 브라우저에만 저장 (`chrome.storage`) — 외부 전송 없음

## 📦 설치 (엣지/크롬)
1. `edge://extensions` (또는 `chrome://extensions`) 접속
2. **개발자 모드** 켜기
3. **"압축해제된 확장 로드"** 클릭
4. 이 `extension/` 폴더 선택

## 📺 사용
1. netflix.com 접속 → 작품 재생
2. 자동으로 진도가 기록됨 (회차가 감지된 경우)
3. 확장 아이콘 클릭 → 시청 목록 확인
4. 작품 클릭 → 회차별 상세 위치 펼침

## 🔍 감지 원리
- `video.currentTime` (표준 API - 안정적) 으로 재생 위치 추출
- `[data-uia="player-title"]` 등에서 제목/시즌/에피소드 파싱 (다중 폴백)
- 5초 이상 위치 변화 시 저장 (과도한 저장 방지)

## ⚠️ 한계
- 넷플릭스 **웹 버전에서만** 작동 (윈도우 앱/TV 불가)
- 넷플릭스가 DOM을 변경하면 회차 감지가 깨질 수 있음 (재생 위치는 안정적)
- 시즌/에피소드가 식별된 시리즈만 회차별 기록 (영화는 위치만)
- 티빙/웨이브는 별도 확장 필요

## 📁 파일 구조
```
extension/
├── manifest.json    # MV3 선언
├── content.js       # netflix.com DOM 감지
├── background.js    # chrome.storage 저장
├── popup.html       # 진도 목록 UI
├── popup.css        # 다크 테마 스타일
└── popup.js         # 목록 로드/검색/내보내기
```

## 💾 데이터 구조 (chrome.storage.local)
```json
{
  "shows": {
    "오징어게임": {
      "title": "오징어게임",
      "platform": "netflix",
      "latestSeason": 2,
      "latestEpisode": 5,
      "lastWatchedAt": 1730000000000,
      "episodes": {
        "2-5": {
          "season": 2, "episode": 5,
          "positionSec": 1395, "positionText": "23:15",
          "watchedAt": 1730000000000
        }
      }
    }
  }
}
```

