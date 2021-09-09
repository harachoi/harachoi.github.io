---
title: 프로그래머스 프론트엔드 과제관 - ilovecat
toc: true
categories:	
    - programmers
tags:
- 프로그래머스 과제관
- 프론트엔드
last_modified_at: 
---

 이 포스팅은 [woohyeonjo 님 과제관 리뷰](https://github.com/woohyeonjo/ilovecat-javascript)를 토대로 작성되었습니다. **70~80%**는 클론코딩이며, [프로그래머스 프론트엔드 과제관](https://programmers.co.kr/skill_check_assignments/4)의 요구사항에 맞게 커스터마이징했습니다.

# 문제

## 주제(시나리오)

고양이를 좋아하는 당신은 고양이 사진 전용 검색 웹사이트를 운영하고 있었습니다. 지금까지는 혼자 소소하게 운영해왔는데, 생각보다 고양이 사진을 원하는 사람들이 많아지면서 해결해야 할 문제들이 하나씩 드러나기 시작했어요. 몇 개의 문제는 금세 고칠 수 있지만, 기존 코드를 자세히 봐야만 고칠 수 있는 문제들도 있어서 조금 골치아픈 상황! 심지어 최대 4시간 내에 수정한 뒤 배포를 해야만 합니다. 당신이라면 기존 서비스의 여러 버그를 제한시간 내에 고치고, 유저를 위한 추가 기능까지 구현해볼 수 있을까요? 도전해보세요!

## 과제 설명

- thecatapi 에서 크롤링한 데이터를 이용해 이미지를 검색하는 베이스 코드가 주어집니다.
- 베이스 코드는 모두 ES6 클래스 기반으로 작성되어 있으며, 이 코드에는 여러 개의 버그가 존재합니다. 요구사항을 잘 읽고, 버그를 하나씩 해결해주세요.

## 수행 기술

- JavaScript(ES6)
- 설치되어있는 모듈(node_modules) 외에 다른 외부 라이브러리는 사용하지 않도록 합니다. 예를들어 jQuery, Webpack, Lodash, Axios, Angular, React, Vue, Immutable-js, Ramda 등을 사용할 수 없습니다.

## 요구사항

**참고** 요구사항의 순서는 난이도와 상관이 없음

### HTML, CSS 관련

- 현재 HTML 코드가 전체적으로 `<div>` 로만 이루어져 있습니다. 이 마크업을 시맨틱한 방법으로 변경해야 합니다.
- 유저가 사용하는 디바이스의 가로 길이에 따라 검색결과의 row 당 column 갯수를 적절히 변경해주어야 합니다.
  - 992px 이하: 3개
  - 768px 이하: 2개
  - 576px 이하: 1개
- 다크 모드(Dark mode)를 지원하도록 CSS를 수정해야 합니다.
  - CSS 파일 내의 다크 모드 관련 주석을 제거한 뒤 구현합니다.
  - 모든 글자 색상은 `#FFFFFF` , 배경 색상은 `#000000` 로 한정합니다.
  - 기본적으로는 OS의 다크모드의 활성화 여부를 기반으로 동작하게 하되, 유저가 테마를 토글링 할 수 있도록 좌측 상단에 해당 기능을 토글하는 체크박스를 만듭니다.

### 이미지 상세 보기 모달 관련

- 디바이스 가로 길이가 768px 이하인 경우, 모달의 가로 길이를 디바이스 가로 길이만큼 늘려야 합니다.
- **`필수`** 이미지를 검색한 후 결과로 주어진 이미지를 클릭하면 모달이 뜨는데, 모달 영역 밖을 누르거나 / 키보드의 ESC 키를 누르거나 / 모달 우측의 닫기(x) 버튼을 누르면 닫히도록 수정해야 합니다.
- 모달에서 고양이의 성격, 태생 정보를 렌더링합니다. 해당 정보는 `/cats/:id` 를 통해 불러와야 합니다.
- `추가` 모달 열고 닫기에 fade in/out을 적용해 주세요.

### 검색 페이지 관련

- 페이지 진입 시 포커스가 `input` 에 가도록 처리하고, 키워드를 입력한 상태에서 `input` 을 클릭할 시에는 기존에 입력되어 있던 키워드가 삭제되도록 만들어야 합니다.
- **`필수`** 데이터를 불러오는 중일 때, 현재 데이터를 불러오는 중임을 유저에게 알리는 UI를 추가해야 합니다.
- **`필수`** 검색 결과가 없는 경우, 유저가 불편함을 느끼지 않도록 UI적인 적절한 처리가 필요합니다.
- 최근 검색한 키워드를 `SearchInput` 아래에 표시되도록 만들고, 해당 영역에 표시된 특정 키워드를 누르면 그 키워드로 검색이 일어나도록 만듭니다. 단, 가장 최근에 검색한 5개의 키워드만 노출되도록 합니다.
- 페이지를 새로고침해도 마지막 검색 결과 화면이 유지되도록 처리합니다.
- **`필수`** SearchInput 옆에 버튼을 하나 배치하고, 이 버튼을 클릭할 시 `/api/cats/random50` 을 호출하여 화면에 뿌리는 기능을 추가합니다. 버튼의 이름은 마음대로 정합니다.
- lazy load 개념을 이용하여, 이미지가 화면에 보여야 할 시점에 load 되도록 처리해야 합니다.
- `추가` 검색 결과 각 아이템에 마우스 오버시 고양이 이름을 노출합니다.

### 스크롤 페이징 구현

- 검색 결과 화면에서 유저가 브라우저 스크롤 바를 끝까지 이동시켰을 경우, 그 다음 페이지를 로딩하도록 만들어야 합니다.

### 랜덤 고양이 배너 섹션 추가

- 현재 검색 결과 목록 위에 배너 형태의 랜덤 고양이 섹션을 추가합니다.
- 앱이 구동될 때 `/api/cats/random50` api를 요청하여 받는 결과를 별도의 섹션에 노출합니다.
- 검색 결과가 많더라도 화면에 5개만 노출하며 각 이미지는 좌, 우 슬라이드 이동 버튼을 갖습니다.
- 좌, 우 버튼을 클릭하면, 현재 노출된 이미지는 사라지고 이전 또는 다음 이미지를 보여줍니다.(트렌지션은 선택)

### 코드 구조 관련

- ES6 module 형태로 코드를 변경합니다.
  - `webpack` , `parcel` 과 같은 번들러를 사용하지 말아주세요.
  - 해당 코드 실행을 위해서는 `http-server` 모듈을(로컬 서버를 띄우는 다른 모듈도 사용 가능) 통해 `index.html` 을 띄워야 합니다.
- API fetch 코드를 `async` , `await` 문을 이용하여 수정해주세요. 해당 코드들은 에러가 났을 경우를 대비해서 적절히 처리가 되어있어야 합니다.
- **`필수`** API 의 status code 에 따라 에러 메시지를 분리하여 작성해야 합니다. 아래는 예시입니다.

```
  const request = async (url: string) => {     try {       const result = await fetch(url);       return result.json();     } catch (e) {       console.warn(e);     }   }    const api = {     fetchGif: keyword => {       return request(`${API_ENDPOINT}/api/gif/search?q=${keyword}`);     },     fetchGifAll: () => {       return request(`${API_ENDPOINT}/api/gif/all`);     }   };
```

- SearchResult 에 각 아이템을 클릭하는 이벤트를 Event Delegation 기법을 이용해 수정해주세요.
- 컴포넌트 내부의 함수들이나 Util 함수들을 작게 잘 나누어주세요.

## API

### 1. GET /cats/random50

#### Request parameter

None

#### Query paramter

None

#### Response

Success 200

| Field name | Type  | Description                           |
| ---------- | ----- | ------------------------------------- |
| data       | Array | 랜덤한 50개의 고양이 사진 목록입니다. |

```
HTTP/1.1 200 OK
{
  "data": [{
    id: string
    url: string
    name: string
  }]
}
```

### 2. GET /cats/search

#### Request parameter

None

#### Query paramter

| Field name | Type   | Description              |
| ---------- | ------ | ------------------------ |
| q          | string | 고양이의 품종(영어/한글) |

#### Response

Success 200

| Field name | Type  | Description                              |
| ---------- | ----- | ---------------------------------------- |
| data       | Array | Keyword로 검색된 고양이 사진 목록입니다. |

```
HTTP/1.1 200 OK
{
  "data": [{
    id: string
    url: string
    name: string
  }]
}
```

### 3. GET /cats/:id

#### Request parameter

| Field name | Type   | Description                |
| ---------- | ------ | -------------------------- |
| id         | string | 고양이 사진의 id값 입니다. |

#### Query paramter

None

#### Response

Success 200

| Field name | Type   | Description                     |
| ---------- | ------ | ------------------------------- |
| data       | Object | Id로 검색된 고양이 사진 입니다. |

```
HTTP/1.1 200 OK
{
  "data": {
    name: string
    id: string
    url: string
    width: number
    height: number
    temperament: string
    origin: string
  }
}
```

## 풀이

### 반응형 웹

유저가 사용하는 디바이스의 가로 길이에 따라 검색결과의 row 당 column 갯수를 적절히 변경해주어야 합니다.

```css
.SearchResult {
  margin-top: 10px;
  display: grid;
  grid-template-columns: repeat(4, minmax(250px, 1fr));
  grid-gap: 10px;
}

/* 992px 이하 3개*/
@media (max-width : 992px){
    .SearchResult{
        grid-template-columns: repeat(3, minmax(250px, 1fr));
    }
}
/* 768px 이하 적용 2개 */
@media (max-width : 768px){
    .SearchResult{
        grid-template-columns: repeat(2, minmax(250px, 1fr));
    }
}
/* 576px 이하 적용 1개 */
@media (max-width : 576px){
    .SearchResult{
        grid-template-columns: repeat(1, minmax(250px, 1fr));
    }
}
```

- 미디어쿼리를 적용한다. 
- `minmax(최대값, 최소값)`, `minmax()` 함수를 사용하면 최소값, 최대값 범위 내에서 값을 유연하게 처리한다. 

- 캐스케이딩 등 다른 조건이 같으면 `css`는 나중에 오는 쿼리가 우선순위를 갖는다.
- `.SearchResult`를 가장 아래에 선언하면 `@media` 쿼리가 적용이 안된다.

### 다크모드

다크 모드(Dark mode)를 지원하도록 CSS를 수정해야 합니다.

- 모든 글자 색상은 `#FFFFFF` , 배경 색상은 `#000000` 로 한정합니다. prefers-color-scheme 값은 dark, light 두 가지가 있으며, 브라우저의 모드에 따라서 미디어퀴리가 적용됨.

```
파일명 : darkmode.js
```

```javascript
const STORAGE_KEY = 'user-color-scheme';
const COLOR_MODE_KEY = '--color-mode';

const darkmodeBtn = document.querySelector('.darkmode-btn');

const getCSSCustomProp = (propKey) => {

    /* 
       getComputedStyle 인자로 전달받은 요소의 모든 css 속성값을 담은 객체를 회신
       ref : https://developer.mozilla.org/ko/docs/Web/API/Window/getComputedStyle
    */
    let response = getComputedStyle(document.documentElement).getPropertyValue(propKey);

    // Tidy up the string if there’s something to work with
    if (response.length) {
        response = response.replace(/\'|"/g, '').trim();
    }

    // Return the string response by default
    return response;
};

const applySetting = passedSetting => {
    let currentSetting = passedSetting || localStorage.getItem(STORAGE_KEY);

    if (currentSetting) {
        document.documentElement.setAttribute('data-user-color-scheme', currentSetting);
        setButtonLabel(currentSetting);
    } else {
        setButtonLabel(getCSSCustomProp(COLOR_MODE_KEY));
    }
};

const toggleSetting = () => {
    let currentSetting = localStorage.getItem(STORAGE_KEY);

    switch (currentSetting) {
        case null:
            currentSetting = getCSSCustomProp(COLOR_MODE_KEY) === 'dark' ? 'light' : 'dark';
            break;
        case 'light':
            currentSetting = 'dark';
            break;
        case 'dark':
            currentSetting = 'light';
            break;
    }

    localStorage.setItem(STORAGE_KEY, currentSetting);

    return currentSetting;
};

// 버튼 생성
const setButtonLabel = currentSetting => {
    darkmodeBtn.innerText = currentSetting === 'dark' ? '🌕' : '🌑';
};

darkmodeBtn.addEventListener('click', evt => {
    evt.preventDefault();

    applySetting(toggleSetting());
});

applySetting();
```



**추가**

1. `App.js`에 버튼 생성

```
파일명 : App.js
```

```javascript
const darkmodeBtn = document.createElement('span');
    darkmodeBtn.className = 'darkmode-btn';
    darkmodeBtn.innerText = '🌕';

    $target.appendChild(darkmodeBtn);
```

2. `index.html` 에 `type="module"` 적용

```
파일명 : index.html
```

```html
<script type="module" src="src/utils/darkmode.js"></script>
```

3. `style.css`에 미디어쿼리 `css` 추가

````
파일명 : style.css
````

```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-mode: 'dark';
  }
  
  :root:not([data-user-color-scheme]) {
    --background: var(--color-dark);
    --text-color: var(--color-light);
  }
}

[data-user-color-scheme="dark"] {
  --background: var(--color-dark);
  --text-color: var(--color-light);
}

```

4. `style.css` 전역변수 설정

```css
:root {
  --color-mode: 'light';
  --color-dark: black;
  --color-light: white;
  --background: white;
  --text-color: black;
}
/*
ease-in-out
정해진 시간 동안 요소의 속성값을 부드럽게 변화
*/
body {
  background: var(--background);
  color: var(--text-color);
  transition: background 500ms ease-in-out, color 200ms ease;
}
```



```javascript
import ImageInfo from "./components/ImageInfo.js";
import SearchInput from "./components/SearchInput.js";
import SearchResult from "./components/SearchResult.js";
import Loading from "./components/Loading.js";

import { getItem, setItem } from './utils/sessionStorage.js';
import { api } from "./api/api.js";

export default class App {
  $target = null;
  data = [];

  constructor($target) {
    this.$target = $target;

    this.searchInput = new SearchInput({
      $target,
      onSearch: async keyword => {
        loading.toggleSpinner();
        const response = await api.fetchCats(keyword);
        if(!response.isError){
          setItem('data', response.data);
          console.log(response.data);
          SearchResult.setState(response.data);
          loading.toggleSpinner();
        } else{
          error.setState(response.data);
        }
      }
    });

    this.searchResult = new SearchResult({
      $target,
      initialData: this.data,
      onClick: image => {
        this.imageInfo.setState({
          visible: true,
          image
        });
      }
    });

    this.imageInfo = new ImageInfo({
      $target,
      data: {
        visible: false,
        image: null
      }
    });

    const loading = new Loading({
        $target
    });

    const darkmodeBtn = document.createElement('span');
    darkmodeBtn.className = 'darkmode-btn';
    darkmodeBtn.innerText = '🌕';

    $target.appendChild(darkmodeBtn);
  }

  setState(nextData) {
    console.log(this);
    this.data = nextData;
    this.searchResult.setState(nextData);
  }
}

```



```javascript
const API_ENDPOINT =
  "https://oivhcpn8r9.execute-api.ap-northeast-2.amazonaws.com/dev";


const request = async url =>{
  try {
    const respones = await fetch(url);
    if (reponse.ok) {
        const data= await reponse.json();
        return data;      
    }else{
        const errorData = await respones.json();
        throw errorData;
    }
  } catch (e) {
      throw{
          message: e.message,
          status: e.status
      }
  }
}

const api = {
  fetchCats: async keyword => {
    try{
      const data = await fetch(`${API_ENDPOINT}/api/cats/search?q=${keyword}`);
      const result2=[];
      const result=data.json();
      
      return{
        isError: false,
        data: result2
      }; 
  }catch(err){
      return {
          isError: true,
          data: err
      };
  }
},
  fetchRandomCats: async keyword =>{

  }
};


export { api };
```

### css grid

https://heropy.blog/2019/08/17/css-grid/

### Loading

```v
파일명 : Loading.js
```

```javascript
export default class Loading {
    constructor({ $target }) {
        this.spinnerWrapper = document.createElement('div');
        this.spinnerWrapper.className = 'spinner-wrapper';
        this.spinnerWrapper.classList.add('hidden');

        $target.appendChild(this.spinnerWrapper);

        this.render();
    }

    /*
        toggle 현 상태 = > 반대 상태
    */
    toggleSpinner() {
        const spinner = document.querySelector('.spinner-wrapper');
        spinner.classList.toggle('hidden');
    }

    render() {
        const spinnerImage = document.createElement('img');
        spinnerImage.className = 'spinner-image';
        spinnerImage.src = 'src/img/loading.gif';

        this.spinnerWrapper.appendChild(spinnerImage);
    }
}
```

```
파일명 : App.js
```

```javascript
this.searchInput = new SearchInput({
      $target,
      keywords,
      onSearch: async keyword => {
        loading.toggleSpinner();
        // const response =  await api.fetchCats(keyword);
        // if(!response.isError){
        //   console.log(response.data);
        //   SearchResult.setState(response.data);
        //   loading.toggleSpinner();
        // }else{
        //     console.log(response.data);
        // }


        await api.fetchCats(keyword).then(({ data }) =>{
          setItem('data',data.data);
          this.setState(data.data)
        });
        loading.toggleSpinner();
      }
    });
```

### Api

```javascript
const API_ENDPOINT =
  "https://oivhcpn8r9.execute-api.ap-northeast-2.amazonaws.com/dev";


const request = async url =>{
  try {
    const reponse = await fetch(url);
    if (reponse.ok) {
        const data= await reponse.json();
        return data;      
    }else{
        const errorData = await respones.json();
        throw errorData;
    }
  } catch (e) {
      throw{
          message: e.message,
          status: e.status
      }
  }
}

const api = {
  fetchCats: async keyword => {
    try{
       const {data} = await request(`${API_ENDPOINT}/api/cats/search?q=${keyword}`);
      return{
        isError: false,
        data: data
      }; 
  }catch(e){
      return {
          isError: true,
          data: e
      };
  }
},
  fetchRandomCats: async ()=>{
    try {
        const {data} = await request(`${API_ENDPOINT}/api/cats/ramdom50`);
        return {
          isError: false,
          data: data
        }
    } catch (e) {
      return{
        isError: true,
        data: e
      }
    }
  },
  fetchDetailcat: async id=> {
    try {
      const {data} = await request(`${API_ENDPOINT}/api/cats/${id}`);
      return {
        isError: false,
        data: data
      }
  } catch (e) {
    return{
      isError: true,
      data: e
    }
  }
  }
};


export { api };
```

### keyword 세션 스토리지

```
파일명 : App.js
```

```javascript
import ImageInfo from "./components/ImageInfo.js";
import SearchInput from "./components/SearchInput.js";
import SearchResult from "./components/SearchResult.js";
import Loading from "./components/Loading.js";
import Error from "./components/Error.js";
import DetailModal from './components/DetailModal.js';

import { api } from "./api/api.js";
import { getItem, setItem } from './utils/sessionStorage.js';

export default class App {
  $target = null;
  data = [];

  constructor($target) {
    const keywords = getItem('keywords');
    const data = getItem('data');
    this.$target = $target;

    const searchInput = new SearchInput({
      $target,
      keywords,
      onSearch: async keyword => {
        loading.toggleSpinner();
        const response =  await api.fetchCats(keyword);
        if(response.data.length===0){
          alert("검색결과가 없습니다.");
          searchInput.deleteKeyword();
          loading.toggleSpinner();
        }
        else if(!response.isError){
          setItem('data', response.data);
          searchResult.setState(response.data);
          loading.toggleSpinner();
        }else{
          error.setState(response.data);
        }
      }
    });

    const searchResult = new SearchResult({
      $target,
      initialData: this.data,
      onClick: async id => {
        loading.toggleSpinner();
        const response= await api.fetchDetailCat(id);
        if(!response.isError){
          detailModal.setState(response);
          loading.toggleSpinner();
        }else{
          error.setState(response.data);
        }
      }
    });

    const imageInfo = new ImageInfo({
      $target,
      data: {
        visible: false,
        image: null
      }
    });

    const loading =new Loading({
      $target
    });

    const error = new Error({
      $target
    });

    const detailModal = new DetailModal({
      $target
    });

    const darkmodeBtn = document.createElement('span');
    darkmodeBtn.className = 'darkmode-btn';
    darkmodeBtn.innerText = '🌕';

    $target.appendChild(darkmodeBtn);
  }
}

```



```
파일명 : SearchInput.js
```

```javascript
import { getItem, setItem } from "../utils/sessionStorage.js";
const TEMPLATE = '<input type="text">';

export default class SearchInput {
  constructor({ $target,keywords, onSearch }) {

    this.recent=keywords;
    this.onSearch=onSearch;
    this.section = document.createElement('section'); 
    this.section.className = 'searching-section';

    $target.appendChild(this.section);

    console.log("SearchInput created.", this);

    this.render();
    this.focusOnSearchBox();
  }

  searchByKeyword(keyword) {
    if (keyword.length == 0) return;

    this.addRecentKeyword(keyword);
    this.onSearch(keyword);
}

  focusOnSearchBox() {
    const searchBox = document.querySelector('.search-box');
    searchBox.focus();
  }

  addRecentKeyword(keyword) {
    if (this.recent.includes(keyword)) return;
    if (this.recent.length == 5) this.recent.shift();

    this.recent.push(keyword);
    setItem('keywords', this.recent);

    this.render();
}

  searchByKeyword(keyword) {
    if (keyword.length == 0) return;

    this.addRecentKeyword(keyword);
    this.onSearch(keyword);
  }

  deleteKeyword() {
      const searchBox = document.querySelector('.search-box');
      searchBox.value = '';
  }

  render() {
    this.section.innerHTML = '';

    const randomBtn = document.createElement('span');
    randomBtn.className = 'random-btn';
    randomBtn.innerText = '🐱';

    const wrapper = document.createElement('div');
    wrapper.className = 'search-box-wrapper';

    const searchBox = document.createElement('input');
    searchBox.className = 'search-box';
    searchBox.placeholder = '고양이를 검색하세요.';
    /*
        div blokc 속성, 블록 처럼 쌓인다.
        span inline 속성, 횡렬로 다닥다닥 붙는다.
    */
    const recentKeywords = document.createElement('div');
    recentKeywords.className = 'recent-keywords';

    this.recent.map(keyword => {
        const link = document.createElement('span');
        link.className = 'keyword';
        link.innerText = keyword;

        link.addEventListener('click', () => { this.searchByKeyword(keyword); });

        recentKeywords.appendChild(link);
    });

    randomBtn.addEventListener('click', this.onRandom);
    searchBox.addEventListener('focus', this.deleteKeyword);
    searchBox.addEventListener('keyup', event => {
        if (event.keyCode == 13) {
            this.searchByKeyword(searchBox.value);
        }
    });


    wrapper.appendChild(searchBox);
    wrapper.appendChild(recentKeywords);
    this.section.appendChild(randomBtn);
    this.section.appendChild(wrapper);
  }
}

```



```css
파일명 : style.css
```

```css
.search-box-wrapper {
  display: flex;
  flex-direction: column;
  width: 50%;
}

.search-box {
  font-size: 20px;
}

.recent-keywords {
  margin-top: 10px;
}
```



```
파일명 : SearchResult.js
```

```javascript
export default class SearchResult {
  $searchResult = null;
  data = null;
  onClick = null;

  constructor({ $target, initialData, onClick }) {
    this.$searchResult = document.createElement("div");
    this.$searchResult.className = "SearchResult";
    $target.appendChild(this.$searchResult);

    this.data = initialData;
    this.onClick = onClick;

    this.render();
  }

  setState( data ) {
    this.data = data;
    this.render();
  }


  render() {
    this.$searchResult.innerHTML = this.data
      .map(
        ({url,name})=> `
          <div class="item">
            <img src=${url} alt=${name} />
          </div>
        `
      )
      .join("");

    this.$searchResult.querySelectorAll(".item").forEach(($item, index) => {
      $item.addEventListener("click", () => {
        this.onClick(this.data[index].id);
      });
    });
  }
}

```

- `App.js` 에서 `response.data` 는 `{[]}` 형식이다.
- 그렇기 때문에 `setState({ data })` 객체로 데이터를 전달 받고, `this.data = data;` 알맹이만 뽑아내면 `map(iterator)` 사용이 가능하다.



```
파일명 : DetailModal.js
```

```javascript
export default class DetailModal {
    constructor({$target}) {
        this.isVisible = false;
        this.data = null;
        this.modalWrapper = document.createElement('div');
        this.modalWrapper.className = 'modal-wrapper';
        this.modalWrapper.classList.add('hidden');
        
        $target.appendChild(this.modalWrapper);

        this.render();
    }

    toggleModal(){
        this.isVisible = !this.isVisible;
        
        const modal = document.querySelector('.modal-wrapper');
        modal.classList.toggle('hidden');
    }

    setState(data) {
        this.toggleModal();
        this.data = data;
        this.render();
    }
    
    onClose() {
        this.toggleModal();
        this.data = null;
        this.modalWrapper.innerHTML = '';
    }

    render() {
        if(!this.isVisible) return;
        console.log(this.data);
        const  { url }  = this.data;
        const { name, origin, temperament } = this.data ?
            this.data : {name: '정보없음', origin: '정보없음', temperament: '정보없음'};


        console.log("name = "+name + " origin = " + origin + " temperament= " + temperament);
        
        const overlay = document.createElement('div');
        overlay.className = 'overlay';

        const modalContents = document.createElement('section');
        modalContents.className = 'modal-contents';

        const modalHeader = document.createElement('header');
        modalHeader.className = 'modal-header';

        const modalTitle = document.createElement('p');
        modalTitle.className = 'modal-title';
        modalTitle.innerText = name;

        const closeBtn = document.createElement('span');
        closeBtn.className = 'close-btn';
        closeBtn.innerText = 'X';

        const modalImage = document.createElement('img');
        modalImage.className = 'modal-image';
        modalImage.src = url;

        const modalInfo = document.createElement('article');
        modalInfo.className = 'modal-info';

        const catOrigin = document.createElement('p');
        catOrigin.className = 'cat-origin';
        catOrigin.innerText = origin;

        const catTemperament = document.createElement('p');
        catTemperament.className= 'cat-temperament';
        catTemperament.innerText = temperament;

        // const catWeight = document.createElement('p');
        // catWeight.className = 'cat-width';
        // catWeight.innerText = `${imperial} (imperial) / ${metric} (metric)`;

        closeBtn.addEventListener('click', () => { this.onClose(); });
        overlay.addEventListener('click', () => { this.onClose(); });
        
        modalHeader.appendChild(modalTitle);
        modalHeader.appendChild(closeBtn); 

        modalInfo.appendChild(catOrigin);
        modalInfo.appendChild(catTemperament);
        //modalInfo.appendChild(catWeight);

        modalContents.appendChild(modalHeader);
        modalContents.appendChild(modalImage);
        modalContents.appendChild(modalInfo);

        this.modalWrapper.appendChild(overlay);
        this.modalWrapper.appendChild(modalContents);

    }
}

```

```
파일명 : style.css
```

```css
@font-face {
  font-family: "Goyang";
  src: url("../fonts/Goyang.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}

html {
  box-sizing: border-box;
}

body * {
  font-family: Goyang;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

.App {
  margin: 1.5em auto;
  max-width: 1200px;
  column-gap: 1.5em;
}

.SearchResult {
  margin-top: 10px;
  display: grid;
  grid-template-columns: repeat(4, minmax(250px, 1fr));
  grid-gap: 10px;
}

/* 992px 이하 3개*/
@media (max-width : 992px){
    .SearchResult{
        grid-template-columns: repeat(3, minmax(250px, 1fr));
    }
}
/* 768px 이하 적용 2개 */
@media (max-width : 768px){
    .SearchResult{
        grid-template-columns: repeat(2, minmax(250px, 1fr));
    }
}
/* 576px 이하 적용 1개 */
@media (max-width : 576px){
    .SearchResult{
        grid-template-columns: repeat(1, minmax(250px, 1fr));
    }
}

.SearchResult img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.SearchResult .item {
  background-color: #eee;
  display: inline-block;
  margin: 0 0 1em;
  width: 100%;
}

.SearchInput {
  width: 100%;
  font-size: 40px;
  padding: 10px 15px;
}

.ImageInfo {
  position: fixed;
  left: 0;
  top: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
}

.ImageInfo .title {
  display: flex;
  justify-content: space-between;
}

.ImageInfo .title,
.ImageInfo .description {
  padding: 5px;
}

.ImageInfo .content-wrapper {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 5px;
}

.ImageInfo .content-wrapper img {
  width: 100%;
}

/* 
    css 변수 선언, 하이픈(-)을 두개 쓰고 변수명을 쓴 다음 콜론(:)을 붙이고 값을 입력 
*/
:root {
  --color-mode: 'light';
  --color-dark: black;
  --color-light: white;
  --background: white;
  --text-color: black;
}
/*
ease-in-out
정해진 시간 동안 요소의 속성값을 부드럽게 변화
*/
body {
  background: var(--background);
  color: var(--text-color);
  transition: background 500ms ease-in-out, color 200ms ease;
}
/* 
  미디어쿼리 
*/
@media (prefers-color-scheme: dark) {
  :root {
    --color-mode: 'dark';
  }
  
  :root:not([data-user-color-scheme]) {
    --background: var(--color-dark);
    --text-color: var(--color-light);
  }
}

[data-user-color-scheme="dark"] {
  --background: var(--color-dark);
  --text-color: var(--color-light);
}

.searching-section {
  display: flex;
  justify-content: center;
  align-items: center;
}

.search-box-wrapper {
  display: flex;
  flex-direction: column;
  width: 50%;
}

.search-box {
  font-size: 20px;
}

.recent-keywords {
  margin-top: 10px;
}

.keyword {
  background-color: rgb(255, 127, 0);
  color: white;
  border-radius: 11px;
  
  padding: 5px;
  margin-right: 8px;
}

.random-btn {
  font-size: 50px;
  margin-right: 10px;
}

.results-section {
  margin-top: 3%;
}

.notice-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.notice {
  margin-top: 8%;
  text-align: center;
}

.notice-image {
  height: 300px;
  width: 350px;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

.cat-card {
  display: flex;
  flex-direction: column;

  /* TODO: 카드 정렬하기 */
  margin-left: calc( (100% - (20% * 4)) / 4);
  
  width: 250px;
  height: 350px;
}

.card-image {
  height: 70%;
}

.hidden {
  visibility: hidden;
}

.modal-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  z-index: 1;
  
  display: flex;
  justify-content: center;
  align-items: center;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.5);
}

.modal-contents {
  position: relative;
  display: flex;
  flex-direction: column;
  
  height: 70%;
  width: 30%;
  padding: 10px;
  background-color: var(--background);
  color: var(--text-color);

  /* TODO: box-shadow */
}

.modal-image {
  height: 60%;
}

.modal-header {
  display: flex;
  justify-content: space-between;

  font-size: 30px;
}

.modal-title {
  margin: 0;
}

.spinner-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  z-index: 1;
  
  display: flex;
  justify-content: center;
  align-items: center;

  background-color: rgb(255, 255, 255, 0.7);
}

.spinner-image {
  width: 300px;
  height: 300px;
  border-radius: 49%;
}

.error-section {
  display: flex;
  flex-direction: column;

  justify-content: center;
  align-items: center;
}

.error-image {
  margin-top: 8%;
  border-radius: 10%;
}

.status-code {
  margin: 0;
  font-size: 5rem;
  font-weight: bold;
}

.error-message {
  margin-top: -15px;
  font-size: 20px;
}

.return-btn {
  margin-top: 15px;
}

.darkmode-btn {
  font-size: 3rem;
  position: fixed;
  top: 1rem;
  right: 5rem;
  z-index: 3;
}

```



```
파일명 : index.html
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="src/css/style.css" />
    <title>Cat Search</title>
  </head>
  <body>
    <div class="App"></div>
    <script src="src/utils/validator.js"></script>
    <script type="module" src="src/main.js"></script>
    <script type="module" src="src/utils/darkmode.js"></script>
  </body>
</html>

```

## 소스코드

[github/gwang920](https://github.com/gwang920/ilovecat-javascript)



# Reference

- [woohyeonjo 님 프로그래머스 과제관 리뷰](https://github.com/woohyeonjo/ilovecat-javascript)
