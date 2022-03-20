---
layout: post
title:  "nokogiri install error"
date:   2018-03-26 15:23:35 +0900
categories: 
- Linux
tags:
- linux
- openvas
- ruby
- nokogiri
- splunk
classes: wide
published: true
---


[Flame](https://github.com/blazeinfosec/Flame)라는 openvas의 데이터를 Splunk로 보내주는 프로그램을 설치하고 테스트를 하기 위해 설치 중에 아래와 같은 에러가 발생했습니다.

```
root@:/opt/Flame# gem install nokogiri
Fetching: mini_portile2-2.3.0.gem (100%)
Successfully installed mini_portile2-2.3.0
Fetching: nokogiri-1.8.2.gem (100%)
Building native extensions.  This could take a while...
ERROR:  Error installing nokogiri:
        ERROR: Failed to build gem native extension.

    current directory: /var/lib/gems/2.3.0/gems/nokogiri-1.8.2/ext/nokogiri
/usr/bin/ruby2.3 -r ./siteconf20180326-10285-tdekzx.rb extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h

extconf failed, exit code 1

Gem files will remain installed in /var/lib/gems/2.3.0/gems/nokogiri-1.8.2 for inspection.
Results logged to /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.2/gem_make.out
root@:/opt/Flame#
```

처음에는 중간에 발생한 에러만 보고 조치를 하려고 했는데 엉뚱한 답변들이 많이 있었습니다.

```
ERROR: Failed to build gem native extension.
```

그런데 밑에 자세히 보니 헤더가 없다고 나오는 것을 나중에 알았습니다.

```
mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h
```

해결책은 관련 개발 소스를 설치하고 정상으로 되는 걸 확인했습니다.

- 참조 url
	```
	https://stackoverflow.com/questions/20559255/error-while-installing-json-gem-mkmf-rb-cant-find-header-files-for-ruby
	```


```
root@:/opt/Flame# sudo apt-get install ruby`ruby -e 'puts RUBY_VERSION[/\d+\.\d+/]'`-dev
```

```
root@:/opt/Flame# sudo apt-get install ruby`ruby -e 'puts RUBY_VERSION[/\d+\.\d+/]'`-dev
패키지 목록을 읽는 중입니다... 완료
의존성 트리를 만드는 중입니다       
상태 정보를 읽는 중입니다... 완료
The following additional packages will be installed:
  libgmp-dev libgmpxx4ldbl
제안하는 패키지:
  gmp-doc libgmp10-doc libmpfr-dev
다음 새 패키지를 설치할 것입니다:
  libgmp-dev libgmpxx4ldbl ruby2.3-dev
0개 업그레이드, 3개 새로 설치, 0개 제거 및 0개 업그레이드 안 함.
1,356 k바이트 아카이브를 받아야 합니다.
이 작업 후 6,493 k바이트의 디스크 공간을 더 사용하게 됩니다.
계속 하시겠습니까? [Y/n] 
받기:1 http://ftp.daumkakao.com/ubuntu xenial/main amd64 libgmpxx4ldbl amd64 2:6.1.0+dfsg-2 [8,948 B]
받기:2 http://ftp.daumkakao.com/ubuntu xenial/main amd64 libgmp-dev amd64 2:6.1.0+dfsg-2 [314 kB]
받기:3 http://ftp.daumkakao.com/ubuntu xenial-updates/main amd64 ruby2.3-dev amd64 2.3.1-2~16.04.6 [1,033 kB]
내려받기 1,356 k바이트, 소요시간 0초 (4,332 k바이트/초)
Selecting previously unselected package libgmpxx4ldbl:amd64.
(데이터베이스 읽는중 ...현재 232389개의 파일과 디렉터리가 설치되어 있습니다.)
Preparing to unpack .../libgmpxx4ldbl_2%3a6.1.0+dfsg-2_amd64.deb ...
Unpacking libgmpxx4ldbl:amd64 (2:6.1.0+dfsg-2) ...
Selecting previously unselected package libgmp-dev:amd64.
Preparing to unpack .../libgmp-dev_2%3a6.1.0+dfsg-2_amd64.deb ...
Unpacking libgmp-dev:amd64 (2:6.1.0+dfsg-2) ...
Selecting previously unselected package ruby2.3-dev:amd64.
Preparing to unpack .../ruby2.3-dev_2.3.1-2~16.04.6_amd64.deb ...
Unpacking ruby2.3-dev:amd64 (2.3.1-2~16.04.6) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
libgmpxx4ldbl:amd64 (2:6.1.0+dfsg-2) 설정하는 중입니다 ...
libgmp-dev:amd64 (2:6.1.0+dfsg-2) 설정하는 중입니다 ...
ruby2.3-dev:amd64 (2.3.1-2~16.04.6) 설정하는 중입니다 ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
root@:/opt/Flame# 
```

### 정상 설치
```
root@:/opt/Flame# gem install nokogiri 
Building native extensions.  This could take a while...
Successfully installed nokogiri-1.8.2
Parsing documentation for nokogiri-1.8.2
Installing ri documentation for nokogiri-1.8.2
Done installing documentation for nokogiri after 10 seconds
1 gem installed
root@:/opt/Flame# 
```
