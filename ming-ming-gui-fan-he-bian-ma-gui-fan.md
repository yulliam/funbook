# 命名规范和编码规范

转载：[http://blog.csdn.net/crazy1235/article/details/51346027](http://blog.csdn.net/crazy1235/article/details/51346027)

---

有过团队开发经验的人都知道，如果没有一定的规范可行，那么代码看起来将是苦不堪言，甚至是乱七八糟。

下面就介绍一下，我个人编码过程中使用到的规范，供大家参考~~

---
# 命名规则介绍

命名规范要望文知义，简单明了。 命名规范定制太多，就会让人心烦，反而没人遵守了。 ---《APP研发录》

先介绍两种命名规则：

- **驼峰命名法：**又称小驼峰命名法。除了首个单词首字母小写除外，其余所有单词所有首字母都要大写。

- **帕斯卡命名法：**又称大驼峰命名法。所有单词首字母大写。
---
包名的命名规范
包名一律小写

建议采用如下规则：【com】.【公司名/组织名】.【项目名称】.【模块名】

比如：com.jacksen.mvp.demo。然后在这个目录下根据业务逻辑进行分层。

常见的包分层结构如下：

com.xxx.xxx.view –> 自定义view 或者是View接口

com.xxx.xxx.activities –> activity类

com.xxx.xxx.fragments –> fragment类

com.xxx.xxx.adapter –> 适配器相关

com.xxx.xxx.utils –> 公共工具类

com.xxx.xxx.bean –> 实体类

com.xxx.xxx.service –> service服务

com.xxx.xxx.broadcast –> 广播接收器

com.xxx.xxx.db –> 数据库操作类

com.xxx.xxx.persenter –> 中间对象

com.xxx.xxx.model –> 数据处理类

类的命名规范
​Android中类的命名与JAVA开发采用一致的规范即可。

大驼峰命名法，即所有单词首字母大写。

Activity –> xxxActivity.java

Application –> xxxApplication.java

Fragment –> xxxFragment.java

Service –> xxxService.java

BroadcastReceiver –> xxxBroReceiver.java

ContentProvider –> xxxProvider.java

Adapter –> xxxAdapter.java

Handler –> xxxHandler.java

接口 –> xxxInter.java

接口实现类 –> xxxImpl.java

Persenter –> xxxPersenter.java

公共父类 –> BaseActivity.java、BaseFragment.java、- BaseAdapter.java等

util类 –> LogUtil.java

数据库类 –> BaseSQLiteDBHelper.java

变量的命名规范
采用驼峰命名规则。

Java普通变量：
resultString

userBean

loginPresenter

Android控件变量：
loginBtn

inputPwdEt

showNameTv

有些人建议采用【控件缩写】+【控件逻辑名称】的方式，比如btnLogin。不过我个人比较习惯反过来写，比如loginBtn。与类的命名类似，把逻辑名称写在前面。

常用控件的缩写
控件

布局文件中缩写

代码中缩写

LinearLayout

xxx_layout

xxxLLayout

RelativeLayout

xxx_layout

xxxRLayout

FrameLayout

xxx_layout

xxxFLayout

TextView

xxx_tv

xxxTv

EditText

xxx_et

xxxEt

Button

xxx_btn

xxxBtn

ImageView

xxx_iv

xxxIv

CheckBox

xxx_chk

xxxChk

RadioButton

xxx_rbtn

xxxRbtn

ProgressBar

xxx_pbar

xxxPbar

ListView

xxx_lv

xxxLv

WebView

xxx_wv

xxxWv

GridView

xxx_gv

xxxGv

常见单词的缩写：
单词

缩写

icon

ic

background

bg

foreground

fg

initial

init

information

info

success

succ

failure

fail

error

err

image

img

library

lib

message

msg

password

pwd

length

len

buffer

buf

position

pos

常量命名： 
全部单词采用大写，每个单词之间用“_”分割。

例如：

public static final String API_URL = "http://apis.baidu.com/heweather/weather/free";

1

1

方法的命名规范
与java开发类似，采用驼峰命名规则。首单词首字母小写，其余单词首字母大写。尽量不要使用下划线。

举例：

setxxx()

getxxx()

loginxxx()

onCreate()

onDestory()

isxxx() –> 返回值是boolean类型

checkxxx()

资源的命名规范
全部采用小写，单词之间使用下划线分割。

布局文件：
activity_login.xml

fragment_first_tab.xml

item_choose_city.xml

dialog_choose_city.xml

common_footer.xml

popup_xxx.xml

控件ID：
上面【常用控件的缩写】表格中基本列出了常用控件的ID写法。

login_btn

input_phone_et

input_pwd_et

login_pbar

drawable目录下的命名规范
全部单词小写，单词之间采用下划线分割。

图标 – > ic_xxx.png –> ic_logo.png

背景图 –> bg_xxx.jpg –> bg_splash.jpg

selector –> selector_login_btn.xml

shape –> shape_login_btn.xml

图片状态 –> bg_login_btn_pressed.jpg & - bg_login_btn_unpressed.jpg

anim目录下的命名规范
单词全部小写，单词之间采用下划线分割。

fade_in.xml

fade_out.xml

slide_in_from_left.xml

slide_in_from_top.xml

slide_out_to_right.xml

slide_out_to_bottom.xml

编码规范
代码中尽量不要出现中文。注释和除外。代码中通过strings.xml引用来显示中文。

控件声明放在activity级别，这样在activity其他地方可以使用。

在一个View.OnClickListener中处理所有的点击事件逻辑，这样看起来很集中和直观。

strings.xml中使用%1sd等实现字符串的通配。

布局文件中的字体大小，都定义在dimens.xml中。

有关margin和padding的值也都放在dimens.xml中。

界面之间传值尽量使用intent方式。少用全局变量。

不建议在布局文件中添加点击事件。

数据类型转换一定要校验。

使用常量代替枚举。

实体不要在不同模块间共享，但是可以在统一模块下的不同页面共享。

建议采用左括号与方法名称在同一行的代码格式来进行代码的编写和格式化。貌似左括号在下一行是C#的形式。

业务稍微复杂一些，都有可能提炼一个BaseActivity或BaseFragment出来做为公共父类。

类注释一定要写，管家的方法也要写方法注释。常量尽量写注释。

项目中的命名规范和编码规范，是一个项目Leader前期需要准备的，也是一项必备技能。

制定好了规范，就要遵守，有了统一的规范，项目才好维护，相互之间才好review代码，便于开发与维护。