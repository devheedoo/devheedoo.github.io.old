출처: https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Cache-Control

캐시 관련된 헤더 중에서 직접 변경하거나 주요 설정으로 생각되는 부분만 간추려서 작성했다.

## 헤더1: Cache-Control

요청과 응답 내의 캐싱 메커니즘을 설정한다. 요청과 응답에서 설정이 다를 수 있다. 

#### 문법

##### 요청 디렉티브

```
Cache-Control: max-age=<seconds>
Cache-control: no-cache 
Cache-control: no-store
Cache-control: no-transform
```

##### 응답 디렉티브

```
Cache-control: must-revalidate
Cache-control: no-cache
Cache-control: no-store
Cache-control: no-transform
Cache-control: public
Cache-control: private
Cache-Control: max-age=<seconds>
```

#### 디렉티브

##### 캐시 능력

- public: 캐시
- private: 단일 사용자를 위한 캐시
- no-cache: 캐시된 복사본을 사용자에게 보여주기 전에, 서버로 재검증을 요청하도록 강제

##### 만료

- max-age=[seconds]: 리소스 캐시 기간

##### 재검증과 리로딩

- must-revalidate: 리소스가 만료되었는지 확인 요청

##### 기타

- no-store: 요청 혹은 응답에 대해 어떤 것도 저장하지 않음
- no-transform: 프록시에 의해 수정되지 않음

> no-transform 디렉티브의 경우 웹방화벽이 중간에 수정하지 않도록 할 때 사용할 수 있을 것 같다.

#### 예제

##### 캐싱 막기

```
Cache-Control: no-cache, no-store, must-revalidate
```

##### 정적 에셋 캐싱

```
Cache-Control: public, max-age=86400
```

## 헤더2: Expires

응답 만료 일시를 지정한다. 0과 같이 유효하지 않은 날짜의 경우 만료를 의미한다.

> 응답 내에 Cach-Control: max-age 헤더가 존재할 경우, Expires 헤더는 무시된다.

#### 예제

```
Expires: Wed, 21 Oct 2015 07:28:00 GMT
```

## 헤더3: Pragma

HTTP/1.0 의 Pragma 헤더는 요청-응답에 다양한 영향을 주는 헤더이다. HTTP/1.1 버전의 Cache-Control 헤더와 같은 역할을 담당했다.

> 응답에서 Cache-Control 헤더가 생략되었을 경우, Cache-Control: no-cache 와 동일한 효과를 준다. 하지만 HTTP/1.0 을 사용하는 클라이언트를 위한 비공식적인 호환성을 위해서 사용하는 것이 옳다.

#### 디렉티브

- no-cache: 캐시가 복사본을 보여주기 전에 서버로 유효성 검사를 요청

#### 예제

```
Pragma: no-cache
```

> Pragma 는 디렉티브가 이것뿐이다.

