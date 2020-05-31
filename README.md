# Misskey v11 Front
v11 web clients running on v12 instances!

Available on Misskey.io
https://v11.misskey.io

## notice
I'm really sorry.

Originally, the above site was able to connect to each instance, but there were multiple bugs and the number of users in other instances was low, so it became a dedicated client for Misskey.io.

You can easily build it by following the steps below, so please run the client on your own as needed.


# How to use
To run this client, you must have your own domain (even on localhost) and a PHP environment.

## Configuring instances
Open the `src/client/app/config.ts` and customize the following line to the hostname you want to connect to.
```
export const instanceHost = "misskey.io"
```

## Build
```
yarn
npm run build
```

**Do not run the `npm run clean`**

## Run
Copy the contents of `built/client` to a PHP-enabled web server.