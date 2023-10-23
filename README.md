# Simple example using pytest-qt

[![CI](https://github.com/mkinney/pytest-qt-example/actions/workflows/main.yml/badge.svg)](https://github.com/mkinney/pytest-qt-example/actions/workflows/main.yml)
[![codecov](https://codecov.io/gh/mkinney/pytest-qt-example/branch/master/graph/badge.svg?token=FDF9R46W31)](https://codecov.io/gh/mkinney/pytest-qt-example)

# 변경 내역

배포 버전 패키징을 위해 pyinstaller 추가 설치 
배포 버전 패키징시에 pyinstaller 는 시스템에 설치된 PySide6 를 참조하여 라이브러리 포함시키므로 PySide6 를 system-wide 로 설치 (*** 본래의 venv 환경 activation 은 필요 없고, 적용도 안된다)

```
# system-wide 로 PySide6 설치
$ pip install PySide6
# pyinstaller 설치
$ pip install pyinstaller
# 한개 exe 형식으로 빌드
$ pyinstaller --onefile --windowed messageboxex.py
```

Jenkins pipeline 을 위한 Jenkinsfile 추가


# To setup

Run the following commands to setup a python virtual environment, install the requirements int and set the execution flag on the two sample scripts.

```
python3 -m venv venv
venv/bin/activate
pip install -r requirements.txt
chmod +x hello.py
chmod +x messageboxex.py
```

# To run the test apps

```
./hello.py
```

or

```
./messageboxex.py
```

# To test

```
pytest
```

# Example output:

```
% pytest -s -vv
================================================================ test session starts =================================================================
platform darwin -- Python 3.9.9, pytest-6.2.5, py-1.11.0, pluggy-1.0.0 -- /nix/code/pytest-qt-example/venv/bin/python3.9
cachedir: .pytest_cache
PySide6 6.2.3 -- Qt runtime 6.2.3 -- Qt compiled 6.2.3
rootdir: /nix/code/pytest-qt-example
plugins: qt-4.0.2
collected 6 items

test_hello.py::test_hello PASSED
test_messageboxex.py::test_about_clicked PASSED
test_messageboxex.py::test_warning_clicked PASSED
test_messageboxex.py::test_info_clicked PASSED
test_messageboxex.py::test_like_clicked_yes PASSED
test_messageboxex.py::test_like_clicked_no PASSED

================================================================= 6 passed in 0.39s ==================================================================
```

# To run a code coverage report for messageboxex

```
pytest --cov-report html --cov messageboxex
```
