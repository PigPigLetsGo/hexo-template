---
title: MailUtil工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

需要的依赖

```xml
<!--java mail依赖-->
<dependency>
   <groupId>org.eclipse.angus</groupId>
   <artifactId>jakarta.mail</artifactId>
</dependency>
```

```java
public class MailUtil {
    /**
     * 发送邮件
     * @param emailProperties 发件人信息 (发件人邮箱，发件人授权码) 及邮件服务器信息 (邮件服务器域名，身份验证开关)
     * @param to 收件人邮箱
     * @param title 邮件标题
     * @param content 邮件正文
     * @return
     */
    public static boolean sendMail(EmailProperties emailProperties, String to, String title, String content) {
        Properties props = new Properties();
        props.put("mail.smtp.host", emailProperties.host);
        props.put("mail.smtp.auth", emailProperties.isAuth());
        Session session = Session.getInstance(props, new Authenticator() {
            @Override
            protected PasswordAuthentication getPasswordAuthentication() {
                if (Boolean.valueOf(emailProperties.isAuth())) {
                    PasswordAuthentication auto =
                            new PasswordAuthentication(emailProperties.user, emailProperties.code);
                    return auto;
                }
                return super.getPasswordAuthentication();
            }
        });
        try {
            MimeMessage msg = new MimeMessage(session);
            msg.setFrom(emailProperties.user);
            msg.setRecipients(Message.RecipientType.TO,
                    to);
            msg.setSubject(title);
            msg.setSentDate(new Date());
            msg.setText(content + "\n");
            Transport.send(msg);
        } catch (MessagingException e) {
            System.out.println("send failed, exception: " + e);
            return false;
        }
        return true;
    }
}
```