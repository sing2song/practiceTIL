# MVT 만들기

## 모델

모델을 만들자! 일반적인 순서는 없지만 모델부터 작업하는 것이 편리하다.

모델을 만드는 것은 database에 table을 만드는 것과 같다.

모델은 class로 구현된다.

우리는 polls라는 어플리케이션을 만들었으니까 해당 폴더 아래에 생성한다.

polls/models.py => model 정의하는 파일

이 안에서 우리는 Question / Choice 클래스를 만든다! (필요한 데이터테이블 이름)



> models.py

class를 상속받아서 model을 만들어줘야한다.

table의 column은 속성으로 표현한다.

속성과 쓰는 방법이 따로 정해져있다.

id는 클래스를 만들 때 자동으로 생성이 된다.

def _ _ str_  _(self) : 객체의 주소가 아닌 텍스트 내용이 출력되도록 str 함수를 설정해준다.

```python
from django.db import models

# Create your models here.
class Question(models.Model):
    # 이렇게 정의되는 class가 database table과 매핑이 된다.
    # Table의 column은 어떻게 정의해야하는가. => 속성으로 표현
    question_text = models.CharField(max_length=200)
    # 200자 이상은 불가능
    pub_date=models.DateTimeField('date published')
    # ''=>해당컬럼이 어떤 컬럼인지를 나타내기 위한 속성. 없어도된다.
    
    def __str__(self):
        return self.question_text
  
```



**외래키 설정시에 _id라고 칭해지는 명은 후에 자동으로 설정되므로 question으로만 정의해준다!!!**

> Foreign key의 제약사항 (constraint)

이 키와 연결되어있는 다른테이블의 내용도 함께 지워야하는 제약사항이 걸린다.

`CASCADE` : 연관된 것이 지워지면 같이 지워라!

```python
class Choice(models.Model):
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    question = models.ForeignKey(Question, on_delete=models.CASCADE)

    #객체의 주소가 아닌 텍스트 내용이 출력되도록 str 함수를 설정해준다.
    def __str__(self):
        return self.choice_text
```



> admin.py

여기다 모델을 등록하면 어드민 사이트에 뜨게 된다!