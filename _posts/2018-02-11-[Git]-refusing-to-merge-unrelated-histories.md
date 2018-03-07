지킬-깃허브 블로그 생성 중 다음과 같은 에러가 발생했다.

```bash
$ git pull origin master
# fatal: refusing to merge unrelated histories
```

해결책은 다음과 같다.

```bash
$ git pull origin master --allow-unrelated-histories
```

출처: http://cpdev.tistory.com/51