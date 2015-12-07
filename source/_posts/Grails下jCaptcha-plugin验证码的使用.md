title: Grails下jCaptcha plugin验证码的使用
date: 2015-12-05 17:28:07
tags: grails
---
grails添加验证码插件,
在<code>BulidConfig.groovy</code>文件中,plugins里面加上
```
compile "org.grails.plugins:jcaptcha:1.5.0"
```
重启项目,这时候可能会有错误:
<!-- more -->
```
Resolve error obtaining dependencies: Could not transfer....
```
这是由于墙的问题导致资源下载不下来,这时候在<code>repositories</code>中加入oschina的maven仓库,
```
mavenRepo "http://maven.oschina.net/content/groups/public"
```
重启,这时候没有报错,OK.
现在开始进行配置jcaptchas,在<code>Config.groovy</code>文件中加入
```
jcaptchas {
    imageCaptcha = new com.octo.captcha.service.multitype.GenericManageableCaptchaService(
            new com.octo.captcha.engine.GenericCaptchaEngine(
                    new com.octo.captcha.image.gimpy.GimpyFactory(
                            new com.octo.captcha.component.word.wordgenerator.RandomWordGenerator("0123456789"),//配置验证码
                            new com.octo.captcha.component.image.wordtoimage.ComposedWordToImage(
                                    new com.octo.captcha.component.image.fontgenerator.RandomFontGenerator(Integer.valueOf(20), Integer.valueOf(30)),
                                    new com.octo.captcha.component.image.backgroundgenerator.GradientBackgroundGenerator(
                                            150, // 设置验证码宽度
                                            50, // 设置验证码高度
                                            new com.octo.captcha.component.image.color.SingleColorGenerator(java.awt.Color.WHITE),
                                            new com.octo.captcha.component.image.color.SingleColorGenerator(java.awt.Color.WHITE)
                                    ),
                                    new com.octo.captcha.component.image.textpaster.NonLinearTextPaster(4, 4, java.awt.Color.BLACK)) )//设置验证码位数
            ),
            180, // minGuarantedStorageDelayInSeconds
            180000, // maxCaptchaStoreSize
            75000 // captchaStoreLoadBeforeGarbageCollection
    )
}
```
在页面中的代码:
```
<g:form action="image">
	<jcaptcha:jpeg name="imageCaptcha" /><br>
	<g:textField name="code" value=""/><br>
	<g:submitButton name="submit" value="Submit" />
</g:form>
```
**注意:页面标签中的name的值要和Config.groovy中jcaptchas的name保持一致!!!**
controller中的代码:
```
class LoginController {
    def jcaptchaService

    def image = {
        if (params.submit) {
            flash.message = (jcaptchaService.validateResponse("image", session.id, params.code)) ? "Text matches" : "Text doesn't match"
        }
        render view: 'image'
    }
}
```
**注意:
    1.如果验证码有英文,区别大小写!!!
    2.验证码只能验证一次
**

另外,点击刷新验证码功能会有点问题:因为```<jcaptcha:jpeg >```标签吧图片路径封装起来，不能够获得src属性值，所以没有办法写JS脚本来实现换一张功能.
解决方案:换为```<img>```标签.
例:

&lt;img id="imageCaptcha" onclick="changePic()"  src="/jcaptcha/jpeg/imageCaptcha?id=${new Date().getTime()}"/>

写一段JS脚本:

functon changePic(){
    $("#imageCaptcha").attr("src","/jcaptcha/jpeg/imageCaptcha?id="+new Date().getTime());
}
即可解决该问题.



