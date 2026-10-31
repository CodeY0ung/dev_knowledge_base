# Servlet · JSP 파일 업로드 (d1030)

## 1. 단일 파일 업로드

HTML Form에 `enctype="multipart/form-data"`를 지정한다. **multipart는 여러 개의 Part로 구성된 요청 본문 형식**이라는 뜻이며, 반드시 여러 파일을 보낸다는 의미는 아니다.

```jsp
<form action="upload" method="post" enctype="multipart/form-data">
    작성자: <input type="text" name="author">
    <input type="file" name="file">
    <button type="submit">업로드</button>
</form>
```

Servlet에 `@MultipartConfig`를 지정하고 파일을 `Part`로 받는다.

```java
@WebServlet("/upload")
@MultipartConfig(
    fileSizeThreshold = 1024 * 1024,       // 1MB: 임시 디스크 저장 임계값
    maxFileSize = 1024 * 1024 * 10,        // 파일당 10MB
    maxRequestSize = 1024 * 1024 * 50      // 요청 전체 50MB
)
public class UploadServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        Part file = request.getPart("file");
        if (file == null || file.getSize() == 0) return;

        Path uploadDir = Path.of("/var/app/uploads"); // 환경에 맞춰 별도 설정
        Files.createDirectories(uploadDir);
        String storedName = UUID.randomUUID().toString();
        file.write(uploadDir.resolve(storedName).toString());
    }
}
```

- `request.getParameter("author")`: 일반 입력값.
- `request.getPart("file")`: 이름이 `file`인 업로드 파일.
- `getSubmittedFileName()`: 브라우저가 전달한 원본 이름.
- `Part.write()`: 업로드 파일을 저장.
- `getServletContext().getRealPath("/uploads")`: 배포된 웹 애플리케이션의 실제 경로. 재배포 시 파일이 사라지거나 환경에 따라 경로가 없을 수 있으므로 **영구 저장소로 가정하면 안 된다**.

위 예제는 파일 저장 API를 보여주는 축약 코드다. 실제로는 업로드 경로 설정, 확장자·MIME·내용 검증, 접근 권한 확인, 예외 처리가 필요하다. 업로드 원본 파일명을 그대로 저장 경로에 연결하면 경로 조작이나 덮어쓰기 위험이 있으므로 서버에서 안전한 저장명을 생성한다.

## 2. 여러 파일 업로드

```html
<form action="uploadMulti" method="post" enctype="multipart/form-data">
    <input type="file" name="files" multiple>
    <button>업로드</button>
</form>
```

```java
for (Part part : request.getParts()) {
    if (!"files".equals(part.getName())) continue;
    if (part.getSize() == 0 || part.getSubmittedFileName() == null) continue;
    // 파일별 크기·형식 검증 후 고유한 파일명으로 저장
}
```

- `multiple`은 한 입력 항목에서 파일 여러 개를 선택하게 한다.
- `getParts()`는 파일뿐 아니라 멀티파트 요청의 일반 필드 Part도 포함한다.
- 실습의 `fileSizeThreshold`는 **단일 Part가 메모리에 유지될지 임시 디스크를 이용할지에 대한 임계값**이며, 업로드 크기 제한은 `maxFileSize`/`maxRequestSize`로 구분한다.

## 3. 파일과 DB

실습에서 배운 구성은 이미지 파일을 파일 저장소에 보관하고 DB에는 파일 경로나 식별자를 기록하는 방식이다. DB에 BLOB으로 파일 내용을 저장하는 것도 기술적으로 가능하므로 반드시 경로만 저장해야 하는 것은 아니다.

```text
요청 → Servlet(Part) → 파일 저장소
                      → DB에 파일 식별자/메타데이터 저장
```

파일 저장 성공과 DB 등록 실패가 서로 다른 작업이므로 실패 시 임시 파일 정리 등을 고려한다.

**참고:** [jsp_board 실습 저장소](https://github.com/CodeY0ung/jsp_board) (선택적 참고 자료).
