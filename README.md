# Python3 Flask Rest API with Celery example

[![Build Status](https://travis-ci.org/matthieugouel/python-flask-celery-example.svg?branch=master)](https://travis-ci.org/matthieugouel/python-flask-celery-example)
[![Coverage Status](https://img.shields.io/coveralls/github/matthieugouel/python-flask-celery-example.svg)](https://coveralls.io/github/matthieugouel/python-flask-celery-example?branch=master)
[![license](https://img.shields.io/github/license/matthieugouel/python-flask-celery-example.svg)](https://github.com/matthieugouel/python-flask-celery-example/blob/master/LICENSE)

This project is an example of using Flask-restful and celery to perform asynchronous tasks.

## Installation

Note : The installation into a virtualenv is heavily recommended.

If you want to install the package :

```
pip install .
```

For development purposes, you can install the package in editable mode with the dev requirements.

```
pip install -e . -r requirements-dev.txt
```

## Usage

To start the application, you can use the file run.py :

```
python run.py
```

Moreover, to be able to play with celery, you have to first start Redis, then start a celery worker like this :

```
celery -A run.celery worker --loglevel=info
```
or
```
celery -A run.celery worker -l INFO
```

Note : It's cleaner to use docker-compose to start the whole application (see the section below).

## Usage with Docker Compose

In order to start the whole system easily, we can use docker-compose :

```
docker-compose up
```

It will start three docker containers :
- Redis
- Flask API
- Celery Worker

Then, you can access to the API in localhost :

```
curl -X GET -H "Content-Type: application/json" localhost:5000/api/bye/test
```
## 注意，在Windows平台下运行，celery终端会提示“Celery ValueError: not enough values to unpack (expected 3, got 0)”
解决办法：

原网页:![Unable to run tasks under Windows](https://github.com/celery/celery/issues/4081)

看别人描述大概就是说win10上运行celery4.x / celery5.x 就会出现这个问题，解决办法如下,原理未知：

先安装一个 eventlet
```
pip install eventlet
```
然后启动worker的时候加一个参数，如下：
```
celery -A run.celery worker -l INFO -P eventlet
```
然后就可以正常的被调用了。
发送一个get请示
```
curl -X GET -H "Content-Type: application/json" localhost:5000/api/bye/test
```
在终端就显示
```
[2021-06-21 11:15:24,814: INFO/MainProcess] Task api.resources.main.asynchronous[75b8f49c-331d-4685-9c29-ed6861e000d4] received
[2021-06-21 11:15:29,871: INFO/MainProcess] Task api.resources.main.asynchronous[75b8f49c-331d-4685-9c29-ed6861e000d4] succeeded in 5.062999999965541s: {'async': 'test'}
```
taskid可视化
![image](https://user-images.githubusercontent.com/13137730/122703075-63180780-d283-11eb-9b58-fecf8b08a47d.png)


## Syntax

You can check the syntax using flake8 (you must have flake8 package installed first) :

```
flake8 api
```

You can also use tox (you must have tox package installed first) :

```
tox -e lint
```

## Test coverage

To execute the test coverage, you must install the package with the dev requirements (see installation section).

You can run the coverage with the following command :

```
coverage run --source api -m py.test
```

You can also use tox (you must have tox package installed first) :

```
tox -e test
```
