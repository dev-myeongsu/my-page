<img src="https://img.shields.io/badge/Git-white?style=for-the-badge&logo=git&logoColor=F03C2E"> <img src="https://img.shields.io/badge/Github-black?style=for-the-badge&logo=github&logoColor=#181717">

## Git 브랜치 전략

<details>
<summary>① main</summary>
ROM EM
- 과거에 배포된 코드 또는 앞으로 배포될 최종 단계의 코드가 관리되는 곳
- 태그와 함께 버전 정보가 기록됨
- 원격저장소(origin/main)에서관리

</details>

<details>
<summary>② develop</summary>

- 분기되어 나온 브랜치 : main
- 병합할 브랜치 : main, release
- 다음에 배포할 프로그램의 코드를 관리
- 배포할 프로그램의 기능이 준비되면 release 브랜치로 병합되어 검사하거나, main으로 병합되어 배포 버전으로 태그를 부여받음
- 원격저장소(origin/develop)에서 관리
- 브랜치 생성 예시

  ```
  git checkout -b develop
  ```

</details>

<details>
<summary>③ feature</summary>

- 분기되어 나온 브랜치 : develop
- 병합할 브랜치 : develop
- 브랜치 이름 규칙 : feature/이름
- 토픽 브랜치라고도 부르며, 다음 배포에 추가될 기능을 개발하는 브랜치
- 기능이 완성되면 develop으로 병합됨
- 반드시 develop으로 병합할 필요는 없음. 예를 들어 기능이 필요하지 않거나 만족스럽지 않으면 병합하지 않고 삭제 가능
- 보통 개발자의 로컬저장소에 위치하며 원격저장소에 푸시하지 않음
- 브랜치 생성 예시 : func1이라는 이름의 feature 브랜치를 생성할 때

  ```
  git checkout -b feature/func1
  ```

- 작업 후 브랜치 병합, 삭제 예시 : --no-off 옵션으로 병합

  ```
  git checkout develop
  git merge --no-ff feature/func1
  git branch -d feature/func1
  git push origin develop
  ```

</details>

<details>
<summary>③ release</summary>

- 분기되어 나온 브랜치 : develop
- 병합할 브랜치 : develop, main
- 브랜치 이름 규칙 : release/버전 정보
- 기능이 완성된 develop 브랜치의 코드를 병합해서 배포를 준비하는 브랜치
- 배포에 필요한 준비, 품질 검사, 버그 수정 등을 진행
- release 브랜치로 인해 develop 브랜치는 다음 배포에 필요한 기능에 집중할 수 있음
- release 브랜치가 배포할 상태가 되면, main으로 통합
- 이후 main 브랜치에 만들어진 커밋에 태그를 추가함
- 배포 후 release 브랜치가 필요 없으면 삭제(선택 사항)
- 브랜치 생성 예시 : v0.1 버전의 배포를 위한 브랜치 생성

  ```
  git checkout -b release/v0.1
  ```

- 작업 후 브랜치 병합, 태깅, 삭제 예시 : main, develop에 통합

  ```
  git checkout main
  git merge --no-ff release/v0.1
  git tag -a v0.1
  git checkout develop
  git merge --no-ff release/v0.1
  git branch -d release/v0.1
  ```

</details>

<details>
<summary>⑤ hotfix</summary>

- 분기되어 나온 브랜치 : main
- 병합할 브랜치 : develop, main
- 브랜치 이름 규칙 : hotfix/버전 정보
- 기존 배포한 버전에 문제를 해결하기 위한(버그 수정 등) 브랜치
- 브랜치는 문제가 발생한 main의 버전으로부터 생성
- 문제 해결 후에는 main과 develop에 각각 병합을 진행
- 만약 release 브랜치가 삭제되지 않았다면 release 브랜치에도 병합 진행
- 병합 후 브랜치는 삭제
- 브랜치 생성 예시 : v0.1.1 버전으로 문제를 해결하기 위한 브랜치 생성

  ```
  git checkout -b hotfix/v0.1.1
  ```

- 작업 후 브랜치 병합, 삭제 예시 : main, develop에 병합

  ```
  git checkout main
  git merge --no-ff hotfix/v0.1
  git tag -a 0.1.1
  git checkout develop
  git merge --no-ff hotfix/v0.1.1
  git branch -d hotfix/v0.1.1
  ```

  </details>

---

## 커밋 메시지 구조

```
<타입(type)>[범위(scope, 선택 사항)]: <제목(subject or description)>
<한줄 공백(BLANK LINE)>
<본문(body, 선택 사항)>
<한줄 공백(BLANK LINE)>
<꼬리말(footer, 선택 사항)>
```

- feat : 새로운 기능의 추가, 삭제, 변경 등(제품 코드 수정)
- fix : 버그 수정(제품 코드 수정)
- docs : 문서 추가, 삭제, 변경(제품 코드 수정)
- style : 포맷, 정렬 등의 변경과 같이 스타일과 관련된 수정(제품 코드가 수정되지만 동작에 영향 없음)
- refactor : 코드 전면 수정(리팩토링, 제품 코드 수정)
- test : 시험을 위한 코드 추가, 삭제, 변경 등(제품 코드 수정 없음)
- chore : .gitignore 파일처럼 외부 사용자가 관심 없는 파일이나 빌드, 패키지 매니저, CI 등과 관련된 파일의 변경(제품 코드 수정 없음)

꼬리말 부분에는 이슈 트래커(issue tracker)와 함께 사용할 때 해결한 이슈나 참고할 부분을 명시해 주면 좋습니다.

```
Resolves: #123
See also: #456, #789
```

---
