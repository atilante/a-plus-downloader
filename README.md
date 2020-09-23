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

First, activate the Python virtual environment if you have not done that yet.

```
source venv/bin/activate
```

Then configure the A+ downloader. You must set at least the domain of the A+
service and an API access token. The API access token is an unique identifier
which A+ downloader uses to authenticate you when it reads the API. Go to
<https://aplusdomain/accounts/accounts/>. Alternatively, login to A+ with your
web browser, click your name on the top-right corner and select "Profile".
You will see an information page about your A+ account. Copy the text in a
box under the text "API Access Token". Usually this means double-clicking the
text with the mouse and pressing Ctrl+C. Then give the following command:


```
python3 aplus_downloader.py set-domain aplusdomain --api-token xxx
```

where `aplusdomain` is again the domain of the A+ service, e.g.
`plus.cs.aalto.fi`, and `xxx` is the API Access Token, e.g.
`4b8e612ac31483e041501538408d453248343510`. A+ Downloader saves this information
into a new configuration file `_config/config.ini`.

TODO

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
