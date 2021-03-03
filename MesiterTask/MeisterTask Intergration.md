# MeisterTask Intergration

마이스터태스크는 구글 캘린더, 슬랙(Slack), GitHub, 빗버킷(Bitbucket) 등 선호하는 도구들과 연동할 수 있습니다.

## 연동방법

- [Common](#common)
- [GitHub](#github)

### Common

![common_img1](/Users/tophun/Desktop/common_img1.png)

![common_img2](/Users/tophun/Desktop/common_img2.png)

먼저 대시보드 우측 상단 프로필 아이콘을 선택하여 `내 계정`로 이동합니다.
좌측메뉴에서`MeisterTask > 연동`을 선택하여 연동하고 싶은 툴을 선택하여 연동합니다.

### GitHub

![github_img1](/Users/tophun/Desktop/github_img1.png)

연동할 GitHub 계정을 인증합니다.

![github_img2](/Users/tophun/Desktop/github_img2.png)

GitHub 계정인증이 완료되면 위 화면처럼 `설정 추가` 버튼이 활성화 됩니다.
`설정 추가` 버튼을 눌러줍니다.

![github_img3](/Users/tophun/Desktop/github_img3.png)

사용할 MeisterTask의 Project와 GitHub의 Repository를 연결해줍니다.
이렇게 프로젝트와 레파지토리를 설정하면 특수커밋을 이용하면 작업을 완료처리하거나 작업의 체크리스트를 완료처리 할 수 있습니다.

````
# 작업 완료
$ git commit -m 'message #TaskId'
# 체크리스트 첫번째 아이템 완료
$ git commit -m 'message #TaskId:1'
````

위와 같이 특수 커밋메시지를 사용하면 마이스터테스크에 직접 컨트롤하지 않고 GitHub에 Push하는 것 만으로 TaskID에 해당하는 작업 또는 작업의 체크리스트를 편리하게 완료처리 할 수 있다:)





