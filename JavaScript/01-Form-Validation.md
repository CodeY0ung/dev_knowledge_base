# Form 유효성 검사 (d1028)

- `submit` 이벤트에서 `preventDefault()`로 기본 제출을 중단한다.
- 필수 입력값과 `select` 선택 여부를 확인한다.
- 입력이 비어 있으면 `alert()`와 `focus()`로 안내하고 제출을 중단한다.
- 검증을 통과하면 `form.submit()`으로 제출한다.
- 등록 성공 여부에 따라 안내창을 띄우고 메인 화면으로 이동한다.

```javascript
form.addEventListener("submit", (event) => {
  event.preventDefault();
  if (!field.value.trim()) {
    alert("필수 항목을 입력하세요.");
    field.focus();
    return;
  }
  form.submit();
});
```

브라우저 검증은 우회 가능하므로 서버에서도 필수값을 검증해야 한다.
