# Ajax

Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

Ajx는 브라우저에서 제공하는 Web API인 XMLHttpRequest 객체를 기반으로 동작한다. XMLHttpRequest 객체는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.

이전의 웹 페이즈는 HTML 태그로 시작해서 HTML 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다. 따라서 화면이 전환되면 서버로부터 새로운 HTML을 전송받아
웹페이지 전체를 처음부터 다시 렌더링했다. 이러한 전통적인 방식은 다음과 같은 단점이 있다.

1. 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
2. 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 화면 전환이 일어나면 화면이 순간저긍로 깜빡이는 현상이 발생한다.
3. 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.

> Ajax의 등장은 이전의 전통적인 패러다임을 획기적으로 전환했다. 즉, 서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해진 것이다.

## Ajax의 장점

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
2. 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

## JSON

JSON은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

### JSON 표기 방식

JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.

```javascript
{
    "name": "Lee",
    "age": 20,
    "alive": true,
    "hobby": ["traveling","tennis"]
}
```

JSON의 키는 반드시 큰따옴표로 묶어야 한다. 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰 따옴표로 묶어야 한다.

### JSON.stringify

JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야하는데 이를 직렬화(Serializing)한다.

```javascript
const obj = {
    name: 'Lee',
    age: 20,
    alive: true,
    hobby: ['traveling','tennis']
};

const json = JSON.stringify(obj);
console.log(typeof json, json);
//string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}


const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);

/*
string {
    "name": "Lee",
    "age": 20,
    "alive": true,
    "hobby": [
        "traveling",
        "tennis"
    ]
}
*/
```

JSON.stringify 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.

```javascript
const todos = [
    { id: 1, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);

/*
string [
    {
        "id": 1,
        "content": "CSS",
        "completed": false
    },
    {
        "id": 2,
        "content": "CSS",
        "completed": true
    },
    {
        "id": 3,
        "content": "Javascript",
        "completed": false
    }
]
*/
```

### JSON.parse

JSON.parse 메서드는 JSON 포맷의 무자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야하는데 이를 역직렬화(Deserializing)이라 한다.

```javascript
const obj = {
    name: 'Lee',
    age: 20,
    alive: true,
    hobby: [ 'traveling','tennis' ]
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);

// JSON 포맷의 문자열을 객체로 변환한다.
const parsed = JSON.parse(json);

console.log(typeof parsed, parsed);
// object { name: "Lee", age:20, alive:true, hobby:[ "traveling","tennis" ]}
```

배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

```javascript
const todos = [
    { id: 1, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false }
];

// 배열을 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(todos);

// JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);

/*
    object [
        { id: 1, content: 'HTML', completed: false },
        { id: 2, content: 'CSS', completed: true },
        { id: 3, content: 'Javascript', completed: false }
    ]
*/
```

## XMLHttpRequest

브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

## XMLHttpRequest 객체 생성

XMLHttpRequest 객체는 생성자 함수를 호출하여 생성한다. XMLHttpRequest 객체는 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.

```javascript
const xhr = new XMLHttpRequest();
```

## XMLHttpRequest 객체의 프로퍼티와 메서드

### XMLHttpRequest의 프로토타입 프로퍼티

|프로토타입 프로퍼티|설명|
|----|----|
|readyState|HTTP 요청으로 현재 상태를 나타내는 정수. 다음과 같은 XMLHttpRequest의 정적 프로퍼티를 같는다. UNSERT, OPENED, HEADERS_RECEIVED, LOADING, DONE 순서대로 0부터 4의 값을 같는다.|
|stats| HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수|
|statusText|HTTP 요청에 대한 응답 메세지를 나타내는 문자열|
|responseType|HTTP 응답 타입
|reponse|HTTP 요청에 대한 응답 몸체, responseType에 따라 타입이 다르다.
|responseText|서버가 전송한 HTTP 요청에 대한 응답 문자열|

### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

|이벤트 핸들러 프로퍼티|설명|
|---|---|
|onreadystatechange|readyState 프로퍼티 값이 변경된 경우|
|onloadstart|HTTP 요청에 대한 응답을 받기 시작한 경우|
|onprogress|HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|
|onabort|abort 메서드에 의해 HTTP 요청이 중단된 경우|
|onerror|HTTP 요청에 에러가 발생한 경우|
|onload|HTTP 요청이 성공적으로 완료한 경우|
|ontimeout|HTTP 요청 시간이 초과한 경우|
|onloadend|HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패하면 발생|

### XMLHttpRequest 객체의 메서드

|메서드|설명|
|---|---|
|open|HTTP 요청 초기화|
|send|HTTP 요청 전송|
|abort|이미 전송된 HTTP 요청 중단|
|setRequestHeader|특정 HTTP 요청 헤더의 값을 설정|
|getResponseHeader|특정 HTTP 요청 헤더의 값을 문자열로 반환|

### XMLHttpRequest 객체의 정적 프로퍼티

| 정적 프로퍼티 | 값 | 설명 |
| --- | --- | --- |
| UNSERT | 0 | open 메서드 호출 이전 |
| OPENED | 1 | open 메서드 호출 이후 |
| HEADERS_RECEIVED | 2 | send 메서드 호출 이후 |
| LOADING | 3 | 서버 응답 중 (응답 데이터 미완성 상태) |
| DONE | 4 | 서버 응답 완료 |

## HTTP 요청 전송

HTTP 요청을 전송하는 경우 다음 순서를 따른다.

1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET','/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

### XMLHttpRequest.prototype.open

open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다.

```javascript
xhr.open(method, url[, async]);
```

|매개변수|설명|
|---|---|
|method|HTTP 요청 메서드("GET","POST",'PUT","DELETE" 등)|
|url| HTTP 요청을 전송할 URL|
|async|비동기 요청 여부, 옵션으로 기본값은 true이며, 비동기 방식으로 동작한다.|

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)를 알리는 방법이다. 주로 5가지의 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현한다.

|HTTP 요청 메서드|종류|목적|페이로드|
|---|---|---|---|
|GET|index/retrieve|모든/특정 리소스 취득|X|
|POST|create|리소스 생성|O|
|PUT|replace|리소스의 전체 교체|O|
|PATCH|modify|리소스의 일부 수정|O|
|DELETE|delete|모든/특정 리소스 삭제|X|

### XMLHttpRequest.prototype.send

send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.

- GET 요청 메서드의 경우 데이터를 URL의 일부분인 **쿼리 문자열**로 서버에 전송한다.
- POST 요청 메서드의 경우 데이터(**페이로드**)를 요청 몸체에 담아 전송한다.

send 메서드에는 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다. 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달해야 한다.

```javascript
xhr.send(JSON.stringify({ id:1, content:'HTML', completed: false }));
```

**HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달된 인수는 무시되고 요청 몸체는 null로 설정된다.**

### XMLHttpRequest.prototype.serRequestHeader

setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야 한다. 자주 사용하는 HTTP 요청 헤더인 Content-type과 Accept에 대해 살펴보자.

Content-type은 요청 몸체에 담아 전솔할 데이터의 MIME 타입의 정보를 표현한다. 자주 사용되는 MIME 타입은 다음과 같다.

|MIME 타입|서브타입|
|---|---|
|text|text/plain, text/html, text/css, text/javascript|
|application|application/json, application/x-www-form-urlencode|
|multipart|multipart/formed-data|

다음은 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정하는 예다.

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('POST', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.serRequestHeader('content-type','application/json');

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있다. 다음은 서버가 응답할 데이터의 MIME 타입을 지정하는 예다.

```javascript
// 서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('accept','application/json');
```

만약 Accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 Accept 헤더가 */*로 전송된다.

## HTTP 응답 처리

서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.

XMLHttpRequest 객체는 onreadystatechange, onload, onerror 같은 이벤트 핸들러 프로퍼티를 갖는다. 이 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 다음과 같이 HTTP 응답을 처리할 수 있다.

XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 다음 예제는 반드시 **브라우저 환경에서 실행해야 한다.**

참고로 HTTP 요청을 전송하고 응답을 받으려면 서버가 필요하다. 다음 예제에서는 JSONPlaceholder에 서 제공하는 가상 REST API를 사용한다.

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// http://jsonplaceholder.typicode.com은 fake REST API를 제공하는 서비스이다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readystate 프로퍼티가 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
    if (xhr.readyState !== XMLHttpRequest.DONE) return;

    if (xhr.status === 200) {
        console.log(JSON.parse(xhr.reponse));
    } else {
        console.log('Error', xhr.status, xhr.statusText);
    }
};
```

send 메서드를 통해 HTTP 요청을 서버에 전송하면 서버는 응답을 반환한다. 하지만 언제 응답이 클라이언트에 도달하는지는 알 수 없다. 따라서 readystatuschange 이벤트를 통해 HTTP 요청의 현재 상태를 확인해야 한다. readystatuschange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때 마다 발생한다.

onreadystatechange 이벤트 핸들러 프로퍼티에 할당한 이벤트 핸들러는 HTTP 요청의 현재 상태를 나타내는 xhr.readyStat가 XMLHttpRequest.DONE인지 확인하여 서버의 응답이 완료되었는지 확인한다. (상태코드 200)

readystatechange 이벤트 대신 load 이벤트를 캐치해도 좋다. load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다. 따라서 load 이벤트를 캐치하는 경우 xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요가 없다.

```javascript
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET','https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.load = () => {
    if (xhr.status === 200) {
        console.log(JSON.parse(xhr.response));
    } else {
        console.error('Error', xhr.status, xhr.statusText);
    }
}
```
