iFrame 보다 Ajax 방법을 많이 씀 (jquery Ajax)



### pom.xml

아래의 내용을 pom.xml 에 추가해주기
<dependencies>의 하위 위치 어디든 삽입해주면 됨. 
  
```
<!-- 파일업로드/다운로드 -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
 
<!-- 파일관련 / 썸네일라이브러리 -->
<dependency>
    <groupId>org.imgscalr</groupId>
    <artifactId>imgscalr-lib</artifactId>
    <version>4.2</version>
</dependency>
```
  
  

  
 ### root-context.xml
  
```
  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!--  파일업로드 용량 (10MB)-->
    <property name="maxUploadSize" value="10485760"/>
    <property name="defaultEncoding" value="UTF-8" />
  </bean>
  
  <!--  파일업로드 디렉토리 설정 -->
  <bean id="uploadPath" class="java.lang.String">
    <constructor-arg value="[프로젝트 내 폴더, 절대경로로 설정]"/>
  </bean>
```
  
  
  ### 기능 구현
  
  ```
  (import, package 생략)
 
@Controller
@RequestMapping("/file")
public class FileController {
    
    @Resource(name="uploadPath")
    String uploadPath;
    
    @RequestMapping(value="/upload", method=RequestMethod.GET)
    public String fileupload() {
        return "post/test_file.basic";
    }
    
    @RequestMapping(value="/upload", method=RequestMethod.POST)
    public ModelAndView uploadForm(MultipartFile file, ModelAndView mv) {
 
        String fileName = file.getOriginalFilename();
        File target = new File(uploadPath, fileName);
        
        //경로 생성
        if ( ! new File(uploadPath).exists()) {
            new File(uploadPath).mkdirs();
        }
        //파일 복사
        try {
            FileCopyUtils.copy(file.getBytes(), target);
            mv.addObject("file", file);
        } catch(Exception e) {
            e.printStackTrace();
            mv.addObject("file", "error");
        }
        //View 위치 설정
        mv.setViewName("post/test_upload.basic");
        return mv;
    }
}
  ```
  
  
  참고 링크 : 
  https://joalog.tistory.com/48
  https://youtu.be/I0ChYxQVZJw 의 #1, #2, #3, #4
  https://github.com/indiflex/swp
  
  


