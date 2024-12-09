# fanpage

We are given this website with the objective to access the admin panel. No viewing of the backend this time.

![image](https://github.com/user-attachments/assets/94002c70-1340-4b51-b53e-bc380cb25db8)

Obviously, it's checking something to determine whether we're an admin. Let's check the web request.


As we can see, it has a cookie. A JWT token, to be specific. Putting this through [JWT debugger](https://jwt.io/) gives us 


```
{
  "username": "guest",
  "exp": 1733765697
}
```

The exp parameter just determines the expiry of the cookie. But it becomes fairly obvious we need to change the username parameter to "admin"

In order to do that, we first need to understand the structure of a JWT token.

It has the structure ``` HEADER.PAYLOAD.SIGNATURE ```, With the header and payload being base64encoded, and the signature being encoded in this manner ``` HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)```

Now, if a weak secret is used, it can still be cracked, and that's exactly what we're going to do.

Running this in hashcat 

```hashcat -m 16500 jwt_hashed.txt /usr/share/wordlists/rockyou.txt``

Gives us 'i-love-shark'. This allows us to create a authentic cookie, completing the challenge

![image](https://github.com/user-attachments/assets/705152dc-3673-4ea6-846b-3cb88b7bb94f)

`blahaj{Jwt_BrUt3f0Rc3_9291}`
