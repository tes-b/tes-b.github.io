---
published: True
title:  "mysqlclient 설치 오류 error: subprocess-exited-with-error"
categories: Etc Database
tag: [MySQL, ubuntu]
---

mysqlclient 를 설치하려고 하니 다음과 같은 에러로 설치가 안됐다.  

```
(venv) ➜  Resume git:(main) ✗ pip install mysqlclient
Collecting mysqlclient
  Using cached mysqlclient-2.1.1.tar.gz (88 kB)
  Preparing metadata (setup.py) ... error
  error: subprocess-exited-with-error
  
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [16 lines of output]
      /bin/sh: 1: mysql_config: not found
      /bin/sh: 1: mariadb_config: not found
      /bin/sh: 1: mysql_config: not found
      Traceback (most recent call last):
        File "<string>", line 2, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "/tmp/pip-install-bm80vrdb/mysqlclient_253fb51a77ad4450a73c1bca3a2e7ec5/setup.py", line 15, in <module>
          metadata, options = get_config()
        File "/tmp/pip-install-bm80vrdb/mysqlclient_253fb51a77ad4450a73c1bca3a2e7ec5/setup_posix.py", line 70, in get_config
          libs = mysql_config("libs")
        File "/tmp/pip-install-bm80vrdb/mysqlclient_253fb51a77ad4450a73c1bca3a2e7ec5/setup_posix.py", line 31, in mysql_config
          raise OSError("{} not found".format(_mysql_config_path))
      OSError: mysql_config not found
      mysql_config --version
      mariadb_config --version
      mysql_config --libs
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.

```

pip와 setuptools 을 업그레이드 해봤지만 소용이 없었고  
아래 명령어로 libmysqlclient-dev 를 설치하니 mysqlclient도 정상적으로 설치가 되었다.

```
sudo apt-get install libmysqlclient-dev -y
```

## 참고
<https://codethief.io/ko/pip-install-mysqlclient-%EC%97%90%EB%9F%AC-error/>

OS : ubuntu 20.04

