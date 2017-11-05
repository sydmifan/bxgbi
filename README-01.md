# 博学谷项目
## 目录结构
bxg9
    js/
        teacher
            add.js
            index.js
            edit.js
        course
            index.js-->课程列表
            add.js-->添加课程
        lib：存放第三方的纯js库
            jquery.js
            require.js
    assets/：存放第三方的项目(可能拥有js/css/fonts等资源文件)
        bootstrap
        ueditor
        uploadify
        datetimepicker
    css/
        index.less/
    tpls/   -->用于存放网站中的模板文件
        teacherAddTpl.html
        courseAddTpl.html
    login.html  -->登录页
        -->没有模块化开发
    index.html  -->主页
        -->用requireJS进行模块化开发
    main.js
        -->requireJS的入口文件
## jquery事件回调函数有一个参数：事件对象e
+ e.preventDefault();阻止事件的默认行为
+ e.stropPropagation();阻止事件冒泡

## 登录功能？
1. bootstrap进行页面布局
2. 要进行表单提交-->异步提交
    1. 阻止页面跳转-->通过给form标签绑定submit事件，并且return false阻止默认行为就可以实现阻止页面跳转
    2. 自己通过ajax的方式把数据提交到服务器中验证？
        + 要进行ajax，就必须要有接口地址、接口类型，接口请求参数这三个条件
            - 接口地址，接口类型通过接口文档：http://doc.botue.com得知
            - 请求参数（要提交的数据）：
                - 基本原理：$("form").serialize();获取表单的序列化数据
                    - 要获取的表单数据，该表单必须要有name属性，name属性应该通过接口文档查看参数说明，一一对应的编写

## 错误情况
1. 整个博学谷项目在调试的时候，一定，必须，千万要用昨天配置的网站来访问
    -- file
    -- localhost
    -- 127.0.0.1
        -- bxg.com
2. 请求失败404的情况？
    1. 看下数据有没有
    2. 看看数据内容对不对
        tc_name：前端学院
        tc_pass：123456


## arttemplate模板引擎
### 介绍
+ 作者：糖饼
+ github：https://github.com/aui/art-template
+ 官网：https://aui.github.io/art-template/

### 基本使用
+ 下载arttemplate
+ 页面中引用arttemplate
+ 编写模板内容：
    var tpl="hi,{{value}}";
+ 编译模板内容：
    var html=template.render(tpl,{ value:100 });//正常编译完成之后的内容存放在html中

### API比较
template("script的id",数据);//-->弊端：一定要在页面中准备无数个模板


template.render("模板字符串",数据);


template("模板文件路径",数据);


## 首页菜单的切换
1. 给菜单绑定事件
    1. 通过给菜单的容器绑定事件，通过事件委托由不同的菜单触发
2. 事件触发的时候，判断用户到底是什么菜单？
    1. 通过给不同的菜单添加了不同的类名
    2. 事件触发的时候，判断该菜单是否有指定的类名，根据不同的类名实现不同的功能
        $("div").hasClass("hover")
3. 发现页面一旦加载完成之后，就出现了讲师列表？
    1. 实际上点击了讲师管理菜单就可以出现讲师列表
    2. 解决方案：通过模拟点击讲师管理菜单实现该功能

## 讲师列表的加载-->teacher/list中实现的
1. 编写了teacher/list模块
    1. 准备讲师数据
        ajax：/api/teacher   数据存放在返回值的result中
    2. 准备讲师列表的模板
        tpls/teacherList.html-->通过requireJS提供的text插件获取模板内容
    3. 将讲师数据放在模板中编译，获取到真实内容
        arttemplate
        var html=template.render(模板内容,数据);
    4. 把真实内容放到页面的指定位置
        $(".panel-content .panel-body").html(html);
2. 在main.js中，首先引入teacher/list的模块依赖；然后在菜单切换的时候，调用teacher/list