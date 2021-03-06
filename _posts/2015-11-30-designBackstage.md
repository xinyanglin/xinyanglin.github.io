---
layout:     post
title:      "近期设计后台的心得"
subtitle:   " \"又叫这几天踩过的坑\""
date:       2015-11-30 23:09:00
author:     "Linxy"
header-img: ""
tags:
    - Work
---

>怎么设计后台？第一步，梳理清楚业务流程。

在刚开始加入现在的团队时，就接到了第一个任务－－根据已有的业务和低保真原型，梳理出一份新的，可供开发人员查阅的需求文档。在对证券行业未熟悉的情况下，要去接触一个金融后台，确实遇到了不少的困难。以下便是近段时间接触后台设计以来的一些想法。

## 用大于看
初步接触后台设计时，百度与知乎上面有许多关于后台设计的看法，万变不离其中的就是［梳理业务逻辑］。拿到手的需求，需要一步步转化，翻译成为一个个界面的排版，按钮位置，以及跳转。对于界面设计没有很高要求的后台系统而言，个人认为这一步是非常重要的，至少在原型这一步，就要让后台能让普通人能够以较低的学习成本进行使用。同时，这也能节省一定的设计成本－－即便没有UI，每个人也都能使用。

许多产品设计文章都提到：后台要在界面上花些功夫，毕竟也是人在用。好看的界面当然会让人用得清爽，但其实实际状况是：如果是公司自己人使用的后台，那么开发团队不会投入太多地成本在界面上。除非这个后台是给外部的人使用，如微信公众平台。

同时，好看的界面并没有那么重要。老大一再和我强调：在进行产品设计的时候，要将注意力控制在业务流程上，至于界面能否好看，会有比我更为专业的人进行处理。如果业务流程不顺畅，逻辑不清晰，那么再好看的界面也是坑用户。

## 思维的切入点
梳理业务逻辑根据思考的切入点，也有不同的方法。
第一种：罗列法。根据后台的业务需求，或者功能模块以结构图的形式罗列出来。如下图：

![有帮助的截图](/assets/解构法.png)
（图来源于知乎）

这样做的好处是，整体的结构会非常清晰，可以看到哪一个模块包含了多少个模块，以及它们的从属关系。但缺点也非常明显：每个模块间毫无联系，根本就没有将人的使用流程考虑进去。这种简单粗暴的办法不太适合复杂的后台。

第二种：流程法。与结构法的简单粗暴不同，流程法不以“功能”为核心考虑，而是以“流程”为主。即是以“事件“为线索来描述功能。如下图：

![Alt text](/assets/流程法.png)
（图来源于知乎）
我们可以看到图中将酒店的预定系统根据［申请］，［预定］等不同的事件作为连接线索。这种做法的好处是扩大了思考的维度，将人的操作路径与业务流程紧密地结合起来，以此设计出的后台才是高可用性，并且完整的。而坏处，我想是结构图的表达和组成更为复杂，需要多张图示说明问题。

举设计中一个小小的实例：
我们的后台是通过RCAB模型来管理使用帐号的。即账号－－角色－－权限三者间的相互关系，组成一个可用的账号。

如果是基于第一种方法的构建，我们很容易将后台账号模块分解为三个小模块：账号管理，角色管理，权限管理。每个模块间均有管理，与新建。可每个模块间的并没有连接起来。也就意味着，用户如果打算给一个账号新增权限，就必须先在权限模块新建一个权限，再跑到账号模块中将权限绑定。而这其中就得跳转多个界面了。

如果是基于第二种方法去构建，我们很快就能找到解决方案。只需要将用户使用后台账号各个业务流程列举出来，我们就能发现，只要在新建账号权限的下一步增加多一个绑定账号的选择框，就能解决跳转多个界面的问题。

以上仅仅是一个细小的细节变化，但我们可以通过此得知：使用流程法去构建功能，会让我们考虑到更多的情况。也让我们设计出来的产品更符合实际的操作习惯。更多关于这种方法的说明，可以查看[此条链接](http://www.zhihu.com/question/31859504)，相信看完会有很多的启发。其中文中提到的“穷尽不重复”的思维办法，也是一个很有趣的东西。在以后我会尝试说明下。

“流程法”同样适用于其他产品的设计，个人认为这是有效提高用户体验，以及分析用户使用场景时的法宝。从本质上而言，流程法更像是站在产品全局的角度考虑问题，将功能回归成为一段段用户的行为路径。

当然，这种方法并不在任何地方都适用。有时仅仅考虑流程，可能会导致功能上的重复，或者逻辑不清晰。

## 逻辑与线性的PK

在一篇疑似产品鸡汤的“[微信产品原则](http://www.360doc.com/content/15/0817/16/20720847_492329626.shtml)”一文中称“在逻辑原理和线性原则相冲突的时候，优先线性原则”。我的理解是，逻辑原理就是人意识中的分类：这是模块A，这是模块B，它们之间是并行，从属等关系。而线性原则则是人在实际操作中的使用习惯：我要达到目标C，必须使用模块A然后再模块B；或者，我是角色C，我可以使用模块A和模块C，而其他人不可以。可以笼统地认为，“线性原则”是以结果为导向的。不违背线性，就是尽可能能地不打断用户的操作。

在实际工作中，我也遇上了这样的问题：我们要让一条客户的申请记录进行两次审核，而这两个角色都是在同一个面板中进行操作。

从上述内容我们可以得知：有两种使用角色。因此对应的客户申请记录的生命周期就变为：

![Alt text](/assets/用大于看_-_马克飞象_-_专为印象笔记打造的Markdown编辑器.png)

在设计的时候，我根据角色的使用流程将表单进行了分层。所有的审核纪录被归为［初审］［复审］两种状态。初审中有［需要初审的申请］［被驳回的申请］，而复审中只有［复审的申请］。客服只看他需要处理的审核，而经理只看他需要处理的审核。我也懒得找回原形截图了（喂！），姑且使用文字描述下。

经理在进入后默认选中［复审］一栏。这里只出现他需要审核的表单。但他需要查看手下人正在审核的表单时，可以点击［初审］进入表单，但不能审核。而客服进入此功能时，默认选中［初审］一栏。而无法查看［复审］一栏。
因为对于经理而言，唯一需要care的就只有需要他复审的申请记录，而不需要关注审核的生命周期。相反，客服就需要看到驳回的申请（无论是初审驳回或者复审驳回的申请），因为他需要向申请人解释为什么申请不通过。

简而言之：就是相关人只关注他相关的事。这样的设计能保证用户在操作过程中更专注于他的任务，不容易被打断，尊崇线性原则。

但是在老大看过之后，觉得操作逻辑有些紊乱，因此他建议以审核记录的生命周期为基础，在同一张表单中去完成操作。也就是所有的审核记录按［初审］［复审］［审核不通过］的分类方式，无论什么样的角色进入后都可以自由查看。而这样的设计，也让表单看起来更有逻辑性，功能一目了然，也符合大部分用户目前的操作认知。

逻辑原则与线性原则冲突在产品设计中经常也会遇见，也是目前一直困惑我的一点，希望通过以后的工作可以获得更多的见解。