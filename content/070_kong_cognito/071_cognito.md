---
title: "AWS Cognito"
chapter: true
draft: false
weight: 1
---



# Creating AWS Cognito
First of all, let's create a Cognito instance using the AWS Console<p>

* Go to Cognito console and click on "Managed User Pools" and on "Create a user pool".
* Name your pool as "kongpool" and click on "Step through settings".
* Select “Email address or phone number” and, under that, select “Allow email addresses”.
* Select the following standard attributes as required
  * email
  * family name
  * given name

![cognito1](/images/cognito1.png)


* Click on "Next step".
* For the next pages, "Policies", “MFA and verifications”, “Message customizations” and "Tags", click on "Next step".
* In the page "Devices", select “No” for “Do you want to remember your user’s devices” and click on “Next step”.
* In the pages "App clients" and "Triggers" click on "Next step".
* In the page "Review" click on "Create pool". Take note of the "Pool Id", in our case <b>us-east-1_PNNirRUle</b>.

![cognito2](/images/cognito2.png)




## Application Definition
* Click on "App clients" left menu option.

* Click on "Add an app client" and enter with the following data:
  * App client name: kong-api
  * Refresh token expiration (days): 30
  * Generate client secret: on
  * Enable lambda trigger based custom authentication (ALLOW_CUSTOM_AUTH): off
  * Enable username password based authentication (ALLOW_USER_PASSWORD_AUTH): on
  * Enable SRP (secure remote password) protocol based authentication (ALLOW_USER_SRP_AUTH): off

* Click on "Set attribute read and write permissions"<p>
Uncheck everything except the "email", "family name" and "given name" fields.

![cognito3](/images/cognito3.png)

* Click on "Create app client" and on "Show details"

* Take note of the "App client id". In our case, <b>2pncij68oab2v848bu9682tc9e</b>

* Click on "Details" and take note of the "App client secret". In our case, <b>do88btsf6thn7jg3i1glr43nc7eparuv9p13ap5ecfpujumug77</b>

![cognito4](/images/cognito4.png)






## Register the Ingress endpoint in Cognito
Return to your Cognito User Pool to register the Ingress.

* Click on "App integration" -> "App client settings".
* Click the “Cognito User Pool”

In the "Callback URL(s)" field type insert your URLs like this. Note that AWS Cognito doesn’t support HTTP callback URLs. This field should include the Ingresses that you want to secure using AWS Cognito.
https://a946e3cab079a49a1b6661ab62d5585f-2135097986.us-east-1.elb.amazonaws.com/sampleroute/hello

* Click “Authorization code grant”.
* Click "email", "openid", "aws.cognito.signin.user.admin" and "profile".

![cognito5](/images/cognito5.png)


* Click on “Save changes”.
* Click on "Choose domain name".

In the "Domain prefix" field type "kongidp" and click on "Check availability" to make sure it's available.

![cognito6](/images/cognito6.png)


* Click on "Save changes".




## Test the Ingress using HTTP/S
<pre>
$ http --verify=no https://a0771705c8a114f7da5a179494d2108e-1744117948.us-east-2.elb.amazonaws.com/sampleroute/hello
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 45
Content-Type: text/html; charset=utf-8
Date: Fri, 08 Oct 2021 12:40:56 GMT
Server: Werkzeug/1.0.1 Python/3.7.4
Via: kong/2.5.1.0-enterprise-edition
X-Kong-Proxy-Latency: 1
X-Kong-Upstream-Latency: 1

Hello World, Kong: 2021-10-08 12:40:56.984265
</pre>
