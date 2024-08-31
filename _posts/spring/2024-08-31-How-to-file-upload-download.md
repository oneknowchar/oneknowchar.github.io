---
title: '스프링에서 파일 업로드, 다운로드 구현하기'
excerpt: 'How to file upload download'

published: true

categories: spring
tags: spring

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

# 코드 공유

## application.yml

```
server:
  port: 8088

spring:
   servlet:
      multipart:
         maxFileSize: 1024MB # 파일 하나의 최대 크기
         maxRequestSize: 1024MB  # 한 번에 최대 업로드 가능 용량
```

## FileController.java

```
@Controller
@RequestMapping("/fileController")
@RequiredArgsConstructor
@Slf4j
public class FileController {
	private final FileSerivce fileSerivce;

	private final String FILE_PATH = "C:\\bfw\\fx\\temp\\doc";

	@GetMapping("/")
	public String index() {
		log.debug("test");
		return "fileIndex";
	}

	@PostMapping(path = "/upload")
	@ResponseBody
	public ResponseEntity<ResponseDTO> test(
			@RequestPart("files") List<MultipartFile> files,
			@RequestPart("human") Human humanObj) throws IllegalStateException, IOException
	{
		String responseMsg = "";
		String yyymmddss = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd_HH_mm_ss"));

		List<String>fileNameList = new ArrayList<>();
		for(MultipartFile file : files) {
			fileNameList.add(file.getOriginalFilename());
		}

		for (MultipartFile multi : files) {
			String fileName = multi.getOriginalFilename();

			log.debug("fileName = {}", fileName);
			log.debug("uploadTime = {}", yyymmddss);
			log.debug("contentType = {}", multi.getContentType());
			log.debug("Resource = {}", multi.getResource());

			//파일 업로드
			File uploadFile = null;
			if(multi.getContentType().contains("video")) {
				uploadFile = new File(FILE_PATH, yyymmddss + "_video_"+ fileName);
				responseMsg = yyymmddss + "_video_"+ fileName;
			}else {
				uploadFile = new File(FILE_PATH, yyymmddss + "_"+ fileName);
				responseMsg = yyymmddss + "_"+ fileName;
			}
			multi.transferTo(uploadFile);
		}

		boolean wanaDelete = false;

		if(wanaDelete) {
			log.debug("deleteFile  업로드 했으니 이제 삭제할께= " + wanaDelete);

			files.forEach((file)->{
				String fileName = file.getOriginalFilename();
				File deleteFile = new File(FILE_PATH, yyymmddss + "_"+ fileName);
				if(deleteFile.exists()) {
					if(deleteFile.delete()) {
						log.debug("deleteFile  fileName[삭제 성공] = {}", fileName);
					}else {
						log.debug("deleteFile  fileName[삭제 실패]= {}", fileName);
					}
				}else {
					log.debug("deleteFile  fileName[파일 없음]= {}", fileName);
				}
			});
		}

		return ResponseEntity.ok().header("kiki", "do you love me").body(new ResponseDTO(responseMsg, wanaDelete));
	}

	@GetMapping("/getImage")
	public ResponseEntity<InputStreamResource> getImage(@RequestParam("fileName") String fileName, @RequestParam(name = "download", required = false) String download) throws IOException {
		File img = new File(FILE_PATH, fileName);

		if (!img.exists()) return ResponseEntity.notFound().build();

		HttpHeaders headers = new HttpHeaders();
		String contentType = Files.probeContentType(img.toPath());
		if (contentType == null) {
			//데이터 형식이 불명확한 경우
			//파일 다운로드: 파일 다운로드와 같이 클라이언트가 파일 형식을 알지 못하거나, 모든 파일을 바이너리로 처리할 수 있는 경우에 적합합니다
			//보안상 고려: 데이터의 실제 형식이 중요하지 않고, 보안상의 이유로 데이터 형식을 노출하지 않는 것이 중요한 경우에 사용할 수 있습니다.
			contentType = "application/octet-stream";
        }

		//MediaType.valueOf()보다 유연하게 처리.
		//미디어타입 형식만 맞는다면 비표준 MIME 타입도 MediaType의 필드로 만 	들어 리턴
		headers.setContentType(MediaType.parseMediaType(contentType));
//		headers.setContentLength(img.length());
		if("Y".equals(download)) {
			headers.setContentDisposition(ContentDisposition.attachment().filename(fileName, StandardCharsets.UTF_8).build());
		}

		InputStream inputStream = new FileInputStream(img);
		InputStreamResource resource = new InputStreamResource(inputStream);	//방법1
//		byte[] byteFile1 = FileCopyUtils.copyToByteArray(img);					//방법2
//		byte[] byteFile2 = StreamUtils.copyToByteArray(inputStream);			//방법3
		return ResponseEntity.ok().headers(headers).body(resource);
	}
}

```

## fileIndex.html

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <script src="/jquery.js"></script>
</head>
<style>
  mainDiv {
    display: flex;
  }
  #imgDiv {
    border: 1px solid green;
    width: 50%;
    /* 이미지가 있으면 커서 : pointer */
    cursor: default;
  }
</style>
<body>
  <mainDiv>
    <div id="formDiv">
      <form action="" method="post" enctype="multipart/form-data">
        <div>
          <h1>Upload test</h1>
          <h2>Multipart</h2>
          <div>
            <input type="file" name="file" id="file1">
          </div>
          <div>
            <input type="file" name="file" id="file1">
          </div>
          <div>
            <input type="file" name="file" id="file3">
          </div>

          <h2>JSON</h2>
          <hr>
          <pre>
Human : {
  name : '<span id="nameSp">user00</span>',
  age: '<span id="ageSp">22</span>'
}
          </pre>
          <hr>
          <div>
            <input type="text" name="name" value="user00" id="nameInput" class="human" placeholder="name">
          </div>
          <div>
            <input type="text" name="age" value="22" id="ageInput" class="human" placeholder="age">
          </div>
        </div>

        <div>
          <button type="button" id="apiBtn">api test button</button>
          <button type="button" id="reset" onclick="location.reload()">reset</button>
        </div>
      </form>
    </div>
    <div id="imgDiv">
      <div>이미지 미리보기</div>
      <div>
        <img id="preview" src="" height="50%" width="50%" alt="이미지를 업로드 해 주세요." title="다운로드 클릭">
      </div>
      <div>
        <div>비디오 미리보기</div>
        <video controls height="50%" width="50%">
          <source src="" type="video/mp4">
        </video>
      </div>
      <div id="downloadDiv" style="display: none;">
        <a href="">download</a>
      </div>
    </div>
  </mainDiv>
  <script>
    const SERVER_URL = '/fileController';

    $(document).ready(function () {
      //인풋 타이핑시 화면에 표시한다
      $('#nameInput, #ageInput').on('keyup', function (e) {
        console.log(e.target.value)
        const id = $(this)[0].name;

        if (id == 'age' && isNaN(e.key)) {
          $(this).val('')
        }
        $('#' + id + 'Sp').text($(this).val());
      });

      //apiBtn 클릭시 이미지를 업로드 한다
      //업로드한 이미지를 조회한다
      $('#apiBtn').on('click', function () {
        console.log('apiBtn click!!');

        const formData = new FormData();
        let isFile = false;

        //이미지 업로드
        $('input:file').each(function () {
          if (this.files[0]) {
            formData.append('files', this.files[0]);
            isFile = true;
          }
        });

        //이미지 업로드 체크
        if (!isFile) {
          alert('이미지 업로드 필수!');
          return false;
        }

        const human = {
          name: $('#nameInput').val(),
          age: $('#ageInput').val()
        };

        // formData.append("human", JSON.stringify(human)); //string, 서버에서 JSON 파싱!!
        formData.append("human", new Blob([JSON.stringify(human)], { type: "application/json" })); //json 전송

        $.ajax({
          type: 'post',
          url: `${SERVER_URL}/upload`,
          data: formData,
          contentType: false,
          processData: false,
          dataType: 'json',
          success: function (res) {
            const encodedFileName = res.responseMsg;

            if (!encodedFileName.includes('video')) {
              $('#preview').attr('src', `${SERVER_URL}/getImage?fileName=${encodedFileName}`);
              $('#preview').css('cursor', 'pointer');

              $('#preview').on('click', function () {
                if (confirm('이미지를 다운로드 하시겠습니까?')) {
                  location.href = `${SERVER_URL}/getImage?fileName=${encodedFileName}&download=Y`
                }
              });
            } else {
              $('video>source').attr('src', `${SERVER_URL}/getImage?fileName=${encodedFileName}`);
              $('video')[0].load();
              $('#downloadDiv').show();
              $('#downloadDiv').children('a').attr('href', `${SERVER_URL}/getImage?fileName=${encodedFileName}&download=Y`);
            }
          },
          error: function (error) {
            console.log(error);
          }
        });
      });
    });
  </script>
</body>
</html>
```

---

여기까지 컨트롤러에서 멀티파트와 제이슨을 같이 받는법을 알아보았습니다.
