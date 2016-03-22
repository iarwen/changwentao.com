title: VBS发送邮件
date: 2016/03/22 18:38:25
categories:
- vbs
tags: [vbs,email]
---
没啥说的，直接代码 注意Textbody的编码
```
NameSpace = "http://schemas.microsoft.com/cdo/configuration/"
Set Email = CreateObject("CDO.Message")
Email.From = "360**492*@qq.com"
Email.To = "chang*****@126.com"
Email.Subject = "主题"
Email.Textbody = "内容"
With Email.Configuration.Fields
.Item(NameSpace&"sendusing") = 2
.Item(NameSpace&"smtpserver") = "smtp.qq.com"
.Item(NameSpace&"smtpserverport") = 25
.Item(NameSpace&"smtpauthenticate") = 1
.Item(NameSpace&"sendusername") = "36**8492*"
.Item(NameSpace&"sendpassword") = "********"
.Update
End With
Email.Send
```