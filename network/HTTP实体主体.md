
HTTP 实体的主体部分（entity-body）

实体的主体是 HTTP 要传输的内容，通过Content-Type 首部字段说明了实体主体的 MIME 类型。
MIME 类型（MIME type）: 表示一种主要的对象类型和一个特定的子类型。在HTTP1.0中通过字段Content-Type进行设置。

| 说明                       | 标记                          |
| -------------------------- | ----------------------------- |
| 普通的 ASCII 文本文档      | text/plain                    |
| HTML 格式的文本文档        | text/html                     |
| CSS格式的文本文档          | text/css                      |
| JPEG 格式的图片            | image/jpeg                    |
| GIF 格式的图片             | image/gif                     |
| Apple 的 QuickTime 电影    | video/quicktime               |
| 微软的 PowerPoint 演示文件 | application/vnd.ms-powerpoint |
| javascript格式的文本文档   | application/javascript        |
| pdf 文本文档               | application/pdf               |
| 完整文档中不同的字节范围   | multipart/byteranges          |

> Content-Type 首部还支持可选的参数来进一步说明内容的类型， 如：Content-Type: text/html; ==charset=iso-8859-4==

多部分表格提交：当提交填写的 HTTP 表格时，变长的文本字段和上传的对象都作为多部分主体里面独立的部分发送，这样表格中就可以填写各种不同类型和长度的值。

```
Content-Type: multipart/form-data; boundary=AaB03x
--AaB03x
Content-Disposition: form-data; name="submit-name"
Sally
--AaB03x
Content-Disposition: form-data; name="files"; filename="essayfile.txt"
Content-Type: text/plain
...contents of essayfile.txt...
--AaB03x--
```

多部分范围响应：HTTP 对范围请求的响应也可以是多部分的，Content-Type: multipart/byteranges 首部和带有不同范围的多部分主体。

```
HTTP/1.0 206 Partial content
Server: Microsoft-IIS/5.0
Date: Sun, 10 Dec 2000 19:11:20 GMT
Content-Location: http://www.joes-hardware.com/gettysburg.txt
Content-Type: multipart/x-byteranges; boundary=--[abcdefghijklmnopqrstu
vwxyz]--
Last-Modified: Sat, 09 Dec 2000 00:38:47 GMT

--[abcdefghijklmnopqrstuvwxyz]--
Content-Type: text/plain
Content-Range: bytes 0-174/1441
Fourscore and seven years ago our fathers brought forth on this
continent a new nation, conceived in liberty and dedicated to the
proposition that all men are created equal.
--[abcdefghijklmnopqrstuvwxyz]--
Content-Type: text/plain
Content-Range: bytes 552-761/1441

But in a larger sense, we can not dedicate, we can not consecrate,
we can not hallow this ground. The brave men, living and dead who
struggled here have consecrated it far above our poor power to add
or detract.
--[abcdefghijklmnopqrstuvwxyz]--
Content-Type: text/plain
Content-Range: bytes 1344-1441/1441

and that government of the people, by the people, for the people shall
not perish from the earth.

--[abcdefghijklmnopqrstuvwxyz]--
```

