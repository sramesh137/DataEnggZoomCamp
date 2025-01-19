
### Question 1. Understanding docker first run

1. Docker Command
2. Image Download
3. Checking `pip` version


```
➜  DataEnggZoomCamp git:(main) docker run -it python:3.12.8 bash
Unable to find image 'python:3.12.8' locally
3.12.8: Pulling from library/python
e474a4a4cbbf: Pull complete
d22b85d68f8a: Pull complete
936252136b92: Pull complete
94c5996c7a64: Pull complete
c980de82d033: Pull complete
e05e1469c731: Pull complete
ded9ddaf4f92: Pull complete
Digest: sha256:5893362478144406ee0771bd9c38081a185077fb317ba71d01b7567678a89708
Status: Downloaded newer image for python:3.12.8
```

```
root@44b59ae3af80:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)
```

### Question :2 Understanding Docker networking and docker-compose
