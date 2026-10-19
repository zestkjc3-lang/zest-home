# zest-home (제스트에듀 홈페이지) 작업 메모

- **로컬:** `C:\Users\mophi\zest-home`
- **원격:** https://github.com/zestkjc3-lang/zest-home
- **라이브:** https://zestedu.co.kr (GitHub Pages)
- **미리보기:** `python -m http.server 5500` → http://127.0.0.1:5500/

---

## 최근까지 완료된 것

- [x] 주소 표기 `806, 807호`
- [x] 약속 카드 제목 = 아이콘 옆 배치
- [x] 공지·블로그 모바일 1열
- [x] 강사 프로필 닫기(X) 위치·흰색
- [x] 프로그램 카드 이미지 세로 ~2/3 (`aspect-ratio: 3/2`)
- [x] 모바일 히어로 배너 풀폭 + 화살표 숨김(스와이프)
- [x] 퀵타일: 블로그 / 약도(`#location`) 추가, 가로 간격 = 약속 카드 폭 정렬
- [x] 레벨테스트 예약 모달 폼 (학교·학년·이름·학생/학부모 휴대폰)
- [x] FormSubmit → `zestedu@naver.com` 메일 전송 연동

---

## 관리자 페이지 (배너·공지 카드·강사진 수정)

- **주소:** https://zestedu.co.kr/admin (Sveltia CMS, 서버 없음)
- **구조:** 메인 페이지의 아래 세 영역은 `data/` 폴더의 JSON을 읽어 그린다.
  - 메인 배너 → `data/banners.json`
  - 공지 · 블로그 카드 → `data/posts.json`
  - 강사진(카드 + 프로필 창) → `data/teachers.json`
- 관리자 화면에서 저장하면 브라우저가 GitHub API로 `main`에 직접 커밋 → GitHub Pages가 1~2분 뒤 반영.
- **설정 파일:** `admin/config.yml` (편집 항목), `admin/index.html` (Sveltia 스크립트, 버전 고정 `0.229.0`)
- 업로드한 사진은 `images/`에 저장된다.

### 로그인 토큰 발급 (사람마다 1회, 1년마다 갱신)

1. GitHub → Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate new token
2. Expiration: **1년(Custom)** / Repository access: **Only select repositories → `zest-home`**
3. Permissions → Repository permissions → **Contents: Read and write** (나머지는 건드리지 않음)
4. 발급된 토큰을 `/admin`의 **「액세스 토큰으로 로그인」**에 붙여넣기
   - 「GitHub(으)로 로그인」 버튼은 중계 서버가 없어 동작하지 않음 → 토큰 로그인만 사용
- 토큰은 비밀번호와 같다. 메신저·메일로 공유하지 않고, 공용 PC에서는 사용 후 로그아웃.
- 직원 계정은 저장소 Settings → Collaborators로 초대 + GitHub 2단계 인증 필수.
- 유출 의심 시: 해당 토큰 Revoke → 잘못된 커밋은 `git revert`.

### 수정 방법

관리자 화면 → **메인 페이지** → 아래 중 하나 선택 → 항목 추가/삭제/드래그로 순서 변경 → 저장.

- **메인 배너:** 가로 사진(약 1024×716 비율) 권장. 비율이 다르면 가운데 기준으로 잘림.
- **공지 · 블로그 카드:** 정사각형 사진 권장. 링크에 `https://...`를 넣으면 새 창으로 열림.
- **강사진:** 영문 아이디는 강사끼리 겹치지 않게. 이름 뒤 `T`는 자동으로 붙음.

### 로컬에서 미리 해보기 (토큰·커밋 없이)

1. 저장소 폴더에서 `python3 -m http.server 5500` 실행
2. **크롬/엣지**로 http://127.0.0.1:5500/admin → **「로컬 저장소로 작업」** → `zest-home` 폴더 선택
3. 수정·저장하면 내 컴퓨터의 `data/*.json`, `images/`만 바뀐다 → http://127.0.0.1:5500 새로고침해서 확인
4. 마음에 안 들면 `git checkout -- data/ && git clean -n images/`로 확인 후 되돌리기

---

## 해야 할 일

### 0. 관리자 페이지 첫 확인

- [ ] 푸시 후 토큰 발급 → `/admin` 로그인 → 배너 하나 순서 바꿔 저장 → 라이브 반영 확인 → 원복

### 1. FormSubmit 최초 활성화 (필수)

- [ ] 사이트에서 테스트 예약 1회 제출
- [ ] `zestedu@naver.com`으로 온 FormSubmit 확인 메일 링크 클릭
- [ ] 이후 예약이 메일로 정상 수신되는지 확인

### 2. 예약 → 학원 휴대폰 문자/알림 (미구현)

GitHub Pages(HTML만)로는 SMS 직접 발송 불가. 아래 중 하나 선택 후 연동.

- [ ] **방식 결정:** SMS(알리고/쿨에스엠에스 등) vs 카카오 알림톡 vs Make/Zapier 중계
- [ ] 문자·알림톡 서비스 가입 + 발신번호/채널 등록
- [ ] 중계 구성: `폼 제출 → (Make/Zapier 또는 서버리스) → 문자 API → 학원 번호`
- [ ] 수신 번호(학원 휴대폰) 확정 후 테스트 발송

권장 골격:

```
GitHub Pages(폼) → FormSubmit/웹훅 → Make·Zapier 또는 서버리스 → SMS/알림톡 API → 학원 폰
```

### 3. 기타 점검

- [ ] 라이브에서 레벨테스트 모달·퀵타일·모바일 배너 재확인
- [ ] Cursor 내장 브라우저는 우측 하단 플로팅 버튼이 잘릴 수 있음 → 실기기/일반 브라우저로 확인

---

## 새 채팅에서 이어갈 때

이 파일을 열고 예를 들어 이렇게 요청하면 됩니다.

> `@homepage.md` 기준으로 예약 문자(또는 알림톡) 연동부터 이어서 진행해줘.
