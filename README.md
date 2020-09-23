# A+ Downloader

Script for downloading student data from an [A+](https://github.com/apluslms/a-plus)
course using A+ REST API. You can download data for all students or some of
them, or all data for your own user account. The script downloads data to a
directory structure like this: `course/user/module/exercise/submission`.

## Installation

You will need [Python 3](https://www.python.org/). The software is known to
work with at least Python 3.5.

First, create a Python virtual environment which contains correct versions
of required Python libraries. The following commands assume a macOS or Linux
command line. See [Python documentation]
(https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)
for the Windows version.

```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Usage

Activate the Python virtual environment and run A+ Downloader:

```
source venv/bin/activate
python3 aplus_downloader.py
```

After A+ Downloader has done its work, deactivate the virtual environment to
return your default environment:

```
deactivate
```

## Further information: A+ REST API

The A+ Downloader uses A+ REST API to retrieve data. It is useful to know the
some structure of the API, as the A+ Downloader uses similar terms
(course, submission, file, user).

One can read the API by web browser at <https://aplusdomain/api/v2/>, where
`aplusdomain` is the domain name of the A+ service, such as `plus.cs.aalto.fi`.
When the API is accessed by web browser, it presents a dynamic HTML view of the
data. One can browse the data by clicking the links. The `api/v2/` in the URL
is the root directory of the API. The following list shows some subdirectories
related to downloading student-submitted files.

`courses/` : list of courses (first page)

`courses/?page=x` : list of courses, page number x

`courses/id/` : data of a particular course with identifier `id`

`courses/id/exercises/` : list of exercises on a course

`courses/id/exercises/eid/` : data of exercise having identifier `eid` on course
with identifier `id`

`courses/id/exercises/eid/submissions/` : List of submissions for a particular
exercise on a particular course. Each entry refers to `submissions/id/` as
described below.

`courses/id/exercises/eid` : data of exercise having identifier `eid` on course
with identifier `id`

`courses/id/students/` : list of enrolled students on a course

`submissions/id/` : the data of a particular exercise submission `id`

`submissions/id/files/fid/` : one unique file `fid` in a unique submission `id`

`users/` : list of users

`users/?page=x` list of users, page number x

The A+ Downloader uses the
[A+ API client library](https://github.com/Aalto-LeTech/a-plus-client) to
access the API. It requests the data in JSON format. The A+ API itself is
defined in <https://github.com/apluslms/a-plus/blob/master/api/urls_v2.py> .
It is implemented with
[Django REST framework](https://www.django-rest-framework.org/).
