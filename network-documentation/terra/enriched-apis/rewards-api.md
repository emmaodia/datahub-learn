---
description: Learn how to use the Rewards API on Terra
---

# Rewards API

Test out our Rewards API today with [**DataHub**](https://datahub.figment.io/sign_up?service=terra)!

{% api-method method="get" host="https://terra--search.datahub.figment.io/rewards" path=" " %}
{% api-method-summary %}
rewards
{% endapi-method-summary %}

{% api-method-description %}
Rewards enpoint allows users to query daily reward summaries for an account.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Network identifier \(eg. `terra`\)
{% endapi-method-parameter %}

{% api-method-parameter name="chain\_id" type="string" required=true %}
The chain id \(eg. `columbus-4`\)
{% endapi-method-parameter %}

{% api-method-parameter name="account" type="string" required=true %}
The account identifier
{% endapi-method-parameter %}

{% api-method-parameter name="start\_time" type="string" required=true %}
The start time in UTC. Daily reward summaries will be calculated in 24 hour periods from the start time.
{% endapi-method-parameter %}

{% api-method-parameter name="end\_time" type="string" required=true %}
The end time in UTC.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-parameter name="validators" type="string" required=false %}
A comma separated list of validator account ids
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Success response
{% endapi-method-response-example-description %}

```javascript
[
    {
    "start": 2403477,
    "end": 2416532,
    "time": "2021-04-01T02:00:00Z",
    "validator": "terravaloper1p54hc4yy2ajg67j645dn73w3378j6k05vmx9r9",
    "rewards": [{
        "text": "0.000000239989950658ueur",
        "currency": "ueur",
        "numeric": 239989950658,
        "exp": 18
    }, {
        "text": "781.3557899746317378ukrw",
        "currency": "ukrw",
        "numeric": 781355789974631737802,
        "exp": 18
    }, {
        "text": "5.811522123339340173umnt",
        "currency": "umnt",
        "numeric": 5811522123339340173,
        "exp": 18
    }, {
        "text": "0.000000001332924447usgd",
        "currency": "usgd",
        "numeric": 1332924447,
        "exp": 18
    }, {
        "text": "0.000054802954554323uthb",
        "currency": "uthb",
        "numeric": 54802954554323,
        "exp": 18
    }, {
        "text": "0.000001276891069981ucad",
        "currency": "ucad",
        "numeric": 1276891069981,
        "exp": 18
    }, {
        "text": "0.000005701400811087uchf",
        "currency": "uchf",
        "numeric": 5701400811087,
        "exp": 18
    }, {
        "text": "0.000000025727905834ucny",
        "currency": "ucny",
        "numeric": 25727905834,
        "exp": 18
    }, {
        "text": "0.000001651900681092uaud",
        "currency": "uaud",
        "numeric": 1651900681092,
        "exp": 18
    }, {
        "text": "0.000000009055880212uhkd",
        "currency": "uhkd",
        "numeric": 9055880212,
        "exp": 18
    }, {
        "text": "1.134230855593299543uluna",
        "currency": "uluna",
        "numeric": 1134230855593299543,
        "exp": 18
    }, {
        "text": "0.000000350438429137uinr",
        "currency": "uinr",
        "numeric": 350438429137,
        "exp": 18
    }, {
        "text": "0.000550493820764571usdr",
        "currency": "usdr",
        "numeric": 550493820764571,
        "exp": 18
    }, {
        "text": "0.689624729065837528uusd",
        "currency": "uusd",
        "numeric": 689624729065837528,
        "exp": 18
    }, {
        "text": "0.000000003079230273ugbp",
        "currency": "ugbp",
        "numeric": 3079230273,
        "exp": 18
    }, {
        "text": "0.000000317791080219ujpy",
        "currency": "ujpy",
        "numeric": 317791080219,
        "exp": 18
    }]
    }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad parameters sent
{% endapi-method-response-example-description %}

```javascript
{
  "error": "Bad parameters sent"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Internal/Other server error while processing request
{% endapi-method-response-example-description %}

```javascript
{
  "error": "Something bad happened" 
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Example Request

```http
/rewards?network=terra&chain_id=columbus-4&account=terra1h77gzryshx02v3vlmky32ug0ewx4m5jsdrly69&start_time=2021-04-01T02:00:00.00Z&end_time=2021-04-03T02:00:00.00Z&validators=terravaloper1p54hc4yy2ajg67j645dn73w3378j6k05vmx9r9
```

## Example Response

```javascript
[{
  "start": 2403477,
  "end": 2416532,
  "time": "2021-04-01T02:00:00Z",
  "validator": "terravaloper1p54hc4yy2ajg67j645dn73w3378j6k05vmx9r9",
  "rewards": [{
    "text": "0.000000239989950658ueur",
    "currency": "ueur",
    "numeric": 239989950658,
    "exp": 18
  }, {
    "text": "781.3557899746317378ukrw",
    "currency": "ukrw",
    "numeric": 781355789974631737802,
    "exp": 18
  }, {
    "text": "5.811522123339340173umnt",
    "currency": "umnt",
    "numeric": 5811522123339340173,
    "exp": 18
  }, {
    "text": "0.000000001332924447usgd",
    "currency": "usgd",
    "numeric": 1332924447,
    "exp": 18
  }, {
    "text": "0.000054802954554323uthb",
    "currency": "uthb",
    "numeric": 54802954554323,
    "exp": 18
  }, {
    "text": "0.000001276891069981ucad",
    "currency": "ucad",
    "numeric": 1276891069981,
    "exp": 18
  }, {
    "text": "0.000005701400811087uchf",
    "currency": "uchf",
    "numeric": 5701400811087,
    "exp": 18
  }, {
    "text": "0.000000025727905834ucny",
    "currency": "ucny",
    "numeric": 25727905834,
    "exp": 18
  }, {
    "text": "0.000001651900681092uaud",
    "currency": "uaud",
    "numeric": 1651900681092,
    "exp": 18
  }, {
    "text": "0.000000009055880212uhkd",
    "currency": "uhkd",
    "numeric": 9055880212,
    "exp": 18
  }, {
    "text": "1.134230855593299543uluna",
    "currency": "uluna",
    "numeric": 1134230855593299543,
    "exp": 18
  }, {
    "text": "0.000000350438429137uinr",
    "currency": "uinr",
    "numeric": 350438429137,
    "exp": 18
  }, {
    "text": "0.000550493820764571usdr",
    "currency": "usdr",
    "numeric": 550493820764571,
    "exp": 18
  }, {
    "text": "0.689624729065837528uusd",
    "currency": "uusd",
    "numeric": 689624729065837528,
    "exp": 18
  }, {
    "text": "0.000000003079230273ugbp",
    "currency": "ugbp",
    "numeric": 3079230273,
    "exp": 18
  }, {
    "text": "0.000000317791080219ujpy",
    "currency": "ujpy",
    "numeric": 317791080219,
    "exp": 18
  }]
}, {
  "start": 2416532,
  "end": 2429529,
  "time": "2021-04-02T02:00:00Z",
  "validator": "terravaloper1p54hc4yy2ajg67j645dn73w3378j6k05vmx9r9",
  "rewards": [{
    "text": "0.000000000260020867ucad",
    "currency": "ucad",
    "numeric": 260020867,
    "exp": 18
  }, {
    "text": "0.000384967519933595usdr",
    "currency": "usdr",
    "numeric": 384967519933595,
    "exp": 18
  }, {
    "text": "0.000000000073010243usgd",
    "currency": "usgd",
    "numeric": 73010243,
    "exp": 18
  }, {
    "text": "0.000012877428241842uthb",
    "currency": "uthb",
    "numeric": 12877428241842,
    "exp": 18
  }, {
    "text": "0.000000019585835343uaud",
    "currency": "uaud",
    "numeric": 19585835343,
    "exp": 18
  }, {
    "text": "0.000000000438101461ucny",
    "currency": "ucny",
    "numeric": 438101461,
    "exp": 18
  }, {
    "text": "0.000000000174520582ugbp",
    "currency": "ugbp",
    "numeric": 174520582,
    "exp": 18
  }, {
    "text": "0.000000004737425805uinr",
    "currency": "uinr",
    "numeric": 4737425805,
    "exp": 18
  }, {
    "text": "0.000000004943816493ujpy",
    "currency": "ujpy",
    "numeric": 4943816493,
    "exp": 18
  }, {
    "text": "739.40247509717338675ukrw",
    "currency": "ukrw",
    "numeric": 739402475097173386720,
    "exp": 18
  }, {
    "text": "1.129814042990982488uluna",
    "currency": "uluna",
    "numeric": 1129814042990982488,
    "exp": 18
  }, {
    "text": "5.009892271080488988umnt",
    "currency": "umnt",
    "numeric": 5009892271080488988,
    "exp": 18
  }, {
    "text": "0.550808710436799426uusd",
    "currency": "uusd",
    "numeric": 550808710436799426,
    "exp": 18
  }, {
    "text": "0.000000001036483458uchf",
    "currency": "uchf",
    "numeric": 1036483458,
    "exp": 18
  }, {
    "text": "0.000000000028490095uhkd",
    "currency": "uhkd",
    "numeric": 28490095,
    "exp": 18
  }, {
    "text": "0.000000000646422156ueur",
    "currency": "ueur",
    "numeric": 646422156,
    "exp": 18
  }]
}]
```

If you need help with this API or simply want to share with other builders, you can [**join our community today**](https://discord.gg/fszyM7K)!

