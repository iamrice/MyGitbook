# 准备：自我介绍

面试官好，我是陈泰佑，来自华南理工大学。

我第一次接触前端开发是在大一下学期，当时使用小程序开发了一个益智类小游戏，借助微信平台的传播，积累了一千多用户量。项目前端使用基础的小程序开发组件，逻辑与前端三件套相似，服务端使用小程序的云数据库和云函数。通过这个项目，学习了前端开发的基础知识，例如页面渲染、事件绑定、状态维护等。

在大二期间，我参加了两次小程序开发项目，分别是“识图作诗”智能AI小程序和 “圈圈带学”大学校园学习平台。

识图作诗智能AI小程序，应用图像识别AI接口，对用户上传的图片进行识别，根据识别结果在云端数据库中匹配相关的古诗词，返回给用户。同时，小程序提供了诗词详细注解、诗词卡片生成等功能。圈圈带学是和其他三位学长合作开发，项目针对大学生学习中的痛点，为学生提供了辅导信息交流，资料共享，个人课表等功能，我在项目在负责用户注册登录，辅导页面的设计。两个项目均为比赛作品，取得了一定的成绩。在这几次的项目开发中，更加深刻地巩固了前端开发的知识，学习到了一些正确的开发理念，例如拆解封装复杂的业务逻辑，注重数据请求的安全性等。

除了小程序开发项目，我也参与过Web项目开发。在大三上学期，参与开发了一个校园二手商品交易平台，这是数据库的课程作业。该项目前端采用React框架，后端采用django框架，我在项目中负责技术选型，整体框架的搭建和商品浏览页面的开发。通过这个项目学习了react框架的使用，应用react特殊语法、组件化等特性，提高了开发效率和代码质量。



version 2:

    面试官好，我是陈泰佑，来自华南理工大学计算机学院，目前大三，就读于计算机科学与技术全英创新班。我熟悉的语言有c++\js，参与过多个前端项目，包括微信小程序开发，例如开发过一个大学生校园学习平台，使用微信小程序开发组件，项目获得小程序大赛华南赛区二等奖；同时我也参与过web项目开发，项目是一个校园二手商品交易平台，项目前端使用react框架，后端使用django框架。除此之外我也参与过实验室科研项目，主要研究群体智能计算，例如基于群体智能的分布式大规模优化，基于群体智能的交通路网车流规划等。我还参与过一个编译器开发项目，使用c++开发，实现了一个ARM平台上的sysy语言编译器，项目获得华为编译系统大赛全国三等奖。

    编译器项目中，我主要负责将抽象语法树转译为线性执行的中间代码，然后将中间代码转译为ARM平台上的汇编语言。其中涉及到的几个难点是：1. 将树状的语法树转为线性的中间代码，中间代码的表示解决汇编代码，只有变量赋值、算术运算、标签跳转、数值比较四种语句，每个语句最多包含三个变量。因此，主要工作在于将条件语句和循环语句拆解开，转为多个代码块，使用goto在代码块之间进行跳转。2. 将中间代码转为ARM汇编，为变量、数组分配地址，并高效地利用仅有的13个寄存器。当然，除此之外，还涉及许多细节的问题，例如如何处理多重逻辑运算符、如何处理变量作用域等等。

    问面试官的问题：犀牛鸟项目的实际工作中，是由学生自主开发还是会加入相关的团队由前辈带着做呢?

version 3: 遇到过印象深刻的问题

      面试官好，我是陈泰佑，来自华南理工大学计算机学院，目前大三，就读于计算机科学与技术全英创新班。我参与过多个前端项目，包括微信小程序开发，例如开发过一个大学生校园学习平台，使用微信小程序开发组件，项目获得小程序大赛华南赛区二等奖；开发过一个识图作诗小程序，独立设计完成；同时我也参与过web项目开发，项目是一个校园二手商品交易平台，项目前端使用react框架，后端使用django框架。除此之外我也参与过实验室科研项目，主要研究群体智能计算，例如基于群体智能的分布式大规模优化，基于群体智能的交通路网车流规划等。

      我介绍一下自己做过的一个项目，叫识图作诗，核心功能是识别用户上传的图片，匹配与图片内容相关联的古诗词。图像识别是调用了AI接口，诗词是通过爬虫整理储存在数据库中。除此之外，也提供诗词详细注解和诗词卡片生成的功能，诗词卡片使用的是canvas绘制。我觉得这个项目亮点可能在于这个创意吧，不过缺点也在于，核心功能的算法不能很好的实现这个创意，首先是现代汉语和古代汉语的语料库没有打通，可能一个词在古代有多种不同的说法，需要处理好这层映射；第二是对古诗词的标签挖掘不够充分，目前只是停留在字面上的匹配，但很多优美的诗词的表达是隐晦的，因此如果能够挖掘出诗词语义中的标签，这个产品会更好。

        我之前有一次为一个社团活动做一个扫码签到的demo，业务场景是，到场的同学扫描公共二维码，获得一个唯一的ID，这个ID用于会后领取讲座票以及中场抽奖，所以这个ID必须保证唯一并且不超过200个，先到先得。我是用小程序云数据库存的数据，前端调用云数据库的接口，当时在写分配ID的代码时，我就遇到一个问题，小程序开发文档中有一个inc函数，用于数据库中整数字段的自增，我认为这个是适合用于统计人数和分配序号的，在多个用户同时扫码时不会造成冲突。但这个函数有一个问题，他在完成自增之后并不会给回调函数返回当前值，也就意味着，修改操作和查询操作是分离的，可能在修改和查询两个操作中间，有其他用户对数据进行了修改，虽然在服务端对用户的统计是正确的，但用户查询到的序号不一定是正确的。 当时为了解决这个问题，查了很多资料以及在开发社区提问，有相同的提问帖，但都没有找到解决方法。所以我去请教了一个云开发团队的开发人员，他返回给我一个云开发团队开发的nodejs SDK文档，这个SDK中的inc函数就实现了真正的原子自增。 这个经历还蛮有趣的，如果我自己一个人钻研，那我关注的都是如何利用当前这个小程序开发SDK解决问题，很难想到小程序的云数据库有另一个版本的SDK实现。

        另一个印象深刻的经历是，我在开发一个识图作诗的小程序时，业务场景是这样的，：用户上传图片之后，要经过四个流程，第一将用户上传的图片转为base64编码；第二步，调用一个图像识别的AI接口，上传图片编码，接受返回的特征标签；第三步，在诗词数据库中匹配与特征标签相关的古诗词；第四步，将检索到的古诗词展示在前端页面。在这个业务流程中，首先我是遇到js异步特性的问题，因为这个流程环环相扣，为了保证他们是同步执行，那么我一开始是把下一步的代码写在上一步的回调函数中，但是这个就造成了所谓的回调地狱，多层嵌套，代码臃肿。为了解决这个问题，我了解到了异步解决方案promise，学着用promise把每个步骤的代码单独封装起来，再在主函数做链式调用。

        其实我做的这些前端项目，大多不是为了积累前端经验而做的，更多的是源于产品开发的热情，或许这可以解释为程序员的表达欲，希望自己所学可以展示给大众看。所以可能我的这些项目经历中，并没有太关注性能优化这方面，因为个人开发的产品很多时候在引流阶段就扑街了，用户量上不去，还没到需要稳定性维护的阶段；很多时候用户的反响更多来自于上线了什么新功能。不过，我倒还是有一点性能方面的可以讲，当时并没有做好优化，只是从现在的角度看有可以改进的地方。之前第一次尝试小程序开发，做了两个益智类的小游戏，在朋友圈内反响颇好，用户量积累到2k左右，在日活量最好的那几天，服务器资源是不太够的，云数据库提供每日5万次访问，其实5万次挺多了，但造成服务器负担的原因是，我当时做的双人对战高阶井字棋中，因为需要获取对方的实时操作，所以我采取了轮询的方式。但是现在想想，其实可以用websocket方案来替代轮询。

version 5

我来自华南理工大学，目前大三，就读于计算机科学与技术专业全英创新班。在前端开发方面，我参与过微信小程序开发和web 开发。小程序方面我参与过大学生校园学习平台的开发，在项目中负责用户的注册登录和课程浏览页面的开发设计。我也独立开发过一款识图作诗智能AI小程序；在web开发方面，曾经参与过校园二手商品交易网站的开发，是课程大作业，我在项目中负责前后端基础架构，商品浏览页面的设计开发。以上就是我的项目经历。

