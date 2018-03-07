외부 JS 모듈을 사용하다가 디자인 이슈로 인해 두 개의 형제 div를 하나의 부모 div로 묶어야 하는 상황이 생겼다. 처음에 어떻게 접근할 지 조금 헤매다가 JS 메인 호출 함수의 맨 뒷부분에서 직접 DOM 수정 jQuery를 사용했다.

아래와 같이 형제 div가 있으면, 각 div에 공통 클래스를 추가해준 후 그 클래스에 대해 .wrapAll() 함수를 사용하면 된다.

```html
<!-- ... -->
<div id="div1"></div>
<div id="div2"></div>
<!-- ... -->

<script>
$.fn.jsLibarary = function(options) {
  
  ...
  
  // 디자인 작업을 위한 DOM 수정
  // - 두 개의 div를 상위 div 하나로 감싸기
  $('#div1').addClass('wrapping');
  $('#div2').addClass('wrapping');
  $('.wrapping').wrapAll('<div class="wrapper-div" />');
  $('#div1').removeClass('wrapping');
  $('#div2').removeClass('wrapping');
  
  return this;
}
</script>
```

전후에 클래스 추가/제거하는 과정을 더 간단히 할 수 있으면 좋을텐데.. 좀 더 알아봐야겠다.