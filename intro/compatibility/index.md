---
layout: intro
theme: jiny/page
---

# 호환성
---
새로운 기능과 제품이 나오게 되면 학습을 위한 진입 장벽이 발생합니다.  
새로운 제품이 쉽게 시장에 융합되기 위해서는 기존의 솔루션들과 호환성과 사용방법들을 인용하는 것이 중요합니다.

<br>
## 지킬 & 깃허브 호환
---
최근들어 복잡한 동적 웹사이트외에 간단한 정적 웹사이트가 많은 인기를 얻고 있습니다.  
대표적으로 `루비` 언어로 작성된 `jekyll`이 있습니다. `jekyll`은 본인의 컴퓨터에서 html, 마크다운로 작성된 페이지를 레이아웃과 결합하여 정적인 html 파일을 생성해 주는 툴입니다. 이렇게 생성된 웹사이트를 깃허브에 등록하여 웹페이지 블로그로 서비스를 할 수 있습니다.  

하지만, 컨덴츠의 내용이 많이지는 경우에는 `jekyll`과 같은 툴을 이용하여 컨덴츠 페이지들을 관리하기에는 벅찬 부분이 있습니다.  

지니 PHP는 생성, 변경되는 페이지를 동적으로 변환하여 수많은 컨덴츠를 관리 서비스할 수 있게 도와 줍니다.  
또한, 지킬과 깃허브에서 사용하던 문서포맷 `머리말`을 분석하고 템플릿 언어 `liquid`까지 사용을 할 수 있습니다.  
별도의 수정작업 없이 지니PHP로 전환하여 서비스 페이지를 유지할 수 있습니다.  

<br>
## 프레임웍 호환
---
지니PHP는 기존의 다른 프레임워크를 사용하던 개발자들이 쉽게 습득을 하기 위해서 호환성을 고려하여 설계를 하고 있습니다.  
예를 들면, 함수명과 메소드명등을 일치하게 하는 작업들 입니다.

라라벨과 같은 프레임워크는 화면출력을 위해서 `view()` 헬퍼함수를 사용합니다.  
지니PHP 또한 화면을 출력하기 위해서 동일한 `view()` 헬퍼 함수를 제고합니다. 하지만, 내부 기능은 약간의 차이가 있습니다.

<br>
<br>