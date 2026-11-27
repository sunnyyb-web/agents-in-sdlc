---
title: "레슨 12 - GitHub Pages로 사이트 배포하기"
description: "GitHub Copilot app을 사용해 Pages 워크플로를 구성하고, 풀 리퀘스트를 검토한 뒤, 게시된 Space Quiz를 확인합니다."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Space Quiz를 GitHub Pages에 배포하여 공개적으로 이용할 수 있도록 합니다. Copilot은 워크플로를 준비하고 배포 과정을 안내할 수 있지만, 변경 사항을 검토하고 사이트가 게시되었는지 확인하는 책임은 사용자에게 있습니다.

이 레슨에서는 다음을 수행합니다.

- Copilot에 GitHub Pages 배포 구성을 요청합니다.
- 병합하기 전에 워크플로와 풀 리퀘스트를 검토합니다.
- 배포 과정을 따라가고 게시된 사이트를 확인합니다.

## 시나리오

Space Quiz는 이미 GitHub 리포지토리에 있지만, 누구나 간단히 이용할 수 있는 방법은 아직 없습니다. 서버나 런타임 종속성을 추가하지 않고 GitHub Pages로 단일 파일 사이트를 게시하면 공개 URL에서 이용할 수 있습니다.

## Copilot에 배포 준비 요청하기

> [!WARNING]
> 풀 리퀘스트를 만들면 코드가 커밋되고 푸시될 수 있습니다. 이 프롬프트를 보내기 전에 승인 설정과 대상 리포지토리를 검토합니다. **Approve all**에서는 별도의 확인 프롬프트가 표시된다고 기대하지 마십시오.

`space-quiz`의 Copilot app 세션에서 다음 프롬프트를 보냅니다.

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## 워크플로와 풀 리퀘스트 검토하기

1. 리포지토리에 README가 없는 경우에만 추가되었는지 확인합니다.
2. 워크플로의 빌드 및 배포 단계, 트리거, 권한을 포함해 풀 리퀘스트 변경 사항을 검토합니다.
3. 병합되지 않은 풀 리퀘스트가 아니라 기본 브랜치에서 워크플로가 배포되는지 확인합니다.
4. 필요한 수정을 요청하고 업데이트된 diff를 검토합니다. 병합을 승인하기 전에 필수 풀 리퀘스트 검사가 통과할 때까지 기다립니다.

## 병합과 배포 승인하기

> [!WARNING]
> 다음 프롬프트는 에이전트가 검토한 풀 리퀘스트를 병합하고, 리포지토리의 Pages 설정을 변경하고, 사이트를 게시하도록 승인합니다. 검토를 완료하고 리포지토리와 풀 리퀘스트를 확인한 후에만 보냅니다. 승인 설정에 따라 별도의 확인 프롬프트 없이 이러한 작업이 진행될 수 있습니다.

`<PR_NUMBER>`를 검토한 풀 리퀘스트 번호로 바꾼 다음 별도의 프롬프트로 보냅니다.

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz 풀 리퀘스트 병합, Build website·Upload artifact·Deploy to Pages 단계가 성공한 GitHub Actions 배포 실행, yourname.github.io/space-quiz/에서 열린 Space Quiz를 보여 주는 3단계 다이어그램](../../../_images/first-steps-app-pages-deploy.svg)

## 배포 확인하기

1. GitHub Actions 배포 실행이 성공할 때까지 확인합니다.
2. 보고된 URL을 열어 Space Quiz가 로드되는지 확인합니다.

에이전트가 Pages를 활성화하거나 풀 리퀘스트를 병합할 수 없다면 제공된 구체적인 지침을 따릅니다. 배포가 성공하고 실제 사이트가 로드될 때까지 작업이 완료된 것으로 처리하지 않습니다.

## 요약 및 다음 단계

Space Quiz를 실제 사이트로 게시하고 에이전트의 보고에 의존하지 않고 배포를 확인했습니다. 다음으로 [레슨 13: 검토 및 다음 단계][next-lesson]를 진행합니다.

[next-lesson]: ../13-review/
