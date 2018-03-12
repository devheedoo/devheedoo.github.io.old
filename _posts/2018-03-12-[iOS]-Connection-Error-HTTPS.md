https://forums.developer.apple.com/message/298999

There are some posts related to error code -1005. But I couldn't solve my problem with them, so I wrote this.



### Error Explanation

#### on "iOS + HTTPS"

After entering the website, it loads some resources for seconds, then losts connection. It happen on "iOS + HTTPS" in all browsers.

#### Solved After Setting Desktop Mode

Strangely, the error disappears when changing the browser mode to "Desktop Mode". But there's no way to change user's browser mode, so it's not a solution.



### Details

#### Less CSS, Less Error

I tested for a while on test page. If the number of CSS files decreases, error occurs less often. But it doesn't disappear perfectly even if there's no CSS file.

#### HTTP Code 200 ... Then Disappeared

On Tomcat server, log says only 200 HTTP code. There's no 404 nor other codes. And resource loading finishes irregularly.

#### "http load failed (error code -1005)"

I saw the following code repeat at Console on Mac.

```
error 13:09:28.725842 +0900 com.apple.WebKit.Networking TIC Read Status [467:0x15f959500]: 1:57
error 13:09:28.726451 +0900 com.apple.WebKit.Networking Task <201EA34E-AC31-47EE-9AE1-53C53D420BB4>.<223> HTTP load failed (error code: -1005 [1:57])
error 13:09:28.727700 +0900 com.apple.WebKit.Networking Task <201EA34E-AC31-47EE-9AE1-53C53D420BB4>.<223> finished with error - code: -1005
```

#### SSL Chain Uninvolved

Digicert, CA of my website, says the SSL certificate isn't installed normally. Its chain is not registered.

#### Another Website Works Well on Same Server

Strangely again, there is another website which uses the same SSL certificate and Tomcat server. But there is no error on it.



### Development Environment

#### Tomcat, Spring

single Tomcat server without Apache server, and Spring

#### Web Firewall With Basic Mode

A web firewall operates at my clients' side. They said there's no special policies on it.

