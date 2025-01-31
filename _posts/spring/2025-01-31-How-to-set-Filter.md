---
title: '스프링 부트에서 필터 등록 사용하는 법'
excerpt: 'How to set Filter'

published: true

categories: spring
tags: spring

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: 'docs'
---

# 스프링에서 필터 등록하기

## @ServletComponentScan 필수 등록  
```java  
@ServletComponentScan // 어노테이션 등록
@SpringBootApplication
public class JavaWebWorkBookApplication {
	public static void main(String[] args) {
		SpringApplication.run(JavaWebWorkBookApplication.class, args);
	}
}
```  

@WebFilter는 Tomcat의 서블릿 관련 어노테이션입니다.  
Spring의 어노테이션이 아닙니다.  
Spring Boot에서 @WebFilter를 사용하려면 @ServletComponentScan을 추가해줘야, Spring이 이를 Tomcat에 등록하여 필터가 동작하게 됩니다.


## 필터 클래스 생성  
```java
@WebFilter(urlPatterns = "/todo/*")
public class LoginCheckFilter implements Filter{

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		
		HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
		
		HttpSession session =  req.getSession();
		
		String loginInfo = (String) session.getAttribute("loginInfo");
		
		if(loginInfo != null) {
			chain.doFilter(request, response);
		}else {
			res.sendRedirect("/wayOut");
		}
	}
}
```  

@WebFilter(urlPatterns = "~~~")  
url 패턴에 맞춰 필터가 실행됩니다.  

---

여기까지 스프링의 Filter에 대해 알아보았습니다.
