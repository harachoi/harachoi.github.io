---
title: Github 블로그 커스터마이징 - 카테고리
toc: true
categories:	
    - Blog_Customizing
tags:
- github.io
- 깃허브블로그
last_modified_at: 
---



### Github 블로그 커스터마이징 - 카테고리&태그편

jekyll를 기반으로 깃허브 블로그를 커스터마이징해보자. 

이번편은 

**1) 네비게이션바에 카테고리 항목을 추가해,** 
**게시한 글을 범주화하고**

**2) 글에 태그를 추가해,** 

태그로 해당하는 글을 식별하고, 접근할 수 있도록 한다. 

테마는 **minimal-mistakes**를 적용했다. 



##### 1. 네비게이션바 생성

![image](https://user-images.githubusercontent.com/49560745/103127642-e3dfb780-46d5-11eb-86b3-5c7e2d2057af.png)

네비게이션바(위의 Categories)를 통해 카테고리 페이지에 접근하고 싶은 경우에 한해 아래와 같은 작업을 진행하자.

작업은 간단하다. 카테고리를 네비게이션바에 추가해주면 된다. 

네비게이션 항목은 **_data/navigation.yml** 파일 설정을 통해 추가할 수 있다.

**url은 /categories/**로 설정하자.

```
파일명 : navigation.yml
위치 : _data/naivgation.yml
```

```
---
# main links
main:
  - title: "Categories"
    url: /categories/
  - title: "Project"
    url: /project/
  - title: "About"
    url: /about/
---
```



##### 2. 게시글에 category 등록하기

```
파일명 : 2020-12-25-category-config.md
위치 : _posts/2020-12-25-category-config.md
```

```
---
title: Github 블로그 커스터마이징 - 카테고리
categories:	
    - Blog_config
tags:
- github.io 
- 깃허브 블로그
last_modified_at:
---
```



- title : #게시글 제목
- categories : # - 카테고리 제목
- tags : # 태그
- last_modified_at: #수정 시간

게시글은 **_posts/2020-12-25-category-config.md**로 저장되어있다. 우선, 위 사진처럼 분류 기준에 따라 YFM 설정을 완료한다.  tags에 여러 개의 태그를 등록할 경우에는 줄 바꿈을 해주어야 한다. 이때, 줄 바꿈 이후 tab 키를 사용할 경우 아래와 같은 오류가 발생한다. 이점에 주의하고, 줄 바꿈 이후 공백없이 **- 태그명** 형식으로 작성하자.



![image](https://user-images.githubusercontent.com/49560745/103145330-e694e800-477b-11eb-9533-55d88546ed98.png)



##### 3. categories 페이지 등록하기

```
파일명 : category_archive.md
위치 : _pages/category_archive.md
```

```
---
title: "Posts by Category"
layout: categories
permalink: /categories/
author_profile: true
---
```

- title : #카테고리 페이지 제목
- layout:  #categoires
- permalink: #/categories/

categories 페이지는 **_pages/category_archive.md**로 저장되어있다. 

반드시 이 형식(파일명 _archive.md)과 일치하도록 파일명을 설정하고, 해당 경로에 집어넣어줘야한다.  

참고로, permalink는 _config.yml 파일의 category_archive설정의 path 경로와 같음을 기억하자!



```
# Archives
#  Type
#  - GitHub Pages compatible archive pages built with Liquid ~> type: liquid (default)
#  - Jekyll Archives plugin archive pages ~> type: jekyll-archives
#  Path (examples)
#  - Archive page should exist at path when using Liquid method or you can
#    expect broken links (especially with breadcrumbs enabled)
#  - <base_path>/tags/my-awesome-tag/index.html ~> path: /tags/
#  - <base_path>/categories/my-awesome-category/index.html ~> path: /categories/
#  - <base_path>/my-awesome-category/index.html ~> path: /
category_archive:
  type: liquid
  path: /categories/

```

##### 4.tags 페이지 등록하기

```
파일명 : tag_archive.md
위치 : _pages/tag_archive.md
```

````
title: "Posts by Tag"
layout: tags
permalink: /tags/
author_profile: true
````

- title : #타이틀 페이지 제목
- layouts: #tags
- permalink: #/tags/

tags페이지 등록도 **categories 페이지 등록 과정**을 그대로 진행하면된다.

##### 5.  결과  

  

 **카테고리 Page**

![image](https://user-images.githubusercontent.com/49560745/103131791-83587680-46e5-11eb-90d4-12145a38303f.png)

**글 하단 카테고리&태그**

![image](https://user-images.githubusercontent.com/49560745/103145413-71c2ad80-477d-11eb-8d1c-b04db0b9dc31.png)

**태그 page**

![image](https://user-images.githubusercontent.com/49560745/103145426-97e84d80-477d-11eb-880b-60543651c382.png)



1~4의 작업을 완료하고 **categories** 네비게이션바를 클릭하면, 카테고리가 설정됐음을 확인할 수 있다.

마찬가지로, 글 하단에 **tags와 tags페이지**에 접근할 수 있음을 알 수 있다.

자세한 디렉토리, 파일 구조는 Github 링크에서 참고하기 바랍니다!

깃허브 URL - https://github.com/gwang920/gwang920.github.io



# Reference

- [minimal-mistakes 공식사이트](https://mmistakes.github.io/minimal-mistakes/docs/layouts/#layout-categories)

- [카테고리 태그목록 - 취미로 코딩하는 개발자](https://devinlife.com/howto%20github%20pages/category-tag/)



