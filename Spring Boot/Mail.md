# Comprehensive Guide to Sending Emails with JavaMail API

---

## Table of Contents

1. **Understanding the Core Components**
2. **Configuring SMTP Properties**
3. **Authentication and Security**
4. **Creating the Mail Session**
5. **Constructing Email Messages (`MimeMessage`)**
6. **Setting Email Headers and Content**
7. **Adding Attachments and Inline Resources**
8. **Handling Multiple Recipients**
9. **Sending the Email**
10. **Error Handling and Debugging**
11. **Best Practices and Tips**
12. **Sample Complete Code**
13. **Common Issues and Solutions**

---

## 1. Understanding the Core Components

| Component         | Description                  | Details                                    |
| ----------------- | ---------------------------- | ------------------------------------------ |
| `Properties`      | Configuration for connection | SMTP host, port, security settings         |
| `Authenticator`   | Authentication provider      | Supplies username/password for SMTP server |
| `Session`         | Context for mail operations  | Combines properties and authenticator      |
| `MimeMessage`     | Email message                | Headers, content, recipients, attachments  |
| `InternetAddress` | Email address representation | Validates and formats email addresses      |
| `Transport`       | Sending mechanism            | Connects and sends message via SMTP        |

---

## 2. Configuring SMTP Properties

Set the connection details based on your SMTP server:

```java
Properties props = new Properties();
props.put("mail.smtp.host", "smtp.gmail.com");
props.put("mail.smtp.port", "587"); // or 465 for SSL
props.put("mail.smtp.auth", "true");
props.put("mail.smtp.starttls.enable", "true"); // For TLS
// For SSL (port 465), use:
// props.put("mail.smtp.socketFactory.port", "465");
// props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
```

**Note:**

- Use port 587 with STARTTLS (recommended).
- Use port 465 with SSL if required.

---

## 3. Authentication and Security

Create an `Authenticator` to provide login credentials:

```java
Authenticator auth = new Authenticator() {
    @Override
    protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication("your_email@gmail.com", "your_password");
    }
};
```

**Important:**

- Never hardcode credentials in production.
- Use environment variables or secure credential storage.
- Gmail requires "App Passwords" if 2FA is enabled.

---

## 4. Creating the Mail Session

Tie properties and authentication into a `Session`:

```java
Session session = Session.getInstance(props, auth);
```

- Optionally, enable debugging:

```java
session.setDebug(true);
```

This helps troubleshoot connection issues.

---

## 5. Constructing the Email Message (`MimeMessage`)

Create the message object:

```java
MimeMessage message = new MimeMessage(session);
```

---

## 6. Setting Email Headers and Content

### **Set Sender Address**

```java
message.setFrom(new InternetAddress("from@gmail.com"));
```

### **Set Recipients**

- To:

```java
message.setRecipient(Message.RecipientType.TO, new InternetAddress("to@example.com"));
```

- CC or BCC:

```java
message.setRecipient(Message.RecipientType.CC, new InternetAddress("cc@example.com"));
message.setRecipient(Message.RecipientType.BCC, new InternetAddress("bcc@example.com"));
```

- Multiple recipients:

```java
InternetAddress[] recipients = {
    new InternetAddress("recipient1@example.com"),
    new InternetAddress("recipient2@example.com")
};
message.setRecipients(Message.RecipientType.TO, recipients);
```

### **Set Subject**

```java
message.setSubject("Subject of the Email");
```

### **Set Content**

- Plain text:

```java
message.setText("This is a plain text message");
```

- HTML content:

```java
message.setContent("<h1>Hello!</h1><p>This is an HTML email.</p>", "text/html");
```

- Attachments and multipart content require `MimeMultipart`.

---

## 7. Adding Attachments and Inline Resources

### **Attachments:**
Multipurpose Internet Mail Extensions
```java
MimeBodyPart messageBodyPart = new MimeBodyPart();
messageBodyPart.setText("This is message body");
MimeBodyPart attachmentPart = new MimeBodyPart();
DataSource source = new FileDataSource("path/to/file");
attachmentPart.setDataHandler(new DataHandler(source));
attachmentPart.setFileName("filename.ext");

Multipart multipart = new MimeMultipart();
multipart.addBodyPart(messageBodyPart);
multipart.addBodyPart(attachmentPart);

message.setContent(multipart);
```

### **Inline Images:**

Embed images within HTML email by referencing a Content-ID:

```java
MimeBodyPart imagePart = new MimeBodyPart();
DataSource fds = new FileDataSource("path/to/image.jpg");
imagePart.setDataHandler(new DataHandler(fds));
imagePart.setHeader("Content-ID", "<image1>");
multipart.addBodyPart(imagePart);

// In HTML content:
String htmlText = "<html><body><img src='cid:image1'></body></html>";
message.setContent(htmlText, "text/html");
```

---

## 8. Handling Multiple Recipients and Types

You can specify multiple recipients of different types:

```java
message.setRecipients(Message.RecipientType.TO, InternetAddress.parse("user1@example.com, user2@example.com"));
message.setRecipients(Message.RecipientType.CC, InternetAddress.parse("cc1@example.com, cc2@example.com"));
message.setRecipients(Message.RecipientType.BCC, InternetAddress.parse("bcc1@example.com"));
```

---

## 9. Sending the Email

```java
Transport.send(message);
```

- Under the hood, `Transport` connects to SMTP, authenticates, and transmits the message.

---

## 10. Error Handling and Debugging

Wrap sending in try-catch:

```java
try {
    Transport.send(message);
    System.out.println("Email sent successfully");
} catch (MessagingException e) {
    e.printStackTrace();
}
```

Enable debugging to see the SMTP conversation:

```java
session.setDebug(true);
```

---

## 11. Best Practices and Tips

- **Use SSL/TLS properly:** Prefer port 587 with STARTTLS.
- **Handle exceptions carefully:** Log errors for troubleshooting.
- **Secure credentials:** Use environment variables or encrypted storage.
- **Validate email addresses:** Use `InternetAddress.parse()` for multiple addresses.
- **Test with different content types:** Plain text, HTML, attachments.
- **Respect email size limits:** Keep emails within reasonable size.

---

## 12. Complete Example: Sending an Email with Attachment and HTML Content

```java
import javax.activation.*;
import javax.mail.*;
import javax.mail.internet.*;
import java.util.Properties;

public class EmailWithAttachment {
    public static void main(String[] args) {
        // SMTP server configuration
        Properties props = new Properties();
        props.put("mail.smtp.host", "smtp.gmail.com");
        props.put("mail.smtp.port", "587");
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.starttls.enable", "true");

        // Authentication
        Authenticator auth = new Authenticator() {
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication("your_email@gmail.com", "your_password");
            }
        };

        // Create session
        Session session = Session.getInstance(props, auth);
        session.setDebug(true);

        try {
            // Compose message
            MimeMessage message = new MimeMessage(session);
            message.setFrom(new InternetAddress("from@gmail.com"));
            message.setRecipients(Message.RecipientType.TO,
                    InternetAddress.parse("to@example.com"));
            message.setSubject("HTML Email with Attachment");

            // Create multipart content
            MimeMultipart multipart = new MimeMultipart();

            // HTML body part
            MimeBodyPart htmlPart = new MimeBodyPart();
            String htmlContent = "<h1>Hello!</h1><p>This is an HTML email with an attachment.</p>";
            htmlPart.setContent(htmlContent, "text/html");
            multipart.addBodyPart(htmlPart);

            // Attachment part
            MimeBodyPart attachmentPart = new MimeBodyPart();
            DataSource source = new FileDataSource("path/to/file.pdf");
            attachmentPart.setDataHandler(new DataHandler(source));
            attachmentPart.setFileName("file.pdf");
            multipart.addBodyPart(attachmentPart);

            // Set content
            message.setContent(multipart);

            // Send email
            Transport.send(message);
            System.out.println("Email sent successfully!");

        } catch (MessagingException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 13. Common Issues and Solutions

| Issue | Possible Cause | Solution |
|---------|----------------|----------|
| Authentication failed | Wrong credentials or security settings | Verify username/password, enable app-specific passwords |
| Connection refused | SMTP server blocking connection | Check server address, port, firewall rules |
| SSL/TLS errors | Incorrect port or security protocol | Use correct port and enable security properties |
| Email marked as spam | Content or sender reputation | Use proper headers, authenticate with SPF/DKIM |

---

# Final Notes

- Always test with a small subset of recipients.
- Review email provider limits.
- Keep security in mind when handling credentials.
- Use libraries like JavaMail API effectively for complex email features.
