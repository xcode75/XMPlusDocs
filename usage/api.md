# User API


##### Account API

POST `{{apihost}}/api/v2/account`

HEADERS
------------------------------
XMPus-API-Token     md5(<enter_your_api_key>)

JSON BODY
------------------------------
email  <enter_your_login_email>

passwd  <enter_your_login_passwd>

```
<?php
	$curl = curl_init();
	curl_setopt_array($curl, array(
	  CURLOPT_URL => 'https://www.xmplus.dev/api/v2/account',
	  CURLOPT_RETURNTRANSFER => true,
	  CURLOPT_ENCODING => '',
	  CURLOPT_MAXREDIRS => 10,
	  CURLOPT_TIMEOUT => 0,
	  CURLOPT_FOLLOWLOCATION => true,
	  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
	  CURLOPT_CUSTOMREQUEST => 'POST',
	  CURLOPT_POSTFIELDS =>'{
		"email" : "xxx@xmplus.com",
		"passwd" : "12345"
	}',
	  CURLOPT_HTTPHEADER => array(
		'XMPus-API-Token: 02485e4914f905def212ce4793d0xty',
		'Content-Type: application/json'
	  ),
	));

	$response = curl_exec($curl);
	curl_close($curl);
	echo $response;
```

### Register API

POST `{{apihost}}/api/v2/register`

HEADERS
------------------------------
XMPus-API-Token     md5(<enter_your_api_key>)

JSON BODY
------------------------------
email  <enter_your_login_email>

passwd  <enter_your_login_passwd>

aff  <enter_your_inviter_ID>


```
<?php
	$curl = curl_init();
	curl_setopt_array($curl, array(
	  CURLOPT_URL => 'https://www.xmplus.dev/api/v2/register',
	  CURLOPT_RETURNTRANSFER => true,
	  CURLOPT_ENCODING => '',
	  CURLOPT_MAXREDIRS => 10,
	  CURLOPT_TIMEOUT => 0,
	  CURLOPT_FOLLOWLOCATION => true,
	  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
	  CURLOPT_CUSTOMREQUEST => 'POST',
	  CURLOPT_POSTFIELDS =>'{
		"email" : "xxx@xmplus.com",
		"passwd" : "12345",
		"aff : Q1KIgy"
	}',
	  CURLOPT_HTTPHEADER => array(
		'XMPus-API-Token: 02485e4914f905def212ce4793d0xty',
		'Content-Type: application/json'
	  ),
	));

	$response = curl_exec($curl);
	curl_close($curl);
	echo $response;
```