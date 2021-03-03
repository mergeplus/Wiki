# MeisterTask Intergration

마이스터태스크는 구글 캘린더, 슬랙(Slack), GitHub, 빗버킷(Bitbucket) 등 선호하는 도구들과 연동할 수 있습니다.

## 연동방법

- [Common](#common)
- [Github](#github)

### Common

![common_img1](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/common_img1.png)

![common_img2](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/common_img2.png)

먼저 대시보드 우측 상단 프로필 아이콘을 선택하여 `내 계정`로 이동합니다.
좌측메뉴에서`MeisterTask > 연동`을 선택하여 연동하고 싶은 툴을 선택하여 연동합니다.

### Github

Github을 연동하며 무엇이 좋냐? 
그것은 바로 특수 커밋메시지를 사용해서 Github에 Push하면 직접 MeisterTask에 들어가서 작업을 완료하거 체크리스트를 완료하지 않아도 자동으로 된다는 것 입니다!
굉장하죠?

MeisterTask 가이드에도 설명 되어있겠지만 연동하는 방법과 특수 커밋메시지를 사용하는 방법에 대해서 설명드리겠습니다.

![github_img1](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/github_img1.png)

먼저 연동할 Github 계정을 인증해주도로 합시다

![github_img2](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/github_img2.png)

Github 계정인증이 완료되면 위 화면처럼 `설정 추가` 버튼이 활성화 됩니다
`설정 추가` 버튼을 눌러줍시다

![github_img3](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/github_img3.png)

사용할 MeisterTask의 Project와 Github의 Repository를 연결해주면 끝입니다

특수 커밋메시지를 사용하는 방법을 설명드리기전에 TaskID에 대해서 간단하게 설명을 드리겠습니다
TaskID는 작업마다 유니크하게 생성되는 키입니다
이 TaskID를 알아야 특수 커밋메시지를 사용할 수 있습니다

![taskid_img1](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/taskid_img1.png)

이렇게 작업을 선택하시면 우측 하단에 `작업 ID`라고 표기되어있는데요 이것이 바로 TaskID 입니다
그리고 저 부분을 클릭하면 클립보드에 복사할 수 있어요!

자 이제 저 작업의 개발을 완료하시게 되면 모두 커밋을 하실텐데요

````
# 작업 완료
$ git commit -m 'message #goO8aDON'
# 체크리스트 첫번째 아이템 완료
$ git commit -m 'message #goO8aDON:1'
````

요렇게 커밋을 해주고 Github Repository에 푸시를 해주면 
짜잔!

![taskid_img2](https://github.com/mergeplus/Wiki/blob/main/MesiterTask/taskid_img2.png)

이렇게 자동으로 처리되고 우측 메뉴에 연동이라는 항목이 생기고
저 항목에서 어떤 특수 커밋메시지를 실행하였는지 확인할 수 있습니다
그리고 댓글에도 기록이 남고 해당 댓글에 Github으로 이동이 가능한 링크도 같이 포함되어 있습니다 :)



