### EXG 地图消息翻译仓库 ###
## 如何使用
  找到仓库内对应地图的文件，修改其中内容即可
## 没有地图消息文本?我改了为什么没有生效?
  通常是因为文本同步有延迟，请耐心等待
## 修改格式
  请不要破坏文本的json格式,也不要修改文本的编码, 因此你需要使用一些比较高级的编辑器，推荐使用Notepad++或Visual Studio Code.

  json内首尾一对总的中括号括住所有词条，请不要改动

  每一个消息词条首尾由一组花括号括住，相邻词条间有逗号隔开，请不要改动

  每一个消息词条内有数个属性(由键值对表示),格式为  "键": 值,请不要改动双引号和键名，只修改冒号后的值.

  每一个条目内包含以下属性
## original
  表示消息的原始文本，请不要修改！！即使原文本有误，也请在下面的翻译内修改
## zh_trans
  表示消息的中文翻译，可以修改冒号后的文本,注意需要保留前后的英文双引号!!!

  请注意，如果需要配合timer_num进行倒计时,需要将对应数字写为阿拉伯数字

  如果original已经是中文了，也需要在这里复制一次
## timer_num
  表示消息内需要作为倒计时的数字,值不需要加引号，只接受大于等于0的整数。例如消息" 5 secs to open the door"，如果需要倒计时的话这里需要填写5作为值。若在剩余15:00时触发该语句，左下角会提示 [地图消息] 5 secs to open the door@倒计时14:55。

  如果zh_trans内没有匹配的数字,则无效(包括数字写成汉字，或者有多个匹配的数字的情形).
## enable_timer
  是否开启屏幕中央倒计时提示,1为开启，0为关闭，不需要加引号。为1时，还需要timer_num不为0.

  如果某条语句需要有左下角地图消息倒计时但不想要中央倒计时，单独将此条改为0即可。
## 倒计时
    自动识别的数字可能有错误，也可能原倒计时本身就有问题。插件在做匹配时匹配的是zh_trans(中文翻译)内的数字,下面介绍一些具体情况:
<pre><code>
    "original": "The door will open in 20 seconds",
    "en_trans": "",
    "zh_trans": "大门将在20秒后打开",
    "timer_num": 20,
    "enable_timer": 1
</code></pre>
  这里zh_trans内的唯一阿拉伯数字是20,timer_num后也是20，因此匹配,有正常的倒计时，下面我们看几个不太正常的例子已经改进建议(略去其他无关条目)
  <pre><code>    
    "original": "The door will open in 10 seconds",
    "en_trans": "",
    "zh_trans": "大门将在5秒后打开",
    "timer_num": 10,
    "enable_timer": 1
</code></pre>  
这里翻译者发现原语句写的是10秒，但实际是5秒开门，所以把中文语句改成了5秒。但自动识别生成的是10秒，而翻译者没有修改，因此不会触发倒计时。

修改意见:将"timer_num"后的值改为5
  <pre><code>    
    "original": "5 secs to open the door 5",
    "en_trans": "",
    "zh_trans": "5秒后开启5号大门",
    "timer_num": 5,
    "enable_timer": 1
</code></pre>  
这里由于zh_trans中含有2个数字5,都与timer_num中的数字匹配,因此不会有倒计时

修改意见:将zh_trans改为 "5秒后开启五号大门"

(汉字数字不会匹配)
  <pre><code>    
    "original": "5 secs to open the door 1",
    "en_trans": "",
    "zh_trans": "5秒后开启1号大门",
    "timer_num": 0,
    "enable_timer": 1
</code></pre>  
这里timer_num为0是因为第一次生成语句时自动识别发现有2个数字，因此不开启倒计时。

修改意见:将timer_num改为5，此时1不会匹配,5会正常匹配

  <pre><code>    
    "original": "defend 2 minutes",
    "en_trans": "",
    "zh_trans": "防守2分钟",
    "timer_num": 2,
    "enable_timer": 1
</code></pre>  
这里如果在15:00触发了该语句，会显示[地图消息]防守2分钟@倒计时 14:58,同时中央倒计时也会显示为:"防守2分钟"..."防守1分钟"...,因为对数字的解释是当做秒的

修改意见:"zh_trans"改为"防守120秒",同时把timer_num改为120
  <pre><code>    
    "original": "MAP ZE_FFVII_MAKO_REACTOR_V5 BY HANNIBAL",
    "en_trans": "",
    "zh_trans": "地图 ZE_FFVII_MAKO_REACTOR_V5 制作由 HANNIBAL",
    "timer_num": 5,
    "enable_timer": 1
</code></pre>  

这里很滑稽的把地图版本号识别做了倒计时

修改意见:timer_num改为0即可。也可在此基础上将enable_timer改为0
