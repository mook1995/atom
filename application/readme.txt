
D:\Python_Exercise>conda create -n board-env python=3.7;

django는 폼이 정해져있기 때문에 처음개발할때는 이곳저곳 쓸것이 많지만
관리 측면에서는 형식이 정해져 있다는 점떄문에 플라스크 보다 더 쉽다



#mysql
create database django_db default character set utf8;

create user 'django'@'localhost' identified by '1234';
create user 'django'@'%' identified by '1234';

grant all privileges on django_db.* to 'django'@'localhost';
grant all privileges on django_db.* to 'django'@'%';

flush privileges;

이후에 유저 프로필을 직접 생성해준다.....




# django_day01.sql에서 이어지는 내용.....
#우리는 파이썬 버전 3.7을 쓰는 가상환경을 만들것입니다. (board-env
파이썬 버전 3.8은 안돼요!)
#ananconda prompt

D:\Python_Exercise> conda create -n board-env python=3.7

conda update -n base conda

conda update --all
(아나콘다 설치시 만들어진 패키지들을 모두 업데이트 해준다.)


python -m pip install --upgrade pip
(pip자체는 업그레이드 되지만 이거는 수동으로 해줘야 한다)


(base) D:\Python_Exercise>conda activate board-env

(board-env) D:\Python_Exercise>conda list
# packages in environment at D:\Anaconda\envs\board-env:
#
# Name                    Version                   Build  Channel
ca-certificates           2021.7.5             haa95532_1
certifi                   2021.5.30        py37haa95532_0
openssl                   1.1.1l               h2bbff1b_0
pip                       21.0.1           py37haa95532_0
python                    3.7.11               h6244533_0
setuptools                52.0.0           py37haa95532_0
sqlite                    3.36.0               h2bbff1b_0
vc                        14.2                 h21ff451_1
vs2015_runtime            14.27.29016          h5e58377_2
wheel                     0.37.0             pyhd3eb1b0_1
wincertstore              0.2                      py37_0

(pip version이 21.2.4가 되도록 python -m pip install --upgrade pip 실행)

conda install django=3 (그냥 django만 쓰면 default 2버전을 받는다...)







#파이참- 프로젝트 생성
PycharmProject\BoardProject\venv

previously configure interpreter를 선택
conda environment 탭 클릭- interpreter에서 아까 만들어준 가상환경 선택
(만약 없으면 user 밑데 anaconda3/ envs/ board-env/python.exe를 직접찾아볼것!)
conda env list 를 입력하면 생성된 env들의 위치를 볼수 있다.




#파이참 터미널 디폴트경로로 board-env설정하기
setting- tools- terminal- Shell path:를 다음과 같이수정
cmd.exe "/K" C:\ProgramData\Anaconda3\Scripts\activate.bat board-env
(만약 해당 위치가 안되면 해당위치에 activate.bat파일이 있나 확인해볼것)
이후에 수정한 부분을 cmd.exe로 다시 바꾸면 완료된다.



#파이참 터미널에서 django프로젝트를 만들것임(이름을 board로 하겠다.)
django-admin startproject board
실행시키면 좌측 디렉토리에서 ../board/board/가 생성된것을 확인할수 있다.
두번째board폴더(아이콘에 점표시)가 django 프로젝트가 실행되는 위치이다. 그러나
여기서 첫번쨰 board는 manage.py가 있고 이게 관리 폴더인데 이게 이름이 같아 불편하니
바꿔줄것이다.(refactor에서체크박스 둘다 해제 한다. 아니면 board라는 이름으로 된 모든 폴더를 수정하게 된다.)



1.cd boardRoot
2.dir
3.python manage.py startapp noticeboard
(애플리케이션 이름을 noticeboard로 생성한다.)
(명령어 실행하면 좌측에 디렉토리가 생성되는 것을 확인 가능하다.)
이상태에서 설정파일을 건드랴줘야 한다.

#파이참에서
1.board/settings.py 파일 열기
28line> 누구한테 접속을 허용하겠냐 하는부분임 ['localhost', '127.0.0.1']
만약 26line에 DEBUG가 true로 되어있으면 저 로컬호스트가 default로 들어가 있게 된다.
만약 우리가 이걸 배포해서 어떤 클라이언트든 접속가능하게 하려면 [*]이렇게 써주면된다.

33line>기본적으로 들어가는 어플리케이션 목록들임 여기에 우리가 좀전에 생성한 noticeboard를
등록해줘야 한다. noticeboard/apps파일에 보면 NoticeboardConfig라는 클래스가 있는데 이 이름을 복사해서
'noticeboard.apps.NoticeboardConfig',이와 같이 추가하면 등록이 완료된것이다.




#
mysql데이터베이스와 django 연동시켜주기
이를 위해 원레는 mysqlclient라는 c++언어로 된 프로그램을 이용했으나 이게 빠르고 좋았긴 하지만
너무 민감한탓에 문제도 여럿있었다. 그래서 이 걸 만든 사람이 PyMySQL이라는 파이썬 언어로 된것을 만들어서
뿌려줬다. 우리는 이것을 이용할것이며 이걸 설치해줘야 한다.

#anaconda prompt
(board-env)conda install -c anaconda pymysql 설치



#board/settings파일에
line77>DATABASES
'ENGINE':'django.db.backends.mysql',
'NAME':'django_db',
'USER':'django',
'PASSWORD':'1234',
'HOST':'127.0.0.1',
'PORT':'3306',
}}

import pymysql
pymysql.version_info=(1,4,2,"final",0)
pymysql.install_as_MySQLdb()




line117>TIME_ZONE ='ASIA/Seoul'

line123>USE_TZ = False (데이터 베이스에 쓸 타임존)



##터미널
>python manage.py migrate



##mySQL
이제 db를 보면 많은것들이 생성되있는게 보인다.


##터미널
python manage.py runserver 0.0.0.0:8000 (만약 외부에서 접속할경 포트번호를 써줄것인데 파이참 터미널에서 바로 실행할경우에는
default로 저 포트가 잡혀있기 떄문에 안써줘도 된다.)

##크롬http://localhost:8000/

http://localhost:8000/admin 으로 가면 관리계정으로 로그인하는 페이지가 나오는데
우리는 수퍼유저가 등록을 안했기때문에 안된다. 그러니 한번 만들어보자



##터미널
python manage.py createsuperuser

수퍼유저 생성한다.
id: root
password: 1234
email: zotto1004@naver.com으로 생성....
다시 서버키고 실행해보자!!!







###persistance Framework(API): 프로그램 실행을 위한 클래스, 패키지, 테이블 등 파일의 모음
1.이것을 사용하기위한 방법이 두가지가 있는데
SQL Mapping이라고 쿼리문을 직접 작성하여 데이터를 불러오는 방법이 그 중 하나다.
  대표적으로 MyBatis라는게 있다.

2.두번쨰는 ORM으로 (Object Relational Mapping) 이놈은 프로그램 소스내에서 데이터 베이스쪽의 하나의 테이블을 일종의 클래스로
생각하고 프로그램 소스내에서 데이타베이스에 접근하도록 한다.
이러한 것으로 Hibernate라는게 있고 요즘 대세이다. 그러므로 메서드 사용법만 알면 쿼리문을 몰라도 사용할수 있다.


django에서는 2번 방법을 쓰는것이 권고 사항이다.(1번도 쓸수는 있지만 권장되지 않음)


#noticeboard/models.py (데이터베이스를 만드는데 쓸 py파일을 만들어보자)
from django.db import models
from django.utils.timezone import now

Create your models here.
class Notice(models.Model):
    title = models.CharField(max_length=100)
    content = models.CharField(max_length=2000)
    writeDate = models.DateTimeField(default=now, editable=False)
    writeID = models.CharField(max_length=50)
    #여기에 프라이머리키를 설정해줄수있는데 디폴트로도 생성하므로 그거 쓰기위해 그냥 놔둡시다.


    def __str__(self):
    return self.title


#noticeboard/admin.py (좀전에 만든 models를 admins에서 사용할수 있게 해보자)
from noticeboard.models import Notice

admin.site.register(Notice)

이제 여기에서 migrate만 하면 완성이 되지만 우리가 만든Notice에 대한 스크립트가 아직 없기 때문에 잠시 미룬다.



#터미널 (데이터베이스에 적용할 스크립트 생성하기)
python manage.py makemigrations
실행을 하면 migration폴더가 생성된다.

이렇게 models파일을 수정하면 반드시 make migration을 통해 migration script를 만드는것이 기본적인 절차이다.

python manage.py migrate(비로서 마이그레이션을 실행해준다.)

python manage.py runserver
실행하고 브라우저로 접속하며 Notice텝이 새로 생긴것을 볼수 있다.


#mySQL
여기에도 database가 수정된것을 확인할수 있으며 쿼리문을 통해 접근 가능하다.
use django_db;

desc noticeboard_notice;






<URL 주소설계 RESTful URL>
REST API: url 주소가 기존의 폴더, 파일이 아닌 / - / =/ 로 된 URI를 나타낼수 있다는 것이다.
URI는 Unified Resource Identifier라는 의미로 자원식별자라고 할수 있다.
이 하나의 주소가 URI를 나타낸다는것은 기존의 파일 디렉토리와 같은 형식이 아니라 각각의 자원 하나를
유니크하게 식별할수 있는 기능을 제공할수 있다는것이다. 이렇게 유니크하게 주소를 매길수 있는 기능을
django가 default로 제공한다는 것이다.

이제 우리게시판에 필요한 주소를 설정할것인데 어떤 주소가 필요할까?
1.전체 게시판 조회
2....
3.....

#파이참 board/urls.py
board/urls.py가 그러한 주소들을 저장하는 곳이다.

from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('admin/', admin.site.urls),
    path('noticeboard/', include('noticeboard.urls')),
]


<<<여기서 부터는 수업하면서 진행한 경로에 필기를 해둘것임!!>>>


#파이참 noticeboard/urls.py생성
  스크립트 작성......




##templates디렉토리
noticeboard라는 이름으로 디렉토리 생성
  그안에 index.html이라는 이름으로html생성



<전체흐름에 대해>
noticeboard로 들어오면 noticeboard url쪽으로 파싱을 넘긴다. noticeboard로 요청이 들어오면
view라는 함수를 불러오고 그 함수에서 Notice오브젝트의 모든것을 긁어다 템플릿에 넘겨준다.
그럼 해당 템플릿에서는 해당 데이터가 있냐 없냐를 따지고 그것을 바탕으로 작성된 html을 클라이언트에 뿌려주게되는것이다.



<temp>
action="{% url 'noticeboard:add_article' %}"



<오늘 구축한 환경을 이동시키는 방법: boardRoot폴더 이하를 zip으로 묶어서 따로 저장 하고
다른 컴퓨터에 PycharmProject 폴더를 새롭게 만들어서 해당 zip파일을 그대로 풀어서 붙여주면 된다



https://blog.naver.com/soseoso/221271399967
{% url %} 태그 : 이 태그의 주 목적은 소스에 URL을 하드 코딩하는 것을 방지하기 위한 것이다. 만일 이 태그를 사용하지 않는다면 다음과 같이 URL을 하드 코딩해야 할 것이다.
<form action = '/polls/3/vote/' method = 'post'>
만일 위와 같이 코딩한다면, 예를 들어 /polls/라는 URL을 /blog/라고 변경하는 경우에 URLconf 뿐만 아니라 모든 html 파일들을 찾아서 변경해줘야 하는 문제가 발생한다. {% url %} 태그를 사용하면 URL이 변경되더라도 URLconf 모듈만을 참조하여 원하는 URL을 추출할 수 있다.

{% url 'namespace:view-name' arg1 arg2 %}
namespace - urls.py 파일의 include() 함수에서 정의한 이름 공간.
view-name : urls.py 파일의 URL 패턴에서 정의한 뷰 함수 또는 패턴 이름
argN : 뷰 함수에서 사용하는 인자로 없을 수도 있고 여러 개인 경우 빈칸으로 구분한다.
[출처] [웹] Django - template (템플릿 태그와 상속)|작성자 Rhans
