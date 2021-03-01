# Git Flow

git-flow는 Vincent Driessen의 브랜칭 모델을 위한 고수준 저장소 작업을 제공하는 git의 확장이다[1].

## Main branch

Git Flow에서 `master` 브랜치는 언제나 사용자에게 친숙해야 하며, 이와 병렬로 함께 나가는 `develop` 브랜치와 함께 무한한 생명주기를 가진다. 

- `origin/master`:  HEAD의 소스 코드가 Production Release 상태를 반영하고 있어야 한다.
- `origin/develop`: HEAD의 소스 코드가 항상 다음 릴리스의 최신 개발 변경 사항을 포함하는 상태를 반영해야 한다.

`develop` 브랜치가 출시해도 괜찮은 안정적인 상태가 되었을 경우 `master` 브랜치에 머지해야하며, 이때 release number로 태깅을 해주도록 한다. 이렇게 매번 `master` 브랜치에 머지 될 때마다 새 버전의 프로젝트 release로 본다.

Git hook 스크립트를 이용하여 `master` 브랜치에 커밋을 할 때마다 운영 중인 소프트웨어에 자동으로 반영되게 할 수도 있다.

## Supporting branches

다양한 팀원들과의 협업을 위해 다양한 브랜치에서 병렬로 작업할 수 있는 개발 모델을 사용할 수 있다. 

위의 `master`, `develop` 브랜치와 달리 수명이 제한적이며, 사용이 완료된 브랜치는 결국 제거된다. 

일반적으로 사용되는 브랜치는 아래와 같다

- [Feature branch](#feature-branch)
- [Release branch](#release-branch)
- [Hotfix branch](#hotfir-branch)

이러한 브랜치들은 문명한 목적을 가지고 있어야 하며, 분기 및 병합에 대상이 명확해야한다. 

### Feature branch

- `develop` 브랜치로 부터 생성된다.
- `develop` 브랜치로 머지되어야 한다.
- 네이밍 컨벤션:  `master`, `develop`, `release/*`, `hotfix/*`을 제외하여야 한다.

Feature 브랜치는 **미래에 있을 업데이트에 대한 새로운 기능 개발**을 위해 사용된다. 이 브랜치가 생성되었다고 해서 해당 기능이 언제 반영될지 바로 정해지는 것은 아니다. 이 브랜치는 정말 해당 기능이 개발 단계에 있지만, 추후 `develop`에 머지될 것이라는 것을 나타낸다. (마찬가지로 반영될 수도 있고, 아닌 경우 제거될 수도 있다.)

> 새로운 feature 브랜치 생성

```bash
$ git checkout -b myfeature develop
# "myfeature"브랜치 생성 후 전환
```

> 완료된 feature 브랜치를 develop 브랜치에 합치기

```bash
$ git checkout develop
# 'develop' 브랜치로 전환
$ git merge --no-ff myfeature # --no-ff 플래그는 머지 시 새로운 커밋을 생성하도록 함
# Updating ea1b82a..05e9557
# (Summary of changes)
$ git branch -d myfeature
# 머지 후 해당 브랜치를 제거
$ git push origin develop
```

—no-ff 플래그를 사용하면 fast-forward(빠르게?) 머지할 수 있으며, 이전 feature에 대한 히스토리를 그룹화하여 남길 수 있도록 해준다. 아래의 그림은 —no-ff 플래그의 사용 여부에 따른 차이를 보여준다

![image1](https://github.com/mergeplus/Wiki/blob/main/Git/gitflow_img1.png)

오른쪽의 경우 어떤 커밋들이 병합되었는지에 대한 git history를 확인하는 것이 불가능하다. 따라서 이 경우에는 모든 로그 메시지를 분석하여 어떤 feature에 대한 작업이 이루어졌는지를 수동으로 확인해야 하는 불편함이 있다.

### Release branch

- `develop` 브랜치로부터 분기 될 수 있다.
- `develop` 브랜치 및 `master` 브랜치로 머지되어야 한다.
- 네이밍 컨벤션: `release/*`를 사용한다.

`release` 브랜치는 **새로운 production release에 대한 준비**를 위하여 사용하는 브랜치이다. 마이너 버그 픽스, 버전 변경(프로젝트) 등의 메타데이터 변경에 대한 것도 이 브랜치에서 수행한다. 

`develop` 브랜치에서 `release` 브랜치로 분기하는 최적의 상황은 새로운 업데이트에 대한 요구가 거의 다 반영 되었을 경우이다. 최소한 새 릴리스를 위해 개발된 `feature` 브랜치들이 `develop` 브랜치에 머지 된 상태여야 한다.

`develop` 브랜치는 다음 업데이트에 대한 내용을 반영하고 있지만, 어떤 버전으로 출시될지에 대한 정보는 불분명하다. 따라서. `develop` 브랜치에 반영된 기능의 범위(?)에 따라 `release` 브랜치 생성 시에 버전을 정해주도록 하는 것이 좋다.

> Release 브랜치 생성

`release` 브랜치는 `develop` 브랜치로부터 생성된다. 예를 들어 현재 버전이 1.1.5이고 다음 큰 규모의 업데이트를 준비하고 있다 가정하며, `develop` 브랜치가 릴리스하기 위한 준비가 되어 있을 경우 우리는 다음 버전을 1.2(1.1.6 또는 2.0이 아닌)로 정할 수 있을 것이다. 따라서 새로운 버전 넘버를 반영하여 아래와 같이 브랜치를 생성할 수 있다.

```bash
$ git checkout -b release/1.2 develop
# 새로운 브랜치 "release/1.2" 전환
# 위 명령어 이후 우리는 Xcode 또는 Android Studio에서 새로운 버전에 대한 버전넘버 및 빌드 번호를 1.2로 수정한다.
# Files modified successfully, version bumped to 1.2.
$ git commit -a -m "Bumped version number to 1.2"
# [release/1.2 74d9424] Bumped version number to 1.2
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

위의 명령어를 통해 새로운 `release` 브랜치를 생성 및 커밋까지 진행하였다면 해당 브랜치에 대한 작업을 수행하면 된다. QA 등을 통해 발견된 bug-fix 등이 이 브랜치에서 수행 될 수 있다. 큰 규모의 수정(기능 추가 등)은 절대 이 브랜치에서 수행하지 않도록 주의하자. 

이러한 작업이 모두 완료 되었을 경우 다시 `develop` 브랜치로 머지하도록 한다.

> Release 브랜치 끝내기

브랜치가 실제 배포 가능한 상태까지 작업이 완료되었다면, 해당 브랜치를 `master` 브랜치에 머지하도록 한다(물론 위에서 서술한 것 처럼 `develop` 브랜치에더 머지해주어야한다.) 

`master` 브랜치에 `release` 브랜치를 머지하였다는 것은 다음 업데이트가 반영됐음을 의미한다. (git hook을 통해 자동 배포도 가능하다고 이야기 했었음.)

머지 후 해당 업데이트 버전으로 `tag`를 작성하면 해당 버전에 대한 업데이트가 마무리 되고 다음 업데이트를 준비할 수 있다.

아래의 코드는 1.2버전에 대한 `release` 브랜치 머지 및 태깅하는 명령어이다. (태깅은 일반적으로 릴리즈시에 작성한다고 나와있으나, git 또는 프로젝트에서 어떤 의미로 사용되는지는 알아보아야함)

```bash
- Master 브랜치에 머지 및 태깅
$ git checkout master
# 'master' 브랜치로 전환
$ git merge --no-ff release-1.2
# release 브랜치 머지
$ git tag -a 1.2

- Develop 브랜치에 머지
$ git checkout develop
# 'develop' 브랜치로 전환
$ git merge --no-ff release-1.2
# 머지

- 위의 두 과정을 마친 후 release 브랜치를 제거해주도록 하자
$ git branch -d release-1.2
# Deleted branch release-1.2 (was ff452fe).
```

### Hotfix branch

- `master` 브랜치로부터 분기 될 수 있다.
- `develop` 및 `master` 브랜치로 머지되어야 한다.
- 네이밍 컨벤션: `hotfix/*`를 사용한다.

`hotfix` 브랜치는 의도치 않게 새로운 production release를 만든다는 점에서 `release` 브랜치와 비슷한 면이 있다. 이 브랜치는 현재 배포된 버전에서 **긴급하게 발견된 오류 또는 의도치 않은 상황에 대한 수정**이 필요할 경우 사용한다.

![image2](https://github.com/mergeplus/Wiki/blob/main/Git/gitflow_img2.png)

위의 그림에서 확인할 수 있듯이, 릴리즈 브랜치와 비슷하지만 정말 긴급한 수정사항이 있을 떄 `master` 브랜치에서 분기하여 작업 후 `develop` 및 `master` 브랜치로 머지하게 된다.

> Hotfix branch 생성하기

예를들어 1.2 버전에 대한 hotfix를 만들어야한다면, `master` 브랜치에서 문제가 발생한 버전에 브랜치를 생성하여  체크아웃한다. 

```bash
$ git checkout -b hotfix/1.2.1 master
# 새 브랜치 "hotfix/1.2.1" 생성 및 전환
# 1.2.1 버전으로 수정(Xcode 또는 AndroidStudio)
$ git commit -a -m "Bumped version number to 1.2.1"
# [hotfix/1.2.1 41e61bb] Bumped version number to 1.2.1
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

브랜치 생성 후 버전을 올리는 것을 절대 잊지 말고 작업을 진행하도록 하자.

위의 작업을 통해 새 `hotfix` 브랜치 생성 및 버그 수정에 대한 커밋이 모두 완료되었다면, 다시 `master` 및 `develop` 브랜치로 머지해주도록 한다.

```bash
- 마스터 브랜치에 머지 및 태깅
$ git checkout master
# 'master'로 전환
$ git merge --no-ff hotfix/1.2.1
# hotfix 머지
$ git tag -a 1.2.1
# 해당 버전에 대한 태그 지정

- 디벨롭 브랜치에 머지
$ git checkout develop
# 'develop'로 전환
$ git merge --no-ff hotfix/1.2.1
# hotfix 머지

- 브랜치 제거
$ git branch -d hotfix/1.2.1
# Deleted branch hotfix/1.2.1 (was abbe5d6)
```

**여기서 만약 버그 픽스 이전에 이미 해당 업데이트 버전에 대한 `release` 브랜치가 있을경우** hotfix에 대한 변경 사항을 `develop` 브랜치가 아닌 `release` 브랜치에 머지하도록 한다. 이렇게 하면 hotfix에 대한 수정사항 도 포함하여 `release` 브랜치 머지시 `develop` 및 `master` 브랜치에 포함될것이다.

### Reference

[1]. [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/)
