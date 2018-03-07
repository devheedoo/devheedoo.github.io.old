프로젝트 중 하나에 큰 문제가 있다. https를 적용할 경우 IE에서만 폰트가 전부 깨지는 현상이 발생하는 것이다. 특이하게도 URL을 통해 직접 접근했을 때 첫 화면에서만 폰트가 깨진다. 이후 페이지 내에서 링크로 이동할 경우 정상적으로 동작한다.

> IE, https, Cache-Control, font 관련 키워드로 참 많이 검색했다...

관련된 이슈를 찾다보니 결국 `Response Header` 설정을 고쳐줘야했다. 리소스 파일에 대해 브라우저 캐싱을 설정할 것이다.  개발 환경은 Spring이다.

> 사실 캐싱을 막는다고 하더라도 왜 처음에만 폰트가 깨지는지 정확히 이해하진 못했다...



## HTTP 헤더 적용

추가 생성한 파일(2):

-  `PragmaFilter.java` 
- `CacheControlFilter.java`

수정한 파일(1):

-   `web.xml` 

### PragmaFilter.java

HttpServletResponseWrapper 객체를 확장해서 super를 이용해 중간에 헤더 셋팅 과정을 조작한다.

```java
package 패키지경로;

import javax.servlet.*;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpServletResponseWrapper;

import java.io.IOException;

/**
 * PragmaFilter
 * IE, https 환경에서 폰트 깨짐현상이 발생했다. URL로 직접 입력했을 시에만 발생했다.
 * 이를 해결하기 위해 css 파일에 대해 캐싱 설정하는 필터이다.
 * - Pragma: no-cache 헤더 제거
 * @author devheedoo
 */
public class PragmaFilter implements Filter {

    private static String PRAGMA_HEADER = "Pragma";
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
                         FilterChain chain) throws IOException, ServletException {
        chain.doFilter(request, new cacheUsingHttpServletResponseWrapper(response));
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}

    @Override
    public void destroy() {}
    
    private final class cacheUsingHttpServletResponseWrapper
        extends HttpServletResponseWrapper {
        
        private cacheUsingHttpServletResponseWrapper(ServletResponse response) {
            super((HttpServletResponse) response);
        }
        
        @Override
        public void addHeader(String name, String value) {
            if (PRAGMA_HEADER.equals(name)) {
                return;
            }
            super.addHeader(name, value);
        }
        
        @Override
        public void setHeader(String name, String value) {
            if (PRAGMA_HEADER.equals(name)) {
                return;
            }
            super.setHeader(name, value);
        }
    }
    
}
```

### CacheControlFilter.java

```java
package 패키지경로;
    
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import java.io.IOException;

/**
 * CacheControlFilter
 * IE, https 환경에서 폰트 깨짐현상이 발생했다. URL로 직접 입력했을 시에만 발생했다.
 * 이를 해결하기 위해 css 파일에 대해 캐싱 설정하는 필터이다.
 * - Cache-Control: public, max-age=86400 헤더 설정
 * @author devheedoo
 */
public class CacheControlFilter implements Filter {
    
    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
                         FilterChain chain) throws IOException, ServletException {
        
        HttpServletResponse resp = (HttpServletResponse) response;
        resp.setHeader("Cache-Control", "public, max-age=86400");
        chain.doFilter(request, response);
    }

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        // TODO Auto-generated method stub
    }

    @Override
    public void destroy() {
        // TODO Auto-generated method stub
    }
    
}
```

### web.xml

web.xml 파일에서 위 두 필터의 적용 대상과 적용 순서를 설정한다.

두 필터의 위치를 이리저리 바꿔본 결과, PragmaFilter - 나머지 필터 - CacheControlFilter 순서가 되었다.

```xml
...
	<!-- PragmaFilter -->
	<filter>
		<filter-name>PragmaFilter</filter-name>
		<filter-class>com.a2m.webapps.webCommons.PragmaFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>PragmaFilter</filter-name>
		<url-pattern>*.css</url-pattern>
		<url-pattern>*.js</url-pattern>
		<url-pattern>*.gif</url-pattern>
		<url-pattern>*.png</url-pattern>
		<url-pattern>*.jpg</url-pattern>
		<url-pattern>*.eot</url-pattern>
		<url-pattern>*.woff</url-pattern>
		<url-pattern>*.woff2</url-pattern>
		<url-pattern>*.ttf</url-pattern>
		<url-pattern>*.svg</url-pattern>
		<url-pattern>*.mp4</url-pattern>
	</filter-mapping>
...
	<!-- 다른 필터들 -->
...
	<!-- CacheControlFilter -->
	<filter>
		<filter-name>CacheControlFilter</filter-name>
		<filter-class>com.a2m.webapps.webCommons.CacheControlFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>CacheControlFilter</filter-name>
		<url-pattern>*.css</url-pattern>
		<url-pattern>*.js</url-pattern>
		<url-pattern>*.gif</url-pattern>
		<url-pattern>*.png</url-pattern>
		<url-pattern>*.jpg</url-pattern>
		<url-pattern>*.eot</url-pattern>
		<url-pattern>*.woff</url-pattern>
		<url-pattern>*.woff2</url-pattern>
		<url-pattern>*.ttf</url-pattern>
		<url-pattern>*.svg</url-pattern>
		<url-pattern>*.mp4</url-pattern>
	</filter-mapping>
...
```

#### 필터 적용 순서

필터 적용 순서는 Spring 어노테이션을 사용할 경우 따로 설정할 수 없다. web.xml 파일을 사용할 경우 위에서부터 아래로 적용된다. 필터는 입력, 출력 한 번씩 적용되기 때문에 아래 그림을 참고해서 적절하게 배치해야한다.

![Java필터개념도](http://cfs12.tistory.com/image/18/tistory/2008/12/08/14/39/493cb30ae3dd0)

필터 적용 순서 출처: http://javacan.tistory.com/entry/58



## 결과

### 적용 전 xxx.woff 파일의 Response Header

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-XSS-Protection: 1; mode=block
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
Last-Modified: Tue, 06 Dec 2016 04:17:46 GMT
Accept-Ranges: bytes
Content-Type: application/x-font-woff
Content-Length: 210896
Date: Tue, 13 Feb 2018 04:09:24 GMT
```

### 적용 후 xxx.woff 파일의 Response Header

Cache-Control 헤더 값이 변경되고, Pragma 헤더가 제거되었다. (캐싱 가능하도록 바뀐 것)

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Cache-Control: public, max-age=86400
Expires: 0
X-XSS-Protection: 1; mode=block
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
Last-Modified: Mon, 19 Dec 2016 10:36:17 GMT
Accept-Ranges: bytes
Content-Type: application/x-font-woff
Content-Length: 210300
Date: Tue, 13 Feb 2018 08:52:01 GMT
```


