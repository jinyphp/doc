# 라우터

## 라우팅
라우팅은 url 구조에 매칭되어 동작되는 처리 로직을 지장하는 방법입니다.

보통 프로그램은 처리로직이 동작한 다음 결과를 화면에 출력을 합니다. 하지만 처리로직과 화면을 같이 구성을 하는 것은 유지 보수 차원에서 어렵습니다

Mvc 패턴 방식의 프래임워크는 이러한 처리로직과 화면을 분리하여 관리를 합니다. 따라서 어떤 화면과 처리로직이 한쌍으로 이루어 져야 하는지 매핑하는 룰이 필요로 합니다.

코드이그나이터 와 같이 명시작으로 url 구조에 따라서 클래스. 매서드. 값을 구분하여 사용할 수 있습니다. 이 방식은 별도의 라우팅 작업을 필요로 하지 않습니다.

하지만 명시적인 규칙을 따르지 않는 경우 이를 처리할 규칙을 정의할 필요가 있습니다. 이를 라우팅이라고 합니다.

여러가지 정보를 가지는 라우팅은 배열 데이터로 되는 경우가 많습니다. 입력한 url에 맞는 값을  찾아서 함수나 클래스를 호출하게 됩니다.

라라벨, 코드이크나이터와 같은 프레임 워크는 배열 데이터를 정규 패턴화 하여 검색하는 반복문 구조로 되어 있습니다.

## 역구조 리버싱
지니는 입력된 url의 반복구조를 최소화 하기 위해서 다차원 배열로 구성이 되어 있습니다.

일차원 배열의 경우 유사하지 않은 구문자의 값도 전체 검색을 하기 때문에 속도가 많이 걸릴 수 있습니다.

## 구조매칭

지니는 컨트롤러와 메서드를 호출하는 구조는 url 매칭으로 연결되어 있습니다.

기본적으로는 `도매인/컨트롤러/매서드/인자값1/인자값2/인자값3...` 형태의 구조를 가지고 있습니다.

하지만 이러한 규칙을 벗어나 다른 형태로 url과 컨트롤러를 연결하고 싶은 경우가 있을 것입니다.
이를 위해서 별도의 라우터 를 제공합니다.



## 라우트 구조
---

기본적으로 CI, Laravel 과 같은 프레임워크의 라우터 구조는 배열을 응용한 검색 알고리즘 입니다.
만일 1~100까지의 숫자가 썩여 있는 배열에서 50이라는 값을 찾기 위해서는 처음부터 끝까지 배열의 값을 모두 검색을 해야만 합니다.

만일 라우트의 조건이 많아 질 경우 이러한 검색 처리 알고리즘은 성능에 부담이 줄 수 있습니다.

하지만 지니는 이러한 구조를 개선하기 위해서 각각의 처리동작 라우트를 파일단위로 작성을 합니다. 파일명은 유일한 값이며, 고유한 특징으로 즉각적인 처리가 가능합니다.
입력된 url이 입력되면 가장 유사한 구조의 라우트 설정파일을 읽어서 반환을 처리합니다.



## 라우트 파일작성
---

라우트 파일은 `/app/route`폴더 안에 존재합니다. url의 디렉토리 구분자 `/`는 `_`로 대체하여 작성을 합니다.


