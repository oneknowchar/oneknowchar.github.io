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

---

여기까지 컨트롤러에서 이미지 파일을 호출, 브라우저에서 이미지 조회를 알아보았습니다.
