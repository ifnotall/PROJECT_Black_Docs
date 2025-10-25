# 00_DevPipeline.md  
**Project Black – Docs_v3.6 기준**

---

## 🎯 목적
문서·데이터 수정이 코드 변경까지 자동 추적되도록 하는  
**문서-코드 협업 파이프라인 표준**.

---

## 🧩 파이프라인 구조

Excel (.xlsx) → CSV → JSON → Unity

- 원본 수정: `/Docs/DataTables/DataTables_Template.xlsx`
- 변환 저장: `/Docs/DataTables/CSV/`
- 최종 참조: `/Assets/_Project/DataTables/JSON/`

📘 **문서 수정 규칙**
- 기획 변경: `/Docs/*.md`
- 데이터 변경: `/Docs/DataTables/*.xlsx`
- 코드 반영 요청: 채팅에서 `#Update:` 명령 사용

---

## 📚 명명 규칙
| 유형 | 예시 | 설명 |
|------|------|------|
| ID | `card_ace_spade` | snake_case, 전역 고유 |
| Prefab | `pf_card_ace` | 오브젝트 단위 |
| Sprite | `sp_card_ace` | 시각 리소스 |
| Sound | `sfx_hit`, `bgm_table` | 효과음 / 배경음 |
| Script | `CardSystem.cs` | 클래스별 단일 책임(SRP) |

---

## 🔧 #Update 보고 규칙

### 📍 목적  
문서나 테이블 수정 후, GPT가 변경사항을 추적해  
관련 문서·코드·데이터를 자동으로 갱신하도록 하는 명령.

---

### 🧩 사용법

#### 1️⃣ 파일 업로드 후 보고
```
#Update: 02_GameplayRules.md
블랙잭 배당 x2.0 → x2.5로 변경
```

#### 2️⃣ 데이터 테이블 변경
```
#Update: CardTable.csv
블랙잭 보너스 배당 x2.0 → x3.0으로 조정
```

#### 3️⃣ 말로만 변경 (파일 없이)
```
#Update: 블랙잭 배당 x2.0 → x3.0으로 변경 (02_GameplayRules.md)
```

---

### ⚙️ 자동 처리 흐름

1. 변경 감지  
2. 영향 문서 추적  
   → 예: Balance_Tuning.md, GameplayRules.md  
3. 영향 코드 제안  
   → 예: CardSystem.cs, RewardSystem.cs  
4. 문서 반영  
   → 변경 수치 갱신 + 날짜/버전 태그  
5. 로그 기록  
   → `08_ChangeLog.md` 자동 추가

---

### ✅ 실제 예시

입력:
```
#Update: CardTable.csv
블랙잭 보너스 배당 x2.0 → x3.0
```

자동 결과:
```
변경 감지: CardTable.csv
영향: 04_Balance_Tuning.md, 02_GameplayRules.md, CardSystem.cs
제안 패치:
> CardSystem.cs
  case "MultiplyScore":
      score *= 3.0f; // was 2.0f
로그:
| 2025-10-24 | v3.6 | CardTable.csv | 블랙잭 배당 x3.0으로 조정 (#Update) |
```

---

## 🧱 협업 규칙 요약

| 단계 | 형의 액션 | GPT의 처리 |
|------|-------------|-------------|
| ① | 문서/데이터 수정 후 업로드 | 변경 감지 |
| ② | `#Update:` 명령 입력 | diff 분석 & 영향 추적 |
| ③ | 결과 확인 | 수정안 자동 생성 + changelog 기록 |

---

## 📎 참고
- 형은 기획과 아트를 담당, GPT는 시스템·코드·데이터 연동 담당  
- 모든 수정 기록은 `/Docs/08_ChangeLog.md`에 자동 반영  
- 형이 “#Update:”로 명령을 내리면, 나는 항상 **“영향 문서 + 코드 패치 제안”**까지 응답

---

🧠 **기억 포인트**
> 문서 수정 → `#Update:` 보고 → 내가 코드·문서·테이블 모두 갱신  
> Excel 수정 → CSV/JSON 자동 변환  
> 모든 결과는 `/Docs_v3.6` 안에 정리
