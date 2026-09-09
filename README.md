# 나의 레시피 상자 (What to Eat)

내가 만든 레시피를 검색하고 가나다순으로 정리해서 보는 개인 레시피 앱입니다.

## 사용 방법

`index.html` 파일을 브라우저로 열면 바로 사용할 수 있습니다. 별도 설치나 서버가 필요 없습니다.

GitHub Pages로도 열 수 있습니다: 저장소 Settings > Pages에서 `main` 브랜치를 소스로 지정하면 `https://younsse-qor.github.io/what_to_eat/` 주소로 접속할 수 있습니다.

## 기능

- 레시피 추가 / 수정 / 삭제 (이름, 재료, 번호별 조리 순서, 태그)
- 태그마다 다른 파스텔 색이 자동으로 배정됨
- 가나다순 정렬 / 최근 추가순 정렬
- 이름·재료·태그 검색
- 태그별 필터링
- **Supabase 실시간 동기화** — 어느 기기에서 추가/수정/삭제해도 다른 기기에 자동 반영
- JSON 내보내기 / 가져오기로 백업 및 대량 이동

## 참고

레시피 데이터는 Supabase(무료 Postgres DB)에 저장되며, `index.html`에 공개용 anon 키가 포함되어 있습니다(로그인 기능이 없는 개인용 앱이라 URL과 키를 아는 사람은 누구나 읽기/쓰기가 가능한 구조입니다). 데이터베이스 스키마는 아래 SQL로 생성했습니다:

```sql
create table recipes (
  id text primary key,
  title text not null,
  tags text[] not null default '{}',
  ingredients text[] not null default '{}',
  steps text[] not null default '{}',
  created_at bigint not null
);

alter table recipes enable row level security;
create policy "Allow public access" on recipes for all using (true) with check (true);

alter publication supabase_realtime add table recipes;
```
