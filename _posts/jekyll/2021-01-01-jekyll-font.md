---
title: Github 블로그 커스터마이징 - font
toc: true
categories:	
    - Blog_Customizing
tags:
- github.io
- 깃허브블로그
last_modified_at: 
gallery2:
  - url: /assets/images/test.jpg
    image_path: /assets/images/test.jpg
    alt: "Black and grays with a hint of green"
  - url: /assets/images/test.jpg
    image_path: /assets/images/test.jpg
    alt: "Made for open text placement"
---

### Github 블로그 커스터마이징 - font편

 **minimal-mistakes** 테마에서 기본으로 제공하는 폰트의 가독성이 떨어진다고 느껴지면, 폰트를 변경해 가독성을 높일 수 있다. 이번편에서는 font를 변경하는 아주 간단한 방법을 알아보려한다.



사람마다 차이는 있겠지만, font 변경 후의 **가독성**이 훨씬 뛰어나보인다.

**[변경 전]**

![font(default)](https://user-images.githubusercontent.com/49560745/103432766-b1c7db80-4c28-11eb-90fb-b2bc4fc66118.JPG)

**[변경 후]**

![font(modified)](https://user-images.githubusercontent.com/49560745/103432771-bab8ad00-4c28-11eb-9b89-cb42ac40fcd7.JPG)

 이제, **google Fonts**를 활용해 font를 변경해보자.

# 1) google Fonts에서 원하는 font 선택하기

추천하는 서체는 위와 동일한 **[Noto Sans KR](https://fonts.google.com/specimen/Noto+Sans+KR?sidebar.open=true&selection.family=Noto+Sans+KR:wght@100)**이다. 이외에도 다양한 font를 적용할 수 있다. **[Google Fonts 보러가기](https://fonts.google.com/?sidebar.open=true&selection.family=Noto+Sans+KR:wght@100)**

![image](https://user-images.githubusercontent.com/49560745/103432826-9f01d680-4c29-11eb-9467-b94b34372fd0.png)

위 링크를 통해 접속하면, 사진처럼 font의 굵기를 선택할 수 있다. 원하는 스타일을 찾아 **Select this style**을 클릭하자.



# 2) font 적용하기

![image](https://user-images.githubusercontent.com/49560745/103432843-015ad700-4c2a-11eb-8688-046bf975de16.png)

그 다음으로 font를 **적용**시키는 방법이다. **link**를 거는 방법과 **import** 두 가지 방식이있는데, jekyll에서는 두 번째 **import** 방식을 추천한다. 

```scss
---
# Only the main Sass file needs front matter (the dashes are enough)
---

@charset "utf-8";

@import "minimal-mistakes/skins/{{ site.minimal_mistakes_skin | default: 'default' }}"; // skin
@import "minimal-mistakes"; // main partials
@import url('https://fonts.googleapis.com/css2?family=Nanum+Brush+Script&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@100&display=swap');
```

**style**태그를 제외하고 복사 한 후에 **/assets/css/main.scss** 파일에 붙여 넣어주자.



```scss
$global-font-family: 'Noto Sans KR', sans-serif;
$header-font-family: 'Noto Sans KR', sans-serif;
$caption-font-family: 'Noto Sans KR', sans-serif;
```

**font-family**는 **/_sass/minimal-mistakes/_variable.scss** 경로에 붙여 넣어주면, font 적용이 완료된다.

**$global-font-family**는 게시글의 본문,

**$header-font-family**는 게시글의 헤더,

**$caption-font-family**는 프로필에 해당한다.





### Reference

- [폰트 변경 - 취미로 코딩하는 개발자](https://devinlife.com/howto%20github%20pages/set-font/)




