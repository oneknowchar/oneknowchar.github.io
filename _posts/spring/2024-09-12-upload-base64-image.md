---
title: '스프링에서 바이트 배열 이미지 파일 base64 인코딩 이미지 조회 하기'
excerpt: 'Upload base64 image'

published: true

categories: spring
tags: spring

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

# 이미지 파일

![](../../images/2024-09-12/2024-09-12-01-23-59.png)

# 코드 공유

## controller

```
@GetMapping("/getImgeIndex")
public String getImgeIndex() {
  log.debug("getImgeIndex");
  return "getImgeIndex";
}

@PostMapping("/getByteImg")
@ResponseBody
public ResponseEntity<FileDTO> getByteImg() throws IOException{
  String imgPath = "C:\\Users\\hjs\\Pictures\\test\\pikachu.png";
  File imgFile = new File(imgPath);

  if(imgFile.exists()) {
    //pure byte[]
    byte[] fileData = Files.readAllBytes(imgFile.toPath());

    //base64 encode
    String base64Data = Base64.getEncoder().encodeToString(fileData);

    return ResponseEntity.ok().body(new FileDTO(fileData, base64Data));
  }else {
    return ResponseEntity.notFound().build();
  }
}
```

## html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="/jquery.js"></script>
</head>
<body>
  <div>
  <h1>byte[] -> base64 encode, </h1>
  <h1>이미지 조회하기</h1>
  <img src="" id="imgId">
  </div>
	$(document).ready(function () {
		$.ajax({
            type: 'post',
            url: '/fileController/getByteImg',
            contentType: false,
            processData: false,
            success: function (res) {
              if(!res.fileData && !res.base64Data) return;

              if(res.fileData === res.base64Data){
                  const imgUrl = "data:image/png;base64," + res.fileData;

                  $('#imgId').attr('src', imgUrl);  // 이미지를 화면에 표시
              }else{
                alert('ERROR!!!');
              }
            },
            error: function (error) {
              console.log(error);
            }
		})
	});
</body>
</html>

```

# 스크립트에서 byte[]과 base64가 왜 같다고 나오는가?

ResponseEntity의 body에 FileDTO와 같은 객체를 담아 반환할 때

Jackson 라이브러리가 자동으로 직렬화(serialization) 과정을 수행합니다.

Jackson은 자바 객체를 JSON으로 변환하는 라이브러리로, Spring Framework에서 RESTful API를 개발할 때 자주 사용됩니다.

파일의 바이트 배열을 FileDTO에 포함시키는 경우, Jackson은 기본적으로 바이트 배열을 Base64로 인코딩하여 JSON으로 직렬화합니다. 이는 JSON 형식이 바이너리 데이터를 직접적으로 표현할 수 없기 때문입니다.

---

여기까지 컨트롤러에서 이미지 파일을 호출, 브라우저에서 이미지 조회를 알아보았습니다.
