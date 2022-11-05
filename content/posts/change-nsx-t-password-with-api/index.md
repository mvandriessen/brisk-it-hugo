---
title: Change NSX-T password with API
author: Maarten Van Driessen
type: post
date: 2021-04-20T12:11:30+00:00
url: /change-nsx-t-password-with-api/
categories:
  - NSX-T
  - VMware
tags:
  - api
  - nsx-t
  - VMware

---
Last night, I logged into the NSX-T manager in my lab and was greeted with the following message. 

{{< figure src="./images/passexpiration.png" alt="password expiration">}}

In the past, I would SSH into the edge nodes and change the password like that. But since NSX-T is suggesting to use the API, I figured I would try it. This would be a lot easier to do for the 3 users on both my edge nodes than having to type out all the commands.

## Connecting to the API

Whenever I want to explore an API, I fire up Postman to see what I can learn. To connect to NSX-T, you can just enter the URL in the address bar. In this case, I'm going to explore `https://s-bi-nsxmgr1/api/v1/node/users` . Where s-bi-nsxmgr1 is the hostname of one of my NSX-T managers, and /api/v1/node/users is the API endpoint I'm querying. 

Before we can hit the send button, we need to provide some credentials in the **Authorization** tab. Here in my lab, I'm just using basic auth and the admin user.

{{< figure src="./images/image.png" alt="verify if software is installed">}}

## Getting the users

Once you've specified credentials to authorize onto NSX-T, you can hit send and get a list of all users that exist in your NSX-T environment. Here in my lab, only 5 users exist, 2 of which are not activated. This will probably be the same in many labs, I only have the root, admin, and audit user configured.

{{< figure src="./images/image-1.png" alt="">}}

For each user, you also get some additional attributes like the last time the password was changed and the required password change frequency.

## Changing the password

So let's go ahead and change the password of the audit user. Note down the userid of the audit user from the previous API call. In my case, it's 10002. Now that we've got the userid, we can verify if it's actually the user we want. Type the userid in the URL so it looks like this; `https://s-bi-nsxmgr1/api/v1/node/users/10002` and hit enter again. You'll see all the details of the audit user.

Now to actually change the password, we need to send a body along with our API call. In the body, we need to define 2 attributes; password and old\_password. Where password is the new password and old\_password is, well it's the old password of course! 🙂

In postman, go to the **Body** tab and make sure the radio button is set to **raw** and the type to **JSON**. Also change the call type from **GET** to **PUT**, since we're putting a new password.
{{< figure src="./images/image-2.png" alt="">}}

In the body you can set the following, change the values accordingly;

```
{
    "password": "UltraHighS3cur!ty123!",
    "old_password": "SuperSecurePassword2"
}
```

If all goes well you should get the following output
{{< figure src="./images/image-3.png" alt="">}}

HTTP code 200 indicates a success and you can see the _last\_password\_change_ attribute has been reset to 0, indicating it was just changed.

So you can see, changing the password via the API is not something to be scared of, in fact it's dead simple and can save you a lot of time. Just make sure to clean up the postman tabs you used, so no-one can get to the new password.