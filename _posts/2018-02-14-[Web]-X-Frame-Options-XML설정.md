웹에서 일부 요소들은 응답에서 `X-Frame-Options` 헤더 설정이 `DENY` 로 되어 있을 경우 표시가 되지 않는다. 대표적으로 iframe과 같은 요소가 있다. 나모크로스에디터 또한 iframe을 사용하기 때문에 이 헤더 설정이 `DENY` 로 되어 있으면 변경해주어야 한다. Spring 환경에서 Tomcat 서버를 사용할 경우에는 web.xml 설정으로 간단히 해결할 수 있다.

> 개발 환경: Spring, Tomcat

### Web.xml

```xml
	<!-- X-Frame-Options: 값이 DENY이면 나모크로스에디터 오류 발생 -->
	<filter>
		<filter-name>httpHeaderSecurity</filter-name>
		<filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
		<init-param>
			<param-name>antiClickJackingOption</param-name>
			<param-value>SAMEORIGIN</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>httpHeaderSecurity</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
```

출처: https://stackoverflow.com/a/40276959/5722210
