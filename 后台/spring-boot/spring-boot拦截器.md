### 拦截器

#### 自定义拦截规则

**HandlerInterceptor **

> 此处没有加`@Component`标签，后面需要以return bean的方式向 `WebMvcConfigurationSupport `注册自定义拦截器

```java
public class InterceptorConfig implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //给前端返回信息
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json;charset=UTF-8");
        PrintWriter writer = response.getWriter();
        //ReturnObj 为自己定义的返回类
        ReturnObj obj = ReturnObj.fail();
        obj.setMsg("此处不同行!!!");
        //将内容返回给前端
        writer.write(JSON.toJSONString(obj));
        
        
       	//return true就是放行,
        //false 就是拦截
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

#### 配置前端控制器（可能不是这个名字）

 `WebMvcConfigurationSupport `

```java
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {

    @Override //前端控制器
    protected void addViewControllers(ViewControllerRegistry registry) {
        //拦截url，放行页面
        registry.addViewController("/test").setViewName("/test");
    }

    /**
     * 拦截器配置方法
     * @param registry
     */
    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(interceptorConfig())
                //拦截全部
                .addPathPatterns("/**")
                //放行的请求
                .excludePathPatterns("/login","/register");
    }

    @Bean
    public InterceptorConfig interceptorConfig(){
        return new InterceptorConfig();
    }
    /**
    如果InterceptorConfig已经利用@component注册进Spring后
    可以用:
    @Autowired
    InterceptConfig interceptorConfig;
    */
    //方式注册自定义拦截器
}
```

