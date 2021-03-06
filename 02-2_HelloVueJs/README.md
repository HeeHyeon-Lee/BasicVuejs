# 코드 분석
**new Vue**: 인스턴스
```
el: '#app' // el 속성
data: { } // data 속성
```
1. Hello Vue.js! 텍스트를 표현하기 위해 new Vue()로 인스턴스 생성.
2. el속성으로 인스턴스가 그려질 지점을 지정
3. data속성에서 message 값을 정의하여 화면의 {{ message }}에 연결함.

# 자세한 설명
### 뷰 인스턴스 생성자
- new Vue()로 인스턴스를 생성할 때 Vue를 생성자라고 함.
- Vue 생성자는 뷰 라이브러리를 로딩한 후에 접근가능함.
- 생성자를 사용하는 이유는 뷰로 개발할 때 필요한 기능들을 생성자에 미리 정의하고, 사용자가 그 기능을 재정의하여 편리하게 사용하도록 하기 위해서임.

### 뷰 인스턴스 옵션 속성
- 인스턴스를 생성할 때 재정의할 data, el, template등의 속성을 의미함.
- 본 코드에서는 data라는 미리 정의된 속성을 사용함.
- el 속성도 미리 정의되어 있으며, 뷰로 만든 화면이 그려지는 시작점을 의미
- el 속성에서 사용하는 선택자는 css선택자 규칙과 동일
- 이 외에도 template, method, created등 미리 정의되어 있는 속성 사용 가능.
    - **template**: 화면에 표시할 HTML, CSS등의 마크업 요소를 정의하는 속성. 뒤에 5장에서 설명
    - **methods**: 화면 로직 제어와 관계된 메서드를 정의하는 속성. 마우스 클릭 이벤트 처리와 같이 화면의 전반적인 이벤트 및 화면동작과 관련된 로직을 추가함.
    - **created**: 뷰 인스턴스가 생성되자마자 실행한 로직을 정의할 수 있는 속성. 뷰 인스턴스 라이클부분에서 추가로 설명.

### 뷰 인스턴스의 유효 범위
- 뷰 인스턴스를 생성하면 **HTML의 특정 범위 안에서만 옵션 속성들이 적용**되어 나타나는데, 이를 인스턴스의 유효 범위라고 한다.
- 다음 절에서 다루는 지역 컴포넌트와 전역 컴포넌트의 차이점을 이해하기 위해서도 꼭 알아야 하는 개념
- 유효범위는 el 속성과 밀접한 관계가 있음.
- new Vue()로 인스턴스를 생성하고 나서 화면에 인스턴스 옵션 속성을 적용하는 과정은 다음과 같다.
    1. 뷰 라이브러리 파일 로딩
    2. 인스턴스 객체 생성 (옵션속성 포함)
    3. 특정 화면 요소에 인스턴스를 붙임.
    4. 인스턴스 내용이 화면 요소로 변환.
    5. 변환된 화면 요소를 사용자가 최종 확인.

### 뷰 인스턴스 라이프 사이클
앞에서 살펴본 속성중 created은 인스턴스가 생성되었을 때 호출할 동작을 정의하는 속성이다. 이처럼 인스턴스의 상태에 따라 호출할 수 있는 속성들을 **라이프 사이클 속성**이라고 한다.
또한 각 라이프 사이클 속성에서 실행되는 커스텀 로직을 **라이프 사이클 훅**이라고 한다.
1. beforeCreate
2. create
3. beforeMount
4. mounted
5. beforeUpdate
6. updated
7. beforeDestroy
8. destroyed
