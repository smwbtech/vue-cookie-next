# vue-cookie-next

A simple Vue.js 3 plugin for handling browser cookies with typescript support

## Installation

### Browser

```html
  <html lang="en">
    <head>
      <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </head>
    <body>
      <div id="cookie-app">
    </body>
    <script type="module">
      import { VueNextCookies } from "../dist/vue-next-cookies.esm.js";
      const CookieTest = {
        mounted() {
          console.log(this.$cookie.getCookie("username"))
        },
      }
      Vue.createApp(CookieTest)
        .use(VueNextCookies)
        .mount("#cookie-app");
  </script>
  </html>
```

### Package Managers

```
\npm install vue-next-cookies --save
```

```ts
import { createApp } from 'vue'
import { VueNextCookies } from 'vue-cookie-next'

import App from 'App.vue'
const app  = createApp(App)
app.use(VueNextCookies)
app.mount('#app)

// set default config
VueCookieNext.config({ expire:'7d'})

// set global cookie
VueCookieNext.setCookie('theme','default');
VueCookieNext.setCookie('hover-time',{ expire : '1s'});
```

## API Options

syntax format: **[this].\$cookie.[method]** | **[VueCookieNext].[method]**

- Set global config

`````ts
VueCookieNext.config({
  expire: "1d",
  path: "/",
  domain: "",
  secure: "",
  sameSite: "",
});
// default: expireTimes = 1d, path = '/', domain = '', secure = '', sameSite = 'Lax'
```

- Set a cookie

````

this.$cookie.setCookie(keyName, value, {
  expire: "1d",
  path: "/",
  domain: "",
  secure: "",
  sameSite: "",
}) //return this

```

- Get a cookie

```

this.$cookie.getCookie(keyName) // return value

```

- Remove a cookie

```

this.$cookie.removeCookie(keyName , {
  path: "/",
  domain: "",
}) // return this | false if key not found

```

- Exist a `cookie name`

```

this.$cookie.IsCookieAvailable(keyName) // return false or true

```

- Get All `cookie name`

```

this.$cookie.keys() // return a array string

`````

## Example Usage

#### set global config

```ts
import { VueNextCookies } from "vue-cookie-next";
// 30 day after, expire
VueNextCookies.config({ expire: "30d" });

// set secure, only https works
VueNextCookies.config({ expire: "7d", secure: true });

// 2019-03-13 expire
VueNextCookies.config({ expire: new Date(2019, 03, 13).toUTCString() });

// 30 day after, expire, '' current path , browser default
VueNextCookies.config({ expire: 60 * 60 * 24 * 30 });
```

#### support json object

```ts
var user = {
  user_id: 1,
  name: "Ben",
  session: "75442486-0878-440c-9db1-a7006c25a39f",
  session_start_time: new Date(),
};

this.$cookie.setCookieCookie("user", user);
// print user name
console.log(this.$cookie.getCookieCookie("user").name);
```

#### set expire times

**Suppose the current time is : Sat, 11 Mar 2017 12:25:57 GMT**

**Following equivalence: 1 day after, expire**

**Support chaining sets together**

```ts
// default expire time: 1 day
this.$cookie
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f")
  // number + d , ignore case
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f", {
    expire: "1d",
  })
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f", {
    expire: "1D",
  })
  // Base of second
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f", {
    expire: 60 * 60 * 24,
  })
  // input a Date, + 1day
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f", {
    expire: new Date(2017, 03, 12),
  })
  // input a date string, + 1day
  .setCookie("user_session", "75442486-0878-440c-9db1-a7006c25a39f", {
    expire: "Sat, 13 Mar 2017 12:25:57 GMT",
  });
```

#### set expire times, input number type

```ts
this.$cookie.setCookie("default_unit_second", "input_value", { expire: 1 }); // 1 second after, expire
this.$cookie.setCookie("default_unit_second", "input_value", {
  expire: 60 + 30,
}); // 1 minute 30 second after, expire
this.$cookie.setCookie("default_unit_second", "input_value", {
  expire: 60 * 60 * 12,
}); // 12 hour after, expire
this.$cookie.setCookie("default_unit_second", "input_value", {
  expire: 60 * 60 * 24 * 30,
}); // 1 month after, expire
```

#### set expire times - end of browser session

```ts
this.$cookie.setCookie("default_unit_second", "input_value", { expire: 0 }); // end of session - use 0 or "0"!
```

#### set expire times , input string type

| Unit | full name |
| ---- | --------- |
| y    | year      |
| m    | month     |
| d    | day       |
| h    | hour      |
| min  | minute    |
| s    | second    |

**Unit Names Ignore Case**

**not support the combination**

**not support the double value**

```javascript
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "60s",
}); // 60 second after, expire
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "30MIN",
}); // 30 minute after, expire, ignore case
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "24d",
}); // 24 day after, expire
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "4m",
}); // 4 month after, expire
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "16h",
}); // 16 hour after, expire
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "3y",
}); // 3 year after, expire

// input date string
this.$cookie.setCookie(
  "token",
  "GH1.1.1689020474.1484362313",
  new Date(2017, 3, 13).toUTCString()
);
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: "Sat, 13 Mar 2017 12:25:57 GMT ",
});
```

#### set expire support date

```ts
var date = new Date();
date.setDate(date.getDate() + 1);
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", date);
```

#### set never expire

```ts
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", {
  expire: Infinity,
}); // never expire
// never expire , only -1,Other negative Numbers are invalid
this.$cookie.setCookie("token", "GH1.1.1689020474.1484362313", { expire: -1 });
```

#### remove cookie

```ts
this.$cookie.setCookie("token", value); // domain.com and *.doamin.com are readable
this.$cookie.removeCookie("token"); // remove token of domain.com and *.doamin.com

this.$cookie.setCookie("token", value, { domain: "domain.com" }); // only domain.com are readable
this.$cookie.removeCookie("token", { domain: "domain.com" }); // remove token of domain.com
```

#### set other arguments

```ts
// set path
this.$cookie.setCookie("use_path_argument", "value", {
  expire: "1d",
  path: "/app",
});

// set domain
this.$cookie.setCookie("use_path_argument", "value", { domain: "domain.com" }); // default 1 day after,expire

// set secure
this.$cookie.setCookie("use_path_argument", "value", {
  secure: true,
});

// set sameSite - should be one of `None`, `Strict` or `Lax`. Read more https://web.dev/samesite-cookies-explained/
this.$cookie.setCookie("use_path_argument", "value", { sameSite: "Lax" });
```

#### other operation

```ts
// check a cookie exist
this.$cookie.IsCookieAvailable("user_session")

// get a cookie
this.$cookie.getCookie("user_session");

// remove a cookie
this.$cookie.removeCookie("user_session");

// get all cookie key names, line shows
this.$cookie.keys().join("\n");

// remove all cookie
this.$cookie.keys().forEach(cookie => this.$cookie.removeCookie(cookie))

// vue-cookie-next global
[this | VueNextCookies].$cookie.[method]

```

## ⚠️ Warning

**\$cookie key names Cannot be set to ['expires','max-age','path','domain','secure','SameSite']**

## 🌸 Thanks

This project is heavily inspired by the following awesome projects.

- [cmp-cc/vue-cookies ](https://github.com/cmp-cc/vue-cookies)

Thanks!