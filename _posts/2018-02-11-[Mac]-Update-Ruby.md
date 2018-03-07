지킬-깃허브 블로그를 만드는 중이다. 루비 버전이 2.1이상이어야 하는데 현재 2.0이다. 터미널을 사용해서 루비 버전을 업데이트했다.

```bash
# Ruby 버전 관리 시스템? RVM 설치
$ curl -L https://get.rvm.io | bash -s stable

# 사용가능한 버전 확인
$ rvm list known

# Ruby 버전 선택해서 설치
$ rvm install ruby-2.4.0

# Ruby 버전 확인
$ ruby --version
# 결과: ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-darwin16]
```

출처: http://codingpad.maryspad.com/2017/04/29/update-mac-os-x-to-the-current-version-of-ruby/