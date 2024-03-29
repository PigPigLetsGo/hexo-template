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

```xml
<dependency>
    <groupId>com.sun.activation</groupId>
    <artifactId>jakarta.activation</artifactId>
    <version>2.0.1</version>
</dependency>
```

```java
package com.dkx.pojo;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component("properties")
@ConfigurationProperties(prefix = "mail")
@SuppressWarnings("all")
public class EmailProperties {
    // 发件人邮箱
    // @Value("${mail.user}")
    public String user;
    // 发件人邮箱授权码
    // @Value("${mail.code}")
    public String code;
    // 发件人邮箱对应的服务器域名，如果是163邮箱：smtp.163.com，qq邮箱：smtp.qq.com
    // @Value("${mail.host}")
    public String host;
    // 身份验证开关
    // @Value("${mail.auth}")
    private boolean auth;

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public boolean isAuth() {
        return auth;
    }

    public void setAuth(boolean auth) {
        this.auth = auth;
    }
}
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