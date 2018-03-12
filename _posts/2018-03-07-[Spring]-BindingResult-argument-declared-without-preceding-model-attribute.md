메소드에서 파라미터로 `BindingResult` 를 사용하는 경우, 반드시 `@ModelAttribute` 뒤에 위치해야 한다.

수정 전: (에러 발생)

```
	public String functionName(@ModelAttribute("attr1") Attr1 attr1,
			@PathVariable String no, BindingResult result,
			SessionStatus status, Model model, HttpSession session) throws Exception {
		...
	}
```

수정 후: (@PathVariable과 위치 변경)

```
	public String functionName(@ModelAttribute("attr1") Attr1 attr1,
			BindingResult result, @PathVariable String no,
			SessionStatus status, Model model, HttpSession session) throws Exception {
		...
	}
```

Spring 개념에 대한 기본 지식이 부족하다는 걸 느꼈다.