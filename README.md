# CloudFlare Workers

## 설치

기본적으로 wrangler을 전역 설치해야한다.

```bash
npm install -g wrangler
```

그리고 인증한적이 없다면 cloudflare 계정으로 로그인해서 인증해야한다.

```bash
wrangler login
```

실행할 위치에 가서 아래 명령어를 입력하고 여러 설정들을 해주면 된다(타입스크립트, fetch 등 유무)

```bash
wrangler init 앱이름
```

## 실행

```json
{
  "scripts": {
    "start": "wrangler dev",
    "deploy": "wrangler publish"
  }
}
```

기본적으로 script를 보면 start와 deploy가 있다. start로 해서 개발하면 되고, 배포할 때는 deploy를 하면 배포된다. 워커를 만들 때 사용한 도메인으로 배포가 된다.

## routing 하는 법

라우팅 하는 방법은 url을 보고 분기처리해주는 것이다.

```js
const url = new URL(request.url);
if (url.pathname === "/") {
  return new Response(home, {
    headers: {
      "Content-Type": "text/html;charset=utf-8",
    },
  });
}
return new Response(null, {
  status: 404,
});
```

request.url을 보고 pathname을 통해서 라우팅했다.

## kv

key - value 줄여서 kv  
이름엔 -(하이픈) 은 안되고 \_(언더스코어)는 된다.

```bash
wrangler kv:namespace create "view_counter"
wrangler kv:namespace create --preview "view_counter"
```

preview를 붙여주면 dev용 db를 만들어준다.

```
kv_namespaces = [
    { binding = "DB", id = "xxx", preview_id = "xxx" }
]
```

그리고 index.ts에서 Env interface가 있어서 해당 부분을 수정하면 타입스크립트 경고가 뜨지 않는다.

```ts
export interface Env {
  DB: KVNamespace;
}
```

배열에서 binding한 값과 똑같이 넣어주면 된다. 그러면 코드 상에서 env.DB를 통해서 명령어 실행할 수 있음
