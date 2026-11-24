# 리버에이아이 서비스 공지 페이지 (river-ai-notice)

서버 점검·패치·이전 등으로 서비스를 잠시 내릴 때 `nakdong.river-ai.kr`에 접속한 사용자에게 안내를 보여 주는 임시 정적 페이지입니다. 현재 내용은 **사옥 이전에 따른 서버 이전** 안내입니다.
GitHub Pages로 호스팅하며, 가비아 DNS를 잠시 이 페이지로 돌려 두는 방식으로 사용합니다.

## 구성 파일

| 파일 | 역할 |
| --- | --- |
| `index.html` | 안내 페이지 본문 (그림, 문구, 스타일이 한 파일에 포함) |
| `404.html` | `index.html`과 동일한 내용. 어떤 경로로 들어와도 같은 안내가 보이게 함 |
| `logo.svg` | RiverAI 로고 (헤더와 파비콘) |
| `robots.txt` | 임시 페이지가 검색엔진에 수집되지 않도록 차단 |
| `.nojekyll` | GitHub Pages의 Jekyll 처리 비활성화 |

## 미리 보기 주소

https://hello-sb-cho.github.io/river-ai-notice/

지금은 확인용으로 사용자 도메인을 해제해 두었습니다. 실제 전환 전에 아래 0단계로 도메인을 다시 등록하세요.

## 안내 페이지 켜기 (가비아 DNS 전환)

0. 사용자 도메인 다시 등록 (DNS 변경 직전)

   ```bash
   echo "nakdong.river-ai.kr" > CNAME && git add CNAME && git commit -m "Set custom domain" && git push
   ```

   또는 GitHub 저장소 → Settings → Pages → Custom domain에 `nakdong.river-ai.kr` 입력 후 Save

1. 가비아 → **DNS 관리** → `river-ai.kr` 선택
2. 호스트 `nakdong`의 기존 **A 레코드 값을 메모**한 뒤 삭제
   - 값은 내부 문서에 따로 보관하세요 (공개 저장소에 적지 않음)
3. 레코드 추가

   | 타입 | 호스트 | 값 |
   | --- | --- | --- |
   | CNAME | `nakdong` | `hello-sb-cho.github.io.` |

4. 반영을 기다립니다 (보통 몇 분 ~ 1시간). 확인:

   ```bash
   dig +short nakdong.river-ai.kr
   ```

   `hello-sb-cho.github.io.`와 GitHub IP(`185.199.x.x`)가 나오면 반영된 것입니다.
5. GitHub 저장소 → **Settings → Pages**에서 **Enforce HTTPS**를 체크합니다. 인증서 발급이 끝나야 체크할 수 있습니다.

## 안내 페이지 끄기 (서비스 복구)

1. 가비아에서 `nakdong`의 CNAME 레코드 삭제
2. A 레코드를 다시 추가 (메모해 둔 기존 서버 IP, 새 사옥 서버 IP가 바뀌었다면 새 IP)
3. `dig +short nakdong.river-ai.kr`로 서버 IP가 나오는지 확인

필요하면 GitHub 저장소 → Settings → Pages에서 사용자 도메인을 지우거나 Pages를 꺼 둡니다.

## 문구 수정

`index.html`의 `<dl>` 안에서 대상 서비스, 작업 내용, 작업 기간, 문의처를 바꿀 수 있습니다.
현재 작업 기간: **2026.12.04(금) 09:00 ~ 2026.12.07(월) 18:00**

수정한 뒤에는 `404.html`도 같은 내용으로 맞춥니다.

```bash
cp index.html 404.html
git add -A && git commit -m "안내 문구 수정" && git push
```

push하면 1~2분 안에 GitHub Pages에 반영됩니다.

## 로컬에서 미리 보기

```bash
python3 -m http.server 8000
# http://localhost:8000
```

## 저장소

- GitHub: https://github.com/Hello-SB-Cho/river-ai-notice
- 문의: river_info@naver.com
