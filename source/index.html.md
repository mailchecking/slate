---
title: mailchecking v1
language_tabs:
  - shell: Shell
  - http: HTTP
  - javascript: JavaScript
  - ruby: Ruby
  - python: Python
  - php: PHP
  - java: Java
  - go: Go

toc_footers:
  - <a href='https://mailcheck.ing/account'>Sign Up for a API Key</a>
  # - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
code_clipboard: true
highlight_theme: darkula
headingLevel: 2


meta:
  - name: description
    content: Documentation for the Kittn API

---

<!-- Generator: Widdershins v4.0.1 -->

# Introduction

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

You can find our REST API documentation here

Base URLs:

* <a href="https://api.mailcheck.ing/v1">https://api.mailcheck.ing/v1</a>

Email: <a href="mailto:contact@mailcheck.ing">mailchecking</a> Web: <a href="https://mailcheck.ing">mailchecking</a> 

# Authentication

* API Key (ApiKeyAuth)
    - Parameter Name: **auth_key**, in: header. API key for authenticated access. You can view and manage your API key in the [Dashboard](https://mailcheck.ing/account). Be sure to keep your API key secure. Do not share it in publicly accessible areas such as GitHub, client-side code, and so forth. All API requests must be made over HTTPS. Calls made over plain HTTP will fail.

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

> To authorize, use auth_key header

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "auth_key: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with your API key.

# verify

## get verify

> Code samples

```shell
# You can also use wget
curl -X GET https://api.mailcheck.ing/v1/verify?email=string \
  -H 'Accept: application/json' \
  -H 'auth_key: string'

```

```http
GET https://api.mailcheck.ing/v1/verify?email=string HTTP/1.1
Host: api.mailcheck.ing
Accept: application/json
auth_key: string

```

```javascript

const headers = {
  'Accept':'application/json',
  'auth_key':'string'
};

fetch('https://api.mailcheck.ing/v1/verify?email=string',
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

```ruby
require 'rest-client'
require 'json'

headers = {
  'Accept' => 'application/json',
  'auth_key' => 'string'
}

result = RestClient.get 'https://api.mailcheck.ing/v1/verify',
  params: {
  'email' => 'string'
}, headers: headers

p JSON.parse(result)

```

```python
import requests
headers = {
  'Accept': 'application/json',
  'auth_key': 'string'
}

r = requests.get('https://api.mailcheck.ing/v1/verify', params={
  'email': 'string'
}, headers = headers)

print(r.json())

```

```php
<?php

require 'vendor/autoload.php';

$headers = array(
    'Accept' => 'application/json',
    'auth_key' => 'string',
);

$client = new \GuzzleHttp\Client();

// Define array of request body.
$request_body = array();

try {
    $response = $client->request('GET','https://api.mailcheck.ing/v1/verify', array(
        'headers' => $headers,
        'json' => $request_body,
       )
    );
    print_r($response->getBody()->getContents());
 }
 catch (\GuzzleHttp\Exception\BadResponseException $e) {
    // handle exception or api errors.
    print_r($e->getMessage());
 }

 // ...

```

```java
URL obj = new URL("https://api.mailcheck.ing/v1/verify?email=string");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());

```

```go
package main

import (
       "bytes"
       "net/http"
)

func main() {

    headers := map[string][]string{
        "Accept": []string{"application/json"},
        "auth_key": []string{"string"},
    }

    data := bytes.NewBuffer([]byte{jsonReq})
    req, err := http.NewRequest("GET", "https://api.mailcheck.ing/v1/verify", data)
    req.Header = headers

    client := &http.Client{}
    resp, err := client.Do(req)
    // ...
}

```

`GET /verify`

*verify*

Perform a full verification of an email address.

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|email|query|string|true|Input email to check|
|auth_key|header|string|true|Secret key to authenticate mailchecking apis|

<aside class="notice">
This operation does require authentication
</aside>
> Example responses

> 200 Response

```json
{
  "address": [
    "",
    "test@gmail.com",
    "test"
  ],
  "valid_format": true,
  "smtp": {
    "deliverable": false,
    "full_inbox": false,
    "catch_all": false
  },
  "host": {
    "host_exists": true,
    "mx_records": [
      "alt2.gmail-smtp-in.l.google.com.",
      "alt3.gmail-smtp-in.l.google.com.",
      "alt1.gmail-smtp-in.l.google.com."
    ]
  },
  "is_disposable": false,
  "suggested_emails": [
    "test@gmail.com",
    "test@ymail.com",
    "test@mail.com"
  ],
  "final_status": "not deliverable"
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|OK|none|Inline|
|4XX|NOT OK|none|Inline|

### Response Schema

Status Code **200**

*Response Object*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» address|[string]|true|none|['name','email','username'] array for eg if input is 'Tom Gold <tom.g@gmail.com>' the address would be ['Tom Gold','tom.g@gmail.com','tom.g']|
|» valid_format|boolean|true|none|Is email in valid format?|
|» smtp|object|true|none|Verifications performed by connecting to the mail server via SMTP.|
|»» deliverable|boolean|true|none|Is the mail SMTP deliverable?|
|»» full_inbox|boolean|true|none|Does email has full inbox?|
|»» catch_all|boolean|true|none|Is the email catch all?|
|» host|object|true|none|All the destination host details|
|»» host_exists|boolean|true|none|Does the host exist?|
|»» mx_records|[string]|true|none|If exists what are the mx records? Return empty list if there are no host records|
|» is_disposable|boolean|true|none|Is the email from disposable domains?|
|» suggested_emails|[string]|false|none|If the email is not delivarable then this part return few suggested emails considering there is a typo mistake with domain.|
|» final_status|string|true|none|An enum to describe how confident we are that the recipient address is real.|

#### Enumerated Values

|Property|Value|
|---|---|
|final_status|deliverable|
|final_status|not deliverable|
|final_status|deliverable all|
|final_status|unkown|

Status Code **4XX**

*Response Object*

CODE : Reason for a not sucessful request

> Check for error message in code string.

> 4XX Response

```json
{
  "code": "string"
}
```