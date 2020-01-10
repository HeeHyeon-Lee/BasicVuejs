# 뷰 컴포넌트 통신
## 컴포넌트 간 통신과 유효범위
컴포넌트마다 자체적으로 고유한 Scope가 있기 때문에 뷰는 컴포넌트로 화면을 구성하므로 같은 웹 페이지라도 데이터를 공유할수 없음.
각 컴포넌트의 유효범위가 독립적이기 때문에 다른 컴포넌트의 값을 직접적으로 참고할 수 없다.
- 이해를 돕기위한 예제파일 : **index.html**

위의 코드에서 cmp2에 아무것도 표시가 되지 않는데, 이는 my-component2에서 my-component1의 값을 참조할 수 없기 때문이다.

## 상·하위 컴포넌트 관계
- 컴포넌트 간의 참조를 하기 위해서는 뷰 프레임워크 자체에서 정의한 컴포넌트 데이터 전달 방법을 따라야함.
- 가장 기본적인 데이터 전달 방법은 **부모-자식 컴포넌트간의 데이터 전달방법**
- 지역or전역 컴포넌트를 등록하면 등록된 컴포넌트는 자연스럽게 자식 컴포넌트가 된다. 마찬가지로 하위 컴포넌트를 등록한 인스턴스는 상위 컴포넌트가 된다.
- 상위에서 하위로는 props라는 특별한 속성을 전달한다.
- 하위에서 상위로는 기본적으로 이벤트만 전달한다.

## 상위에서 하위 컴포넌트로 데이터 전달하기
### props 속성
- 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성.
```
// 하위 콤포넌트의 props 속성 정의 방식
Vue.component('child-component', {
  props: ['props 속성 이름']
})
// 상위 컴포넌트의 HTML 코드
<child-component v-bind:props 속성 이름="상위 컴포넌트의 data 속성"></child-component>
```
v-bind 속성의 왼쪽 값으로 자식에서 정의한 props속성을 넣고, 오른쪽 값으로 컴포넌트에 전달할 부모의 data 속성을 지정한다.
- props 속성을 사용한 데이터 전달 예제 : **index2.html**

## 하위에서 상위 컴포넌트로 이벤트 전달하기
### 이벤트 발생과 수신
- 이벤트를 발생시켜(event emit) 부모 컴포넌트에 신호를 보낸다.
- 부모는 자식의 특정 이벤트가 발생하길 기다리고 있다가 자식에서 이벤트가 발생하면 해당 이벤트를 수신하여 자신의 메서드를 호출함.
### 이벤트 발생과 수신 형식
- 발생과 수신은 $emit()과 v-on: 속성을 이용하여 구현.
```
// $emit을 이용한 이벤트 발생
this.$emit('이벤트명')
```
```
// v-on: 속성을 이용한 이벤트 수신
<child-component v-on:이벤트명="상위 컴포넌트의 메서드명"></child-component>
```
- emit은 자식의 특정 메서드 내부에서 호출. emit 호출시 사용하는 this는 자식을 가리킴.
- 호출한 이벤트는 자식을 등록하는 태그(상위 컴포넌트의 template 속성에 위치)에서 v-on으로 받는다.
  - 자식에서 발생한 이벤트명을 v-on에 지정하고 속성의 값에 이벤트 발생 시 호출될 상위 컴포넌트의 메서드를 지정한다.
- 예제파일 : **index3.html**

## 같은 레벨의 컴포넌트 간 통신
- 뷰는 부모에서 자식으로만 데이터를 전달해야 하기 때문에 자식에서 공통된 부모에 이벤트를 전달하고 공통된 부모가 2개의 하위 컴포넌트에 props를 내려 보내는 방식으로 전달해야함.
- 이런 통신 구조를 유지하려면 강제로 부모 컴포넌트를 두어야함. 해당 해결 방법이 이벤트 버스.

## 관계없는 컴포넌트 간 통신 - 이벤트 버스
- 부모-자식 관계를 유지하고 있지 않아도 데이터를 컴포넌트 to 컴포넌트로 전달할 수 있는 방식

### 이벤트 버스 구현 형식
```
// 이벤트 버스를 위한 추가 인스턴스 1개 생성
const eventBus = new Vue();

// 이벤트를 보내는 컴포넌트
methods: {
  메서드명: function() {
    eventBus.$emit('이벤트명', 데이터)
  }
}

// 이벤트를 받는 컴포넌트
methods: {
  created: function() {
    eventBus.$on('이벤트명', function(데이터){
      ...
    })
  }
}
```
**예제 : index4.html**