- [1. Check unused dependencies in nodejs app](#1-check-unused-dependencies-in-nodejs-app)
- [2. Migrate AWS SDK from V2 to V3](#2-migrate-aws-sdk-from-v2-to-v3)


## 1. Check unused dependencies in nodejs app

```js
sudo npm install depcheck -g
depcheck
```

## 2. Migrate AWS SDK from V2 to V3

```sh
npm i @aws-sdk/client-s3

npx aws-sdk-js-codemod -t v2-to-v3 S3Bucket.js
```