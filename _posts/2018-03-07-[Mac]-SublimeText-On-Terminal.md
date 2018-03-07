요즘 Jekyll 블로그를 사용하느라 터미널에서 이것저것 작업하는 일이 많다. 그러다가 글을 작성할 때마다 폴더를 직접 찾아가서 SublimeText로 파일을 여는 건 불편했다. 터미널에서 실행하고 싶어서 구글링하자마자 바로 찾았다.

먼저 실행파일 위치를 확인한다.

```
# If Sublime Text 2:
open /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl

# If Sublime Text 3:
open /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl
```

실행파일 위치를 확인했으면 링크를 만든다.

```
# If Sublime Text 2:
ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/sublime

# If Sublime Text 3:
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime

```

아주 마음에 쏙 들게 동작한다! vi 명령어를 사용하듯이 사용하면 된다.