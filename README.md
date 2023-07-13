# Momento Web SDK 
## 기본 설정 가이드

### 1. 사전 안내

- script 태그를 이용해 라이브러리를 삽입합니다.
  - momentoAdSdk 객체를 사용할 수 있게 합니다.
- script 태그에서 momentoAdSdk.showAd 함수를 호출합니다.
- 광고가 노출될 태그(slot)의 id를 지정 해야합니다.
- 광고가 노출될 태그의 id와 showAd에 전달하는 파라미터 settings.slot의 값은 동일해야 합니다.
- 한 페이지에 Momento 광고를 여러개 삽입할 경우 광고가 노출될 태그의 id를 다르게 설정해야 합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
    <script>
      window.momentoAdSdk.showAd({
        format: 'banner',
        appId: 'app-id', //매체를 등록할 때 받은 appId
        unitId: 'unit-id', //매체를 등록할 때 받은 unitId
        settings: {
          slot: 'momento-ad-slot', //광고를 보여줄 태그의 id값
        },
      });
    </script>
  </head>
</html>
```

### 2. 기본 파라미터 정의

|               | **required** | **type** | **description**                | **example**                      |
| ------------- | :----------: | :------: | ------------------------------ | -------------------------------- |
| format        |     true     |  string  | 광고의 기본 포맷               | banner/native/video              |
| appId         |     true     |  string  | 매체를 등록할 때 받은 appId    |                                  |
| unitId        |     true     |  string  | 매체를 등록할 때 받은 unitId   |                                  |
| settings      |     true     |  object  | 포맷에 따라 설정이 달라집니다. | { slot: 'momento-ad-slot', ... } |
| settings.slot |     true     |  string  | 광고를 보여줄 태그의 id 값     | momento-ad-slot                  |

### 3. 스크립트 설정

- 모멘토SDK는 두가지 script 설정 방식을 제공합니다.
- 기본 동작은 동일하지만, SDK에서 제공하는 콜백함수 사용 여부에 따라 선택할 수 있습니다.

1. (기본) momentoAdSdk 객체 스크립트
   - 콜백함수 사용 가능합니다.
     - `onLoaded`, `onFailed`, `nativeCallback` 등 (하단 포맷별 상세 설정 가이드 참고)

```html
<iframe id="momento-ad-slot"></iframe>
<script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
<script>
  window.momentoAdSdk.showAd({
    format: 'banner',
    appId: '12345',
    unitId: '678910',
    settings: {
      slot: 'momento-ad-slot',
      onLoaded: function(unitId){console.log('onLoad', unitId},
      onFailed: function(error){console.log('onFailed', error}
    }
  });
</script>
```

2. 쿼리스트링 스크립트

- 기본 파라미터를 쿼리스트링에 작성하여 사용 가능합니다.
- 콜백함수 사용 불가합니다.

```html
<iframe id="momento-ad-slot"></iframe>
<script src="https://web-sdk.momento.dev/prod/momento-latest.min.js?format=banner&appId=12345&unitId=678910&slotId=momento-ad-slot"></script>
```

# Banner/Native/Video 별 상세 설정 가이드

## 배너 포맷 가이드

1. Momento SDK 초기화

- 서비스 페이지에 Momento SDK를 로드합니다.
- SDK는 페이지에 최초 1회만 로드되어야 합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
  </head>
</html>
```

2. slot 정의하기

- `appId`, `unitId` : 매체를 등록할 때 받은 값을 입력합니다.
- `settings.slot` : 광고를 보여줄 태그의 id 값을 지정할 수 있습니다.
- `settings.backfills` : 광고 호출 에러 시 비어있는 광고 slot에 보여질 html 마크업 요소를 추가할 수 있습니다. 배열에 담긴 요소는 랜덤으로 노출됩니다.
- `settings.onLoaded` : 배너 load 성공 시 실행되는 콜백 함수를 추가할 수 있습니다.
- `settings.onFailed` : 배너 load 실패 시 실행되는 콜백 함수를 추가할 수 있습니다.

### Banner Settings Option 설정

|           | **required** |     **type**     | **description**                                     | **example**                           |
| --------- | :----------: | :--------------: | --------------------------------------------------- | ------------------------------------- |
| slot      |     true     |      string      | 광고를 보여줄 노드의 id 값                          | momento-ad-banner                     |
| backfills |     true     |     string[]     | 광고 에러 시 빈 광고를 채울 HTML 마크업 요소의 배열 | [element, element, …]                 |
| onLoaded  |    false     | function(unitId) | 배너 load 성공 시 실행되는 콜백 함수                | function(unitId){console.log(unitId)} |
| onFailed  |    false     | function(error)  | 배너 load 실패 시 실행되는 콜백 함수                | function(error){console.log(error)}   |

```html
<script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
<script>
  window.momentoAdSdk.showAd({
    format: 'banner',
    appId: '0123456789',
    unitId: '9876543210',
    settings: {
      slot: 'momento-ad-banner',
  	backfills: ['<div>광고 준비중</div>', '<img src="backfill.jpeg" />']
      onLoaded: (unitId) => console.log(unitId),
      onFailed: (error) => console.log(error),
    },
  });
</script>
```

3. 배너 광고 보여주기

```html
<body>
  <div id="momento-ad-banner">여기에 광고가 노출됩니다.</div>
</body>
```

## 네이티브 포맷 가이드

1. Momento SDK 초기화

- 서비스 페이지에 Momento SDK를 로드합니다.
- SDK는 페이지에 최초 1회만 로드되어야 합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
  </head>
</html>
```

2. slot 정의하기

- `appId`, `unitId` : 매체를 등록할 때 받은 값을 입력합니다.
- `settings.slot` : 광고를 보여줄 노드의 id 값을 지정할 수 있습니다.
- `settings.nativeCallback` : 콜백 함수 인자로 네이티브 광고 레이아웃을 구성할 수 있습니다.
- `settings.onLoaded`: 네이티브 load 성공 시 실행되는 콜백 함수를 추가할 수 있습니다.
- `settings.onFailed`: 네이티브 load 실패 시 실행되는 콜백 함수를 추가할 수 있습니다.

### Native Settings Option 설정

|                | **required** |       **type**        | **description**                                                     | **example**                                |
| -------------- | :----------: | :-------------------: | ------------------------------------------------------------------- | ------------------------------------------ |
| slot           |     true     |        string         | 광고를 보여줄 노드의 id 값                                          | momento-ad-native                          |
| nativeCallback |     true     | function({title,...}) | 네이티브 광고를 구성하는 컨텐츠 요소(asset)를 인자로 받는 콜백 함수 | 하단 Native 광고 레이아웃 구성 가이드 참고 |
| onLoaded       |    false     |   function(unitId)    | 네이티브 load 성공 시 실행되는 콜백 함수                            | function(unitId){console.log(unitId)}      |
| onFailed       |    false     |    function(error)    | 네이티브 load 실패 시 실행되는 콜백 함수                            | function(error){console.log(error)}        |

### 네이티브 광고 레이아웃 구성

- `nativeCallback` 콜백 함수 인자로 직접 네이티브 광고 레이아웃을 구성할 수 있습니다.
- (필수) [AdChoices](https://en.wikipedia.org/wiki/AdChoices) 아이콘이 노출되도록 구성 해야합니다. 다른요소에 가려지지 않도록 광고 우측 상단 배치를 권장합니다.

| **asset**     | **required** | **type**                       | **example**                |
| ------------- | :----------: | ------------------------------ | -------------------------- |
| imageUrl      |    string    | 광고 메인이미지 url            | `https://imageUrl...       |
| title         |    string    | 광고 제목                      | ‘광고 제목입니다.’         |
| clickUrl      |    string    | 광고 click시 이동 url          | `https://clickUrl...       |
| description   |    string    | 광고 상세 설명                 | ‘광고 설명입니다.’         |
| cta           |    string    | 광고 액션 유도 문구            | ‘바로가기’                 |
| logoUrl       |    string    | 광고주 로고                    | `https://logoUrl…          |
| adchoiceImage |    string    | 광고 Privacy policy 아이콘 url | `https://adchoiceImageUrl… |
| adchoiceClick |    string    | 광고 Privacy policy 클릭 url   | `https://adchoiceClickUrl… |

```html
<script src="https://web-sdk.momento.dev/prod/momento-latest.min.js"></script>
<script>
  window.momentoAdSdk.showAd({
     format: 'native',
     appId: '0123456789',
     unitId: '9876543210',
     settings: {
       slot: 'momento-ad-native',
       nativeCallback: function({imageUrl, title, description, cta, logoUrl, clickUrl, adChoiceImageUrl, adChoiceClickUrl}){
  		let nativeSlot = document.getElementById('momento-native');

           let anchorElement = document.createElement('a');
           anchorElement.href = clickUrl;

           let imgElement = document.createElement('img');
           imgElement.src = imageUrl;

           let titleElement = document.createElement('p');
           titleElement.innerText = title;

           let descriptionElement = document.createElement('div');
           descriptionElement.innerText = description;

           let logoElement = document.createElement('img');
           logoElement.src = logoUrl;

           let ctaElement = document.createElement('button');
           ctaElement.innerText = cta;

           let adChoiceClickElement = document.createElement('a');
           adChoiceClickElement.href = adChoiceClickUrl;

           let adChoiceImgElement = document.createElement('img');
           adChoiceImgElement.src = adChoiceImageUrl;

           adChoiceClickElement.appendChild(adChoiceImgElement)

           anchorElement.appendChild(imgElement);
           anchorElement.appendChild(titleElement);
           anchorElement.appendChild(descriptionElement);
           anchorElement.appendChild(ctaElement);
           anchorElement.appendChild(logoElement);

           nativeSlot.appendChild(anchorElement);
           nativeSlot.appendChild(adChoiceClickElement);
  }
       onLoaded: (unitId) => console.log(unitId),
       onFailed: (error) => console.log(error),
     },
   });
</script>
```

3. 네이티브 광고 보여주기

```html
<body>
  <div id="momento-ad-native"></div>
</body>
```
