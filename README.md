# Misskey v11 Front
See
https://github.com/RinShibuya/misskey-v11-front
https://github.com/MisskeyIO/misskey/tree/v11-front

## Configuring instances
Open the `src/client/app/config.ts` and customize the following line to the hostname you want to connect to.
```
export const instanceHost = "example.com"
```

## Build
```
yarn
npm run build
```

## Deploy
Copy the contents of `built/client` to the root of your web server. 

PHPは別にいらない。PHPは開発サーバーを動かすためだけにある。`index.php`はDeploy時に消しちゃってOK。  
存在しないファイルにアクセスされたら`/app.html`を返す必要があるので、Rewriteなりでどうにかする。

nginx
```
location / {
	root /home/user/v11-front/built/client;
	try_files $uri /app.html;
	add_header Cache-Control "public, max-age=300";
}
```
