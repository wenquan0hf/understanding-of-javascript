#深入理解J avaScript 系列（结局篇）

##介绍

最近几个月忙得实在是不可开交，终于把《深入理解 JavaScript 系列》的最后两篇“补全”了，所谓的全是不准确的，因为很多内容都没有写呢，比如高性能、Ajax 安全、DOM 详解、JavaScript 架构等等。但因为经历所限，加上大叔希望接下来写点其它东西，所以此篇文字就暂且当前完结篇的总结吧，以后有时间的话，可以继续加上一些未涉及的专题内容。

##网络文章来源

本系列文章参考了大量的互联网网站，在此向各位网站拥有者、博主、提到的以及未提到的作者们说一声：多谢感谢了。

本系列文章主要参考了如下站点：

五大原则：http://freshbrewedcode.com/derekgreer

ECMAScript262 系列：http://dmitrysoshnikov.com/

DOM 系列文章：http://net.tutsplus.com

设计模式系列文章参考如下三个网站：

http://www.addyosmani.com/resources/essentialjsdesignpatterns/book/

http://shichuan.github.com/javascript-patterns/

https://github.com/tcorral/Design-Patterns-in-Javascript/

其它文章，总结自自己的收藏、心得，结合了互联网上的各位大牛的博客总结整理而成，因为参考地址太多，无法一一列出，如果忘记了各位各种的版权声明，请及时告知，以便及时处理，多谢！

##参考书籍

这里列出的书籍是大叔曾经读过的，也是在整理博文的时候经常参考的书籍，推荐给大家阅读。

###初级读物：

《JavaScript 高级程序设计》：一本非常完整的经典入门书籍，被誉为 JavaScript 圣经之一，详解的非常详细，最新版第三版已经发布了，建议购买。

###中级读物：

《JavaScript 权威指南》：另外一本 JavaScript 圣经，讲解的也非常详细，属于中级读物，建议购买。   

《JavaScript.The.Good.Parts》：Yahoo 大牛，JavaScript 精神领袖 Douglas Crockford 的大作，虽然才 100 多页，但是字字珠玑啊！强烈建议阅读。  

《高性能 JavaScript》：《JavaScript 高级程序设计》作者 Nicholas C. Zakas 的又一大作。  

《Eloquent JavaScript》：这本书才 200 多页，非常短小，但是改变了我写作的习惯，本书通过几个非常经典的例子（艾米丽姨妈的猫、悲惨的隐士、模拟生态圈、推箱子游戏等等）来介绍 JavaScript 方方面面的知识和应用方法，非常值得一读，同时这本书的中文版也是大叔翻译的，点击屏幕右上角可以订购，希望大家多多支持。

###高级读物：

《JavaScript Patterns 》：书中介绍到了各种经典的模式，如构造函数、单例、工厂等等，值得学习。 
 
《Pro.JavaScript.Design.Patterns》：Apress 出版社讲解 JavaScript 设计模式的书，非常不错。  

《Developing JavaScript Web Applications》：构建富应用的好书，针对 MVC 模式有较为深入的讲解，同时也对一些流程的库进行了讲解。  

《Developing Large Web Applications》：将这本书归结在这里，貌似有点不妥，因为这里不仅有 JavaScript 方面的介绍，还有 CSS、HTML 方面的介绍，但是介绍的内容却都非常不错，真正考虑到了一个大型的 Web 程序下，如何进行 JavaScript 架构设计，值得一读。

###其它参考书籍：

《大话设计模式》：博文里关于设计模式的文章，有些总结性的文字来自于此。

《设计模式——可复用面向对象软件的基础》：博文里关于设计模式的文章，有些介绍性和总结性的文章来自于此。

##总结

在写此系列文章期间，大叔也学到了很多很多内容。同时为了不误人子弟，大叔参考了很多很多文章，同时也阅读了那么多书籍，但博客里的文章，可能依然有很多错误，希望各位如果发现错误的话，请及时告知，以便及时修正而不再继续误导其它人。

同时，大家在阅读过程中，有任何问题都可以在相应的文章里留言，大叔将在不耽误工作的情况下尽力回复。

##同步与推荐

深入理解 JavaScript 系列文章，包括了原创，翻译，转载等各类型的文章，如果对你有用，请推荐支持一把，给大叔写作的动力。