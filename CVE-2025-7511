# Chat System Using PHP has sql injection in /user/update_account.php



## supplier



https://code-projects.org/chat-system-using-php-source-code/



## Vulnerability file



/user/update_account.php



## describe


The mname, mpassword and musername parameter in /user/update_account.php is not properly sanitized or parameterized, which leaves it vulnerable to SQL injection attacks. Attackers can exploit this by injecting malicious SQL code to manipulate the database queries. Utilizing time-based SQL injection methods, they can introduce intentional delays in the database response through functions such as SLEEP(). This technique can be employed to verify the existence of the vulnerability and may also be used to extract sensitive information from the database.



## **Code analysis**



```php
<?php
	include('session.php');
	
	$mname=$_POST['mname'];
	$cpassword=md5($_POST['cpassword']);
	$apassword=md5($_POST['apassword']);
	$mpassword=$_POST['mpassword'];
	$musername=$_POST['musername'];

	$myq=mysqli_query($conn,"select * from `user` where userid='".$_SESSION['id']."'");
	$myqrow=mysqli_fetch_array($myq);
	
	if ($cpassword!=$apassword){
		?>
		<script>
			window.alert('Verification Password did not match!');
			window.history.back();
		</script>
		<?php
	}
	
	elseif ($cpassword!=$myqrow['password']){
		?>
		<script>
			window.alert('Current Password did not match!');
			window.history.back();
		</script>
		<?php
	}
	
	else{
		if ($mpassword==$myqrow['password']){
			$newpassword=$mpassword;
		}
		else{
			$newpassword=md5($mpassword);
		}
		
		mysqli_query($conn,"update `user` set username='$musername', password='$newpassword', uname='$mname' where userid='".$_SESSION['id']."'");
		?>
		<script>
			window.alert('Changes Saved!');
			window.history.back();
		</script>
		<?php
	}

?>
```

Inserting $_POST['mname'] into Mysql query without any filter. A time-based blind SQL injection can be triggered.





## POC

```
mname=%27+OR+(SELECT+SLEEP(5))+--+&musername=EXAMPLE_NAME&newpassword=EXAMPLE_PASSWORD
```

Send this request, you can observe an additional 5 seconds time delay triggered by the time-based injection.
