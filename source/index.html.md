# Mailcheck.ing API Documentation

Welcome to the Mailcheck.ing API. This API allows you to verify email addresses to check their validity, domain details, SMTP server status, check data breaches, and more.

## API Information

- **API Version**: 1.0
- **Base URL**: `https://api.mailcheck.ing`

## Authentication

> For Authentication, use Authorization header

```shell
curl "URL" \
  -H "Authorization: <API-KEY>"
```

To access the Mailcheck.ing API, you must provide your authentication credentials in the request header.

- **Header Name**: `Authorization`
- **Type**: `string`
- **Required**: Yes

## Endpoints

> Sample request: POST https://api.mailcheck.ing/v1/verify

```shell
curl -X 'POST' \
  'https://api.mailcheck.ing/v1/verify' \
  -H 'accept: application/json' \
  -H 'Authorization: <API_KEY_HERE>' \
  -H 'Content-Type: application/json' \
  -d '{
  "check_breach": true,
  "email": "string",
  "verify": true
}'
```

### Verify Email Address

Verifies the provided email address with optional breach and verification checks.

- **URL**: `/v1/verify`
- **Method**: `POST`
- **Content-Type**: `application/json`
- **Authorization**: Required

#### Request Parameters

- **check_breach**: Flag to enable breach check. If true, then a breach check will be performed.
- **email**: The email address to verify.
- **verify**: Flag to enable verification check. If true, then Syntax, MX, SMTP, CatchAll checks will be performed.

#### Request Example


`
{
"check_breach": true,
"email": "example@example.com",
"verify": true
}
`
<aside class="notice">
API usage will be calculated based on if the verify and check_breach parameters are set to true. If both are set to true, then 2 API calls will be consumed. check_breach is under active development and will be available soon.
</aside>


#### Responses

> Sample 200 Response

```json
{
  "email": "test@gmail.com",
  "reachable": "no",
  "syntax": {
    "username": "test",
    "domain": "gmail.com",
    "valid": true
  },
  "smtp": {
    "host_exists": true,
    "full_inbox": false,
    "catch_all": false,
    "deliverable": false,
    "disabled": false
  },
  "gravatar": {
    "gravatarUrl": "https://gravatar.com/example",
    "hasGravatar": true
    },
  "suggestion": "",
  "disposable": false,
  "role_account": true,
  "free": true,
  "mx": {
    "HasMXRecord": true,
    "Records": [
      {
        "Host": "gmail-smtp-in.l.google.com.",
        "Pref": 5
      },
      {
        "Host": "alt1.gmail-smtp-in.l.google.com.",
        "Pref": 10
      },
      {
        "Host": "alt2.gmail-smtp-in.l.google.com.",
        "Pref": 20
      },
      {
        "Host": "alt3.gmail-smtp-in.l.google.com.",
        "Pref": 30
      },
      {
        "Host": "alt4.gmail-smtp-in.l.google.com.",
        "Pref": 40
      }
    ]
  },
  "breach": {
    "found": false,
    "details": []
  }
}
```

##### 200 OK

Successful response with details about the email verification.

###### Response Attributes

- **breach**: Details about the breach, if any.
- **disposable**: Indicates if the email is from a disposable email provider.
- **email**: The email address that was verified.
- **free**: Indicates if the domain is a free email domain.
- **gravatar**: Details about the Gravatar associated with the email.
- **mx**: Details about the MX records of the domain.
- **reachable**: Describes whether the recipient address is real.
- **role_account**: Indicates if the account is role-based.
- **smtp**: Details about the SMTP response of the email.
- **suggestion**: Suggested domain if the original domain is misspelled.
- **syntax**: Details about the email address syntax.

<aside class="notice">
reachable parameter can have following values:
<ul>
<li>"yes" - The recipient address is real and deliverable.</li>
<li>"no" - The recipient address is not deliverable.</li>
<li>"unknown" - The SMTP server does not allow verification(There could be several reasons and accept all is one of them).</li>
</ul>
</aside>

##### 400 Bad Request

The request was invalid or cannot be served.

##### 401 Unauthorized

Authentication credentials were missing or incorrect.

##### 500 Internal Server Error

Something went wrong on the server.

<aside class="notice">
"Accept all" in the context of SMTP (Simple Mail Transfer Protocol) refers to a configuration or behavior of a mail server where the server accepts all incoming email messages regardless of whether the recipient address is valid or exists on the server. This behavior can be seen in certain types of mail servers, especially in corporate environments or specialized SMTP services.
</aside>

### Result
| Field        | Type    | Description                                                |
|--------------|---------|------------------------------------------------------------|
| breach       | object  | Details about the breach.                                  |
| disposable   | boolean | Whether it is a disposable email address.                  |
| email        | string  | Passed email address.                                      |
| free         | boolean | Whether the domain is a free email domain.                 |
| gravatar     | object  | Gravatar details.                                          |
| mx           | object  | Details about the MX records of the domain.                |
| reachable    | string  | Describes whether the recipient address is real.           |
| role_account | boolean | Whether the account is role-based.                         |
| smtp         | object  | SMTP response details of the email.                        |
| suggestion   | string  | Domain suggestion for misspelled domains.                  |
| syntax       | object  | Details about the email address syntax.                    |


## Definitions

### Breach
| Field   | Type  | Description                              |
|---------|-------|------------------------------------------|
| details | array | Array of `Breach.Details`.               |
| found   | boolean | Indicates if a breach was found.       |

### Breach.Details
| Field       | Type   | Description                        |
|-------------|--------|------------------------------------|
| date        | string | Date of the breach.                |
| description | string | Description of the breach.         |
| name        | string | Name of the breach.                |
| title       | string | Title of the breach.               |

### Gravatar
| Field       | Type    | Description          |
|-------------|---------|----------------------|
| gravatarUrl | string  | Gravatar URL.        |
| hasGravatar | boolean | Whether has gravatar.|

### Mx

| Field         | Description                                         |
|---------------|-----------------------------------------------------|
| hasMXRecord   | Indicates if there is one or more MX records.       |
| records       | Represents DNS MX records. `Mx.Records`             |

### Mx.Records

| Field         | Description                                      |
|---------------|--------------------------------------------------|
| host          | The host of the MX record.                       |
| pref          | The preference number of the MX record.          |

### SMTP

| Field         | Description                                                 |
|---------------|-------------------------------------------------------------|
| catch_all     | Indicates if the domain has a catch-all email address.      |
| deliverable   | Indicates if an email can be sent to the email server.      |
| disabled      | Indicates if the email is blocked or disabled by the provider. |
| full_inbox    | Indicates if the email account's inbox is full.             |
| host_exists   | Indicates if the host exists.                               |

### Syntax

| Field         | Description                                        |
|---------------|----------------------------------------------------|
| domain        | The domain part of the email address.              |
| username      | The username part of the email address.            |
| valid         | Indicates if the email address is in a valid format. |

## Contact

For any additional information or support, please contact the contact@mailcheck.ing
