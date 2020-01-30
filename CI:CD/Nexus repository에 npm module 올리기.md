# Nexus repository에 npm module 올리기

1. Nexus create

2. Root에 **.npmrc** 파일을 만든다.

   ```
   # last modified: 01 Jan 2016
   ; Set a new registry for a scoped package
   @myscope:registry=http://host:port/repository/npm-group/
   registry=http://host:port/repository/npm-group/
   _auth=dijfiejfi#IJ#9292
   init.author.name = Jong hoon Park
   init.author.email = wecube.1@gmail.com
   email=wecube.1@gmail.com
   always-auth=true
   ```

   - _auth : echo -n 'userId:password' | openssl base64

3. package.json에 "publishConfig"를 추가해준다.

   ```json
   {
     ...
     },
     "dependencies": {
      ...
     },
     "devDependencies": {
      ...
     },
     "eslintConfig": {
       "root": true,
       "env": {
   		  ...
       },
       "extends": [
         ...
       ],
       "rules": {
         ...
       },
       "parserOptions": {
         ...
       }
     },
     "postcss": {
       ...
     },
     "browserslist": [
       ...
     ],
     "gcm_sender_id": "103953800507",
   
     "publishConfig": {
       "registry": "http://host:port/repository/npm-private/"
     }
   }
   ```

4. ```npm publish``` 

   - npm-private에는 .tar파일이 업로드 된다.
   - npm-regitstry에는 npm module이 업로드 된다.

5. Create repsitory -> 

   1. npm(proxy) 
      - Remote storage: https://registry.npmjs.org
   2. npm(group), 
   3. npm(hosted) -> 



Reference

https://help.sonatype.com/repomanager3/formats/npm-registry

https://blog.kingbbode.com/posts/nexus-3xx-maven-npm