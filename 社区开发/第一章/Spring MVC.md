# Spring MVC

* 三层架构
	* 表现层、业务层、数据访问层
* MVC 
		* Model：模型层
		* View：视图层
		* Controller：控制层
* 核心组件
	* 前端控制器：DispatcherServlevt

<img src="/home/tqr/Study-Notes/社区开发/第一章/image/2020-02-05 17-46-35 的屏幕截图.png" alt="2020-02-05 17-46-35 的屏幕截图" style="zoom:50%;" />

三层架构和MVC 不能对应，三层架构是指，浏览器访问软件系统的表现层，调用业务层，访问数据层。MVC 是一种设计模式，为了解决表现层的*问题*，浏览器访问是访问的controller，调用业务层代码，得到数据封装到model中，传给view经过处理返回用户数据。---- 三层架构是指软件架构，mvc指的是设计模式，为了解决主要是表现层出现的问题。DispatcherServlet是核心

<img src="/home/tqr/Study-Notes/社区开发/第一章/image/springMVC.png" alt="springMVC" style="zoom:50%;" />

请求（servlet engine，例如tomcat内部）到达前端控制器，根据注解的路径找到相应的Controller，委派给相应的controller处理，controller将数据封装到model中返回给前端控制器，前端控制器在将model传递给视图View，通过模板引擎生成网页，例如Thymeleaf 生成html，在返回给前端控制器，再由前端控制器返回给浏览器。与开发的时候需要编码的部分主要是Model、Controller、View三部分，前端控制器DispatcherServlet不用管是SpringMVC自动管理的。

-------------

### Thymeleaf

* 模板引擎
	* 动态的生成html
* Thymeleaf
	* 倡导自然模板，以HTML文件为模板
* 常用语法
	* 标准表达式
	* 判断与循环
	* 模板的布局

<img src="/home/tqr/Study-Notes/社区开发/第一章/image/image-20200205182344126.png" alt="image-20200205182344126" style="zoom:50%;" />



-----------------------

开发时关闭Thymeleaf缓存，在application.properties中配置，在SpringBoot的手册中附录里有介绍，各种配置都有。

-----------------

视图层三部分mvc，model部分是Spring封装好的，不用写了，c在controll中，v在resource中的template里

response 首先要设置返回内容的格式和字符集.

-------------------

使用get请求传参

```java 
// 对应于访问方式 localhost:8080/community/alpha/student?current=90&limit=98
    @RequestMapping(path = "student", method = RequestMethod.GET)
    @ResponseBody
    public String getStudents(
            @RequestParam(name = "current", required = false, defaultValue = "1") int current,
            @RequestParam(name = "limit", required = false, defaultValue = "2") int limit) {
        System.out.println("current:" + current);
        System.out.println("limit" + limit);
        return "find a lot of students";
    }

    // 另一种访问方式 localhost:8080/community/alpha/student/{id}
    @RequestMapping(path = "/student/{id}", method = RequestMethod.GET)
    @ResponseBody
    public String getAStudent(@PathVariable("id") int id) {
        System.out.println("id:"+id);
        return "get a student";
    }
```

为什么很多时候传递参数的时候不使用get请求，因为get请求传参。两个限制1：显示，不安全2：长度限制。

------------------------------

post方法提交数据

```
@RequestMapping(path = "/student", method = RequestMethod.POST)
    @ResponseBody
    public String saveStudent(int age,String name){
        System.out.println("name is "+name);
        System.out.println("age is "+age);
        return "success";
    }
```

向浏览器响应数据

```java
//两种方法
@RequestMapping(path = "/teacher", method = RequestMethod.GET)
    public ModelAndView getTeacher() {
        ModelAndView mav = new ModelAndView();
        mav.addObject("name", "江老师");
        mav.addObject("age", "32");
        mav.setViewName("/demo/view");
        return mav;
    }

    @RequestMapping(path = "/school", method = RequestMethod.GET)
    public String getSchool(Model model) {
        model.addAttribute("name", "郑州大学");
        model.addAttribute("age", 100);
        return "/demo/view";
    }
```

响应json数据：在异步请求中常用json数据

```java
@RequestMapping(path = "emp",method = RequestMethod.GET)
    @ResponseBody
    public Map<String,Object> getEmp(){
        Map<String,Object> emp = new HashMap<>();
        emp.put("name","铁钦锐");
        emp.put("age",22);
        emp.put("gender","男");
        return emp;
    }
    
    @RequestMapping(path = "emps",method = RequestMethod.GET)
    @ResponseBody
    public List<Map<String,Object>> getEmps(){
        List<Map<String,Object>> list = new ArrayList<>();

        Map<String,Object> emp = new HashMap<>();
        emp.put("name","铁钦锐");
        emp.put("age",22);
        emp.put("gender","男");
        list.add(emp);

        emp = new HashMap<>();
        emp.put("name","李雪");
        emp.put("age",22);
        emp.put("gender","女");
        list.add(emp);

        emp = new HashMap<>();
        emp.put("name","张晓青");
        emp.put("age",22);
        emp.put("gender","女");
        list.add(emp);

        return list;
    }
```









