---
title: "[Spring] filter와 interceptor의 차이"
categories:
- SpringBoot
---

이번 포스팅에서는 웹과 관련된 공통의 관심사를 처리하는데 사용되는 필터와 인터셉터에 대해 써보겠습니다. 

필터와 인터셉터는 웹과 관련된 공통적인 로직을 처리한다는 공통점이 있습니다.
다만, 필터의 경우 서블릿 앞쪽에 위치하여 스프링 컨텍스트의 밖에서 요청을 처리합니다.
인터셉터는 서블릿을 거친 후 컨트롤러 전에 위치합니다.

**필터 제한**

일반적으로 HTTP 요청 이후 WAS  → 필터 → 서블릿 → 컨트롤러까지 전달됩니다.  
그러나, 필터에서 필터 체인을 걸어주지 않으면 요청이 서블릿까지 전달되지 않습니다.  
예를들면, 인증에 대한 로직을 필터로 구현할 수 있습니다.

**필터 체인**

필터는 체인으로 구성되며, 지정한 순서에 따라 실행됩니다.

Filter 인터페이스에서 정의된 doFilter 메서드의 파라미터에는 ServletRequest가 있는데, 이는 웹 서블릿에서 사용하는 HttpServletRequest보다 더 상위 클래스입니다.  
서블릿 앞쪽에서 처리되기에 Http 요청 외에도 처리할 수 있는 클래스입니다.   

```
@Override
public void init(FilterConfig filterConfig) throws ServletException {

}
@Override
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

 }
 @Override
public void destroy() {

}
```

**인터셉터 흐름**

스프링 인터셉터는 스프링이 제공하는 기능이며, 따라서 디스패처 서블릿 이후에 호출됩니다.   
일반적으로 HTTP 요청 이후  WAS  → 필터 → 서블릿 → 인터셉터 → 컨트롤러까지 전달됩니다.  
필터와 마찬가지로, 체인을 통해 여러 개의 인터셉터가 연쇄적으로 실행될 수 있습니다.    

***필터의 경우 단순하게 doFilter() 하나의 메서드만 제공되었지만, 인터셉터의 경우 preHandle, postHandle, afterCompletion과 같이 단계적으로 메서드가 제공됩니다.***

![image](https://user-images.githubusercontent.com/72685070/158714501-cc249ce3-57de-4381-a0ed-128c8dfa74b9.png)



```
default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {}  

default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {}  

default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {}

```

* preHandle : 컨트롤러 호출 전에 호출이 되며,  true으로 반환되면 다음 인터셉터 또는 컨트롤러가 실행되며 false로 반환되면 처리가 그대로 끝이 납니다.  
* postHandle : 컨트롤러 호출 후에 호출되며, ModelAndView 객체를 파라미터로 받습니다.



**인터셉터 예외상황**

예외가 발생하면, postHandle은 호출되지 않으며 afterCompletion에 예외정보를 포함하여 호출됩니다.
