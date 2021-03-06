# 부트스트래핑
---
부트스트래핑은 입력된 url을 분석합니다.
입력된 url에 따라서 컨트롤러와 매소드를 동작합니다.

## 기본구조
---
객체지향으로 작성된 프레임워크가 다른 클래스를 호출하기 위해서는 클래스명과 매소드가 필요합니다.

많은 사용자들을 가지고 있는 CI의 경우 입력된 url의 첫번째 구분자는 클래스명, 두번째 구분자는 매소드 명으로 매칭하고 있습니다.

`도메인/클래스명/매소드명`

이러한 고정된 url값은 동작해야 하는 클래스와 매소드를 지정하는 데 매우 편리합니다.

그러기 위해서는 입력된 url을 분석해야 합니다.


## URL
---
지니는 URL을 분석하여 처리하는 별도의 부트스트랩 클래스를 제공합니다.

PHP에서는 입력된 URL REQUEST를 읽어 올수 있는 기능이 있습니다.
먼저 현재 입려된 REQUEST를 확인합니다.

```
// ReWrite로 전달된 부트스트래핑 URL을 분석합니다.
// REQUEST 값을 읽어 옵니다.
$REQUEST = $this->getREQUEST();
```

URI.php
```
/**
* URL에서 GET부분을 제외한 실제 URL부분만 가지고 옵니다.
*/
private function getREQUEST()
{
    return explode('?', $_SERVER['REQUEST_URI'])[0];
}
```

PHP의 슈퍼글로벌 함수 `$_SERVER['REQUEST_URI']`는 브라우저 주소창에 입력된 주소를 읽어옵니다.

만일 입력한 주소에 GET 요청의 파라미터 ?를 같이 작성하는 경우 이 부분은 제외하고 실제적인 URL만 가지고 옵니다.

GET 데이터는 나중에 별도로 `$_GET` 변수를 통하여 읽어 올 수 있습니다.


## BASEURL
---
보통 URL은 `도메인` 이름 다음부터 시작을 하게 됩니다. 하지만, 개발서버등 다른 이유로 인하여 중간에 일부 경로가 필요한 경우가 있습니다.

이렇게 작성된 URL의 경우 BASEURL를 설정합니다. BASEURL는 환경설정에 정의되어 있습니다.

```
$baseurl = $this->App->Config->data("site.BASEURL");
$this->_REQUEST = $this->getURL($REQUEST, $baseurl);
```
입력된 REQUEST에서 `$baseurl`을 제외한 값을 실제 요청값(`$this->_REQUEST`)으로 저장합니다.

```
/**
* 요청한 URI값을 읽어 옵니다.
* $base는 url 시작이 부분을 스킵처리 합니다.
*/
private function getURL($request, $base=NULL)
{
    // \TimeLog::set(__METHOD__);
    if ($base) {
        return str_replace($base, "", $request);
    } else {
        return $request;
    }
}
```

이제 우리가 처리해야 되는 실제적인 값만 남아 있게 되었습니다.


## locale 처리
---
지니는 다국어, 지역, 문화등에 따른 웹페이지 동작을 구분할 수 있는 url을 처리할 수 있습니다.

이를 위해서 별도의 locale 패키지를 제공하고 있습니다. 로케일을 사용하기 위해서 클래스의 인스턴스를 생성하여 줍니다.

```
$this->Locale = new \Jiny\Locale\Locale($app);
```

## URL 저장
---
부트스트랩의 초기화 마지막으로 작업한 URL을 토큰화 하여 저장을 합니다.
```
// URI를 배열화 합니다.
$this->_urls = $this->explodeURI( $this->_REQUEST );
$this->_urlReals = $this->uriREAL($this->_urls);
```

`->urls`는 입력된 REQUEST 내용을 저장하고 있습니다. BASEURL을 포함한 원본 url 토큰 입니다. 배열값 입니다.

```
/**
* 입력한 URI를 구분하여, 배열화 합니다.
* return: array
*/
private function explodeURI($request)
{
    // \TimeLog::set(__METHOD__);

    // URL의 마지막 '/'를 제거합니다.
    // 마지막 공백의 배열생성을 방지합니다.
    $string = trim($request, '/');

    if (!empty($string)) {
        // URL값이 있는 경우, 구분화 합니다.
        return explode('/', $string);
    }
    return NULL;
}
```



`->realurls`은 후처리를 완료한 REQUEST url값을 가지고 있습니다.

## uriREAL
---


## parser
---
초기화된 url값을 이용하여 파싱처리를 합니다.

```
$this->parser();
```
`parser`의 동작은 크게 3가지로 구분됩니다.

```
/**
* uri 에서 컨트롤러 매소드를 분리합니다.
* 분석한 URL은 _controller, _action, _param에 저장됩니다.
*/
public function parser()
{
    if (!empty($this->_urlReals)) {
        //echo "컨트롤러를 생성합니다..<br>";

        // 컨트롤러 선택합니다.
        $this->setController('\Jiny\Core\IndexController');

        // 실행 매서드를 선택합니다.
        $this->setMethod('index');

        // 파라미터
        $this->parmURL();

    } else {
        // root 접속일 경우, URI가 비어있을 수 있습니다.
        //echo "URI가 비어 있습니다.<br>";
    }

    return $this;
}
```
첫번째는 컨트롤러 값을 읽어 처리합니다.
매소들, 파라미터 값을 처리합니다.



# 부트스트랩 소스
---

## URI 정보 읽어오기
---
PHP는 입력되는 URI의 정보를 슈퍼변수 `$_SERVER['REQUEST_URI']` 를 통하여 읽어 올 수 있습니다.


# 부트스트래핑
---
부트스트래핑은 입력된 url을 분석합니다.

지니PHP의 부트스트래핑은 다국어 필터링, 동적 시작위치 지정, 컨트롤러 및 매소드 매칭등의 특별한 기능을 처리할 수 있습니다.

## 도메인
---
사용자로 부터 입력된 URL은 도메인과 서브경로 형태로 입력을 하게 됩니다.

```php
http://도메인/경로1/경로2/경로3/경로4
```

경로는 웹서버의 디렉토리를 의미합니다. 또한 마지막의 경로는 디렉토리 또는 파일을 의미하기도 합니다.

### 매칭
프레임워크는 이렇게 요청된 URL과 일치된 파일이나 디렉토리의 경로가 존재하지 않는 경우가 많습니다. 기본적으로 웹서버는 디렉토리나 파일이 일치되지 않는 경우 `404` 에러를 출력하게 됩니다.
그럼 없는 파일과 디렉토리를 프레임워크는 어떻게 서비스를 할 수 있는 것일까요?

그 원리는 바로 REWITE 설정에 있습니다. REWITE설정은 `.htaccess`에서 정의되어 있습니다. 존재하지 않는 파일과 디렉토리 접속시 다른 파일로 처리될 수 있도록 재설정하는 것입니다.
하지만, 재설정된 파일이 사용자가 접속한 `uri`에 대한 동작을 수행하기 위해서는 uri를 분석이 필요합니다.

uri를 분석하기 위해서는 먼저 접속한 url값을 읽어 와야 합니다.

```php
/**
 * URL에서 GET부분을 제외한 실제 URL부분만 가지고 옵니다.
 */
private function getREQUEST()
{
    return explode('?', $_SERVER['REQUEST_URI'])[0];
}
```





### 컨트롤러/메소드
---
객체지향으로 작성된 프레임워크가 다른 클래스를 호출하기 위해서는 클래스명과 매소드가 필요합니다.

많은 사용자들을 가지고 있는 CI의 경우 입력된 url의 첫번째 구분자는 클래스명, 두번째 구분자는 매소드 명으로 매칭하고 있습니다.

`도메인/클래스명/매소드명`

이러한 고정된 url값은 동작해야 하는 클래스와 매소드를 지정하는 데 매우 편리합니다.

그러기 위해서는 입력된 url을 분석해야 합니다.



## 다국어
---
URL을 통하여 입력된 `locale` 정보를 판별할 수 있습니다.

보통적으로 `locale`정보는 url의 첫번째 인자로 전달되는 경우가 많습니다. `http://도메인/locale/실제주소` 형태로 접속이 됩니다.
이렇게 입력된 locale 정보를 분석하여, 국가/언어를 판별합니다.

로케일은 크게 3종류로 구분이 됩니다.
* 국가
* 언어
* 문화코드

문화코드는 미국, 캐나다등 다민족 국가를 대상으로 국가와 언어를 한쌍으로 만들어진 코드를 말합니다.

### 코드분리
---
처음으로 들어오는 url의 인자값을 분석하면 국가, 언어, 문화 코드를 분리해 낼 수 있습니다.

약간의 아이디어를 통하여 이를 간략하게 합니다.
로케일코드 특성을 분석해 보면 국가 코드는 2자리로 구성되어 있습니다. 또한 언어코드도 2자리로 구성되어 있습니다. 반면에 문화코드는 `us-en`과 같이 5자리로 구성이 되어 있습니다.

첫번째 인자값이 2자리 또는 5자일 경우에만 로케일을 검사하는 것입니다. 그이외에는 로케일 코드가 아닐 수 있기 때문에 스킵을 합니다.

```php
if (isset($urls[0])) {
    // 로케일 코드값은 2글자 또는 5글자 입니다.
    $lenLC = strlen($urls[0]);
    if($lenLC == 2 || $lenLC == 5) {
        $Locale = Registry::create(\Jiny\Locale\Locale::class, "Locale", $this);
        return $Locale->parser($urls);

    } else {
        // 로케일 글자수가 다른 경우에는
        // 로케일 처리를 하지 않습니다.
    }
}
```

모든 사용자가 locale 코드를 사용하여 접속을 하는 것은 아닙니다.
매번 locale 코드를 분석하는 과정은 많은 부트스트래핑 시간을 소요하게 만들 수 있습니다.

### 로케일 페키지
---
로케일 코드를 분석하는 과정은 별도의 `locale` 페키지로 분리되어 있습니다. 이렇게 패키지를 분리하는 것은 다른 응용프로그램에서도 `locale`처리는 매우 유용하기 때문입니다.
공개된 지니PHP locale 페키지를 많은 응용프로그램이 만들어 질 수도 기대하기 때문입니다.

로케일에 대한 자세한 부분은 로케일 부분을 참고해주시길 바랍니다.


## 기본구조


## URL
---
지니는 URL을 분석하여 처리하는 별도의 부트스트랩 클래스를 제공합니다.

PHP에서는 입력된 URL REQUEST를 읽어 올수 있는 기능이 있습니다.
먼저 현재 입려된 REQUEST를 확인합니다.

```php
// ReWrite로 전달된 부트스트래핑 URL을 분석합니다.
// REQUEST 값을 읽어 옵니다.
$REQUEST = $this->getREQUEST();
```

URI.php
```php
/**
* URL에서 GET부분을 제외한 실제 URL부분만 가지고 옵니다.
*/
private function getREQUEST()
{
    return explode('?', $_SERVER['REQUEST_URI'])[0];
}
```

PHP의 슈퍼글로벌 함수 `$_SERVER['REQUEST_URI']`는 브라우저 주소창에 입력된 주소를 읽어옵니다.

만일 입력한 주소에 GET 요청의 파라미터 ?를 같이 작성하는 경우 이 부분은 제외하고 실제적인 URL만 가지고 옵니다.

GET 데이터는 나중에 별도로 `$_GET` 변수를 통하여 읽어 올 수 있습니다.

## BASEURL
---
보통 URL은 `도메인` 이름 다음부터 시작을 하게 됩니다. 하지만, 개발서버등 다른 이유로 인하여 중간에 일부 경로가 필요한 경우가 있습니다.

이렇게 작성된 URL의 경우 BASEURL를 설정합니다. BASEURL은 환경설정에 정의되어 있습니다.

```php
$baseurl = $this->App->Config->data("site.BASEURL");
$this->_REQUEST = $this->getURL($REQUEST, $baseurl);
```
입력된 REQUEST에서 `$baseurl`을 제외한 값을 실제 요청값(`$this->_REQUEST`)으로 저장합니다.

```php
/**
* 요청한 URI값을 읽어 옵니다.
* $base는 url 시작이 부분을 스킵처리 합니다.
*/
private function getURL($request, $base=NULL)
{
    // \TimeLog::set(__METHOD__);
    if ($base) {
        return str_replace($base, "", $request);
    } else {
        return $request;
    }
}
```

이제 우리가 처리해야 되는 실제적인 값만 남아 있게 되었습니다.

## local 처리
---
지니는 다국어, 지역, 문화등에 따른 웹페이지 동작을 구분할 수 있는 url을 처리할 수 있습니다.

이를 위해서 별도의 locale 패키지를 제공하고 있습니다. 로케일을 사용하기 위해서 클래스의 인스턴스를 생성하여 줍니다.

```php
$this->Locale = new \Jiny\Locale\Locale($app);
```

## URL 저장
부트스트랩의 초기화 마지막으로 작업한 URL을 토큰화 하여 저장을 합니다.
```php
// URI를 배열화 합니다.
$this->_urls = $this->explodeURI( $this->_REQUEST );
$this->_urlReals = $this->uriREAL($this->_urls);
```

`->urls`는 입력된 REQUEST 내용을 저장하고 있습니다. BASEURL을 포함한 원본 url 토큰 입니다. 배열값 입니다.

```php
/**
* 입력한 URI를 구분하여, 배열화 합니다.
* return: array
*/
private function explodeURI($request)
{
    // URL의 마지막 '/'를 제거합니다.
    // 마지막 공백의 배열생성을 방지합니다.
    $string = trim($request, '/');

    if (!empty($string)) {
        // URL값이 있는 경우, 구분화 합니다.
        return explode('/', $string);
    }
    return NULL;
}
```

`->realurls`은 후처리를 완료한 REQUEST url값을 가지고 있습니다.

## uriREAL






## parser
---
초기화된 url값을 이용하여 파싱처리를 합니다.

```php
$this->parser();
```
`parser`의 동작은 크게 3가지로 구분됩니다.

```php
/**
* uri 에서 컨트롤러 매소드를 분리합니다.
* 분석한 URL은 _controller, _action, _param에 저장됩니다.
*/
public function parser()
{
    if (!empty($this->_urlReals)) {
        //echo "컨트롤러를 생성합니다..<br>";

        // 컨트롤러 선택합니다.
        $this->setController('\Jiny\Core\IndexController');

        // 실행 매서드를 선택합니다.
        $this->setMethod('index');

        // 파라미터
        $this->parmURL();

    } else {
        // root 접속일 경우, URI가 비어있을 수 있습니다.
        //echo "URI가 비어 있습니다.<br>";
    }

    return $this;
}
```
첫번째는 컨트롤러 값을 읽어 처리합니다.
매소들, 파라미터 값을 처리합니다.
