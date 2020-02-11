# .gitignore 설정

Spring의 application.properties, Jenkinsfile... 등 과 같은 설정파일을 .gitigore하고 싶을 때가 있다. 하지만 잘 안된다.

1. 이미 commit을 했는데, .gitignore을 등록한 경우

   ```shell
   $ git rm -r --cached
   ```

   명령어를 실행하면, github에 올라갈때, .gitignore에 등록된 파일들이 github에서 삭제된다. 그렇게 되면 github UI에서 한땀한땀 올려준다.

   근데, pull을 받으려고 하면 다음과 같은 오류가 발생한다.

   ```shell
   error: The following untracked working tree files would be overwritten by merge:
           Dockerfile
           Jenkinsfile
           src/main/resources/application.properties
   Please move or remove them before you merge.
   ```

   github에는 있는 파일이 local에는 .gitignore에 등록되어있어서 생기는 오류다.

   또 해결하기 위해, .gitignore에 등록되지않은 디렉토리만 pull 받아준다.

   ```shell
   $ git config core.sparseCheckout true
   $ echo "src/main/java" >> .git/info/sparse-checkout
   $ e2e-api git:(master) git pull sang master 
   ```

   