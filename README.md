# t3-docker-bug

After project initialization, t3 works fine:

```
conja@conrad-desktop MINGW64 ~/t3-docker-bug (main)
$ cd t3-docker-bug/

conja@conrad-desktop MINGW64 ~/t3-docker-bug/t3-docker-bug (main)
$ npm run dev

> t3-docker-bug@0.1.0 dev
> next dev

   ▲ Next.js 14.1.0
   - Local:        http://localhost:3000
   - Environments: .env

 ✓ Ready in 2.3s
```

After following the [Docker documentation](https://create.t3.gg/en/deployment/docker#overview) and removing the `Dockerfile` prisma code (because I did not initialize this project with prisma)

I tried building:

```
conja@conrad-desktop MINGW64 ~/t3-docker-bug/t3-docker-bug (main)
$ docker build -t ct3a-docker --build-arg NEXT_PUBLIC_CLIENTVAR=clientvar .
[+] Building 1.1s (8/21)                                                                                                                                     docker:default
 => [internal] load build definition from Dockerfile                                                                                                                   0.0s
 => => transferring dockerfile: 1.57kB                                                                                                                                 0.0s 
 => [internal] load .dockerignore                                                                                                                                      0.0s 
 => => transferring context: 124B                                                                                                                                      0.0s 
 => [internal] load metadata for gcr.io/distroless/nodejs20-debian12:latest                                                                                            0.2s 
 => [internal] load metadata for docker.io/library/node:20-alpine                                                                                                      0.2s 
 => [internal] load build context                                                                                                                                      0.0s
 => => transferring context: 1.31kB                                                                                                                                    0.0s 
 => CACHED [deps 1/5] FROM docker.io/library/node:20-alpine@sha256:2f46fd49c767554c089a5eb219115313b72748d8f62f5eccb58ef52bc36db4ad                                    0.0s 
 => [runner 1/7] FROM gcr.io/distroless/nodejs20-debian12@sha256:5fdc9b132dfddb4c237305d4ba7272047ff349156a9cf0b559bc3c34b7ff5075                                      0.0s 
 => ERROR [deps 2/5] RUN apk add --no-cache libc6-compat openssl1.1-compat                                                                                             0.8s 
------
 > [deps 2/5] RUN apk add --no-cache libc6-compat openssl1.1-compat:
0.219 fetch https://dl-cdn.alpinelinux.org/alpine/v3.19/main/x86_64/APKINDEX.tar.gz
0.393 fetch https://dl-cdn.alpinelinux.org/alpine/v3.19/community/x86_64/APKINDEX.tar.gz
0.727 ERROR: unable to select packages:
0.727   openssl1.1-compat (no such package):
0.727     required by: world[openssl1.1-compat]
------
Dockerfile:4
--------------------
   2 |
   3 |     FROM --platform=linux/amd64 node:20-alpine AS deps
   4 | >>> RUN apk add --no-cache libc6-compat openssl1.1-compat
   5 |     WORKDIR /app
   6 |
--------------------
ERROR: failed to solve: process "/bin/sh -c apk add --no-cache libc6-compat openssl1.1-compat" did not complete successfully: exit code: 1
```

The error is caused by the `openssl1.1-compat` package. The `openssl` package should be used instead.