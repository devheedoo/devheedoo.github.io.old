javax-mail 사용 내용을 정리했다.

## 사용 중인 자바 메일링 코드 구조

Email.java, EmailSender.java, Controller.java, dispatcher-servlet.xml 연동

### Email.java

```java
public class Email {
    private String subject;
    private String content;
    private String receiver;
    private String articleId;
    private String writer;
    private String rgstDate;
    // ... getter, setter
}
```

### EmailSender.java

```java
import javax.mail.MessagingException;
import javax.mail.Multipart;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.MailException;
import org.springframework.mail.javamail.JavaMailSender;

public class EmailSender {
	@Autowired
	protected JavaMailSender mailSender;
	public void SendEmail(Email email) throws Exception {
		MimeMessage msg = mailSender.createMimeMessage();
		try {
			msg.setSubject("메일 제목");
			Multipart multipart = new MimeMultipart();
			MimeBodyPart htmlPart = new MimeBodyPart();
			htmlPart.setContent("<strong>작성자</strong> : " + email.getWriter() + "<br/><br/>" 
					+ "<strong>작성일시</strong> : " + email.getRgstDate() + "<br/><br/>" 
					+ "<strong>제목</strong> : " + email.getSubject() + "<br/><br/>" 
					+ "<strong>내용</strong> : " + email.getContent(), "text/html; charset=utf-8");
			multipart.addBodyPart(htmlPart);
			msg.setFrom(new InternetAddress("sender@gmail.com")); // 실제 메일 받았을 때 보낸사람 주소로 입력되는 값
			msg.setRecipients(MimeMessage.RecipientType.TO, InternetAddress.parse(email.getReceiver()));
			msg.setContent(multipart);
			msg.saveChanges();
		} catch (MessagingException e) {
			System.out.println("[MessagingException]");
			e.printStackTrace();
		}
		try {
			mailSender.send(msg);
		} catch (MailException e) {
			System.out.println("[MailException]");
			e.printStackTrace();
		}
	}
}

```

### Controller.java

```java
// ...
request.setCharacterEncoding("UTF-8");
SimpleDateFormat fm = new SimpleDateFormat("yyyy-MM-dd");
String aDate = fm.format(new Date());
String MAIL_HDKIM = "devheedoo@gmail.com";
try {
  email.setArticleId("" + artl_rgst_seq);
  email.setWriter(rgst_user_nm);
  email.setRgstDate(aDate);
  email.setSubject(artl_tit);
  email.setContent(artl_cont); // null일 경우 에러 발생
  email.setReceiver(MAIL_HDKIM);
  System.out.println("MAIL SEND TO " + MAIL_HDKIM);
  System.out.println("@@@@@ START @@@@@");
  emailSender.SendEmail(email);
  System.out.println("@@@@@  END  @@@@@");
} catch (Exception e) {
  e.printStackTrace();
}
// ...
```

### dispatcher-servlet.xml

```xml
<!-- 자동알림메일 -->
<beans:bean id="email" class="com.tistory.devhd.mail.Email">
</beans:bean>
<beans:bean id="emailSender" class="com.tistory.devhd.mail.EmailSender">
</beans:bean>
<beans:bean id="mailSender" class ="org.springframework.mail.javamail.JavaMailSenderImpl" >
  <beans:property name="host" value="smtp.gmail.com" />
  <beans:property name="port" value="25" />
  <beans:property name="defaultEncoding" value="utf-8" />
  <beans:property name="username" value="devheedoo@gmail.com" />
  <beans:property name="password" value="PASSWORD" />
  <beans:property name="javaMailProperties">
    <beans:props>
      <beans:prop key="mail.smtp.starttls.enable">true</beans:prop>
      <beans:prop key="mail.smtp.auth">true</beans:prop>
    </beans:props>
  </beans:property>
</beans:bean>
```

## 이슈

### SMTP 인증

운영서버마다 SMTP 인증 방식이 다르다. 내부망 IP 허용으로 될 수도 있고, 서비스용 계정 정보를 받아서 적용해야할 수도 있다.

결국 실제 서비스에 적용하려면 IP 인증방식이 아닌 경우 dispatcher-servlet.xml 파트의 host, port, username, password 정보를 알아야 한다.

테스트 시 구글 SMTP를 사용하려면 인증하려는 구글 계정 설정에서 `내 계정 > 로그인 및 보안 > 연결된 앱 및 사이트 > 보안 수준이 낮은 앱 허용` 을 설정해야 한다.

### 메일 내용

지금은 정확히 기억이 나지 않지만 줄바꿈, html 적용 등의 문제로 위처럼 코드를 작성했던 것 같다. MimeMultipart, MimeBodyPart 객체를 사용했다.

메일 내용 뒷부분에 인코딩 설정도 중요하고, 보낸사람 주소로 입력되는 msg.setFrom 부분도 단순 텍스트를 넣으면 안 된다.

실제 발송되는 메일의 보낸사람 주소가 Email 객체의 writer 혹은 dispatcher-servlet 파일의 username일 줄 알았다. 그런데 msg.setFrom 파라미터 값이었다.