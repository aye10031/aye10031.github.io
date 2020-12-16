---

layout: post

title: "Android studio无法渲染xml布局的解决办法"

date: 2018-09-11

tag: 教程

---

最近重装了系统，于是AS也重新安装了一下，结果创建项目之后发现不能预览布局文件了，这可不好玩，毕竟我不是脑补大师。        
于是查了一下资料，对比了一下以往的工程文件和Android的API，解决了之后还是在这里记录一下。        
作为案例这里新建一个项目。可以看到我添加了一个Button组建，然而preview窗口中并没有渲染：
![](\images\posts\androidXML\an01.PNG)
解决方法就是打开AndroidManifest.xml文件，
![](\images\posts\androidXML\an02.PNG) 
注意
```
 android:theme="@style/AppTheme"
```

这一字段，现在我们要做的就是查看`@style/AppTheme`这一配置文件，
按住CTRL键点击就可以打开，      
![](\images\posts\androidXML\an03.PNG)      
打开这个styles.xml文件，关注
```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
```
![](\images\posts\androidXML\an04.PNG)      
将其修改为
```
<style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
```
说白了就是在前面加上`Base.` ：
![](\images\posts\androidXML\an05.PNG)      
现在可以看到能成功的渲染布局了：
![](\images\posts\androidXML\an06.PNG)      

>**9月18日更新：**      
>亲测此问题是由于使用了Android API 28（也就是Android9）造成的，之前的应该没有这个问题。