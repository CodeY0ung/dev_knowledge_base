# HTML Form · CSS 배치 (d1028)

- `<select>`와 `<option>`으로 선택형 입력을 만든다.
- 폼 필드의 `name`은 서버에서 읽는 요청 파라미터 이름과 일치시킨다.
- `background-color`로 배경색을 적용한다.
- `text-align: center`로 텍스트를 가운데 정렬한다.
- `margin: 0 auto` 또는 Flexbox로 블록 요소를 가운데 배치한다.

```html
<select name="category" required>
  <option value="">선택</option>
  <option value="A">A</option>
</select>
```

```css
body { background-color: #f5f5f5; }
table { margin: 0 auto; }
section { display: flex; justify-content: center; }
```
