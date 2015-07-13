# 根本没有“JSON 对象”这回事！ 

## 前言 

写这篇文章的目的是经常看到开发人员说：把字符串转化为 JSON 对象，把 JSON 对象转化成字符串等类似的话题，所以把之前收藏的一篇老外的文章整理翻译了一下，供大家讨论，如有错误，请大家指出，多谢。

## 正文

本文的主题是基于 ECMAScript262-3 来写的，2011 年的 262-5 新规范增加了 JSON 对象，和我们平时所说的 JSON 有关系，但是不是同一个东西，文章最后一节会讲到新增加的 JSON 对象。

我想给大家澄清一下一个非常普遍的误解，我认为很多 JavaScript 开发人员都错误地把 JavaScript `对象字面量`（Object Literals）称为 `JSON 对象`（JSON Objects），因为他的语法和[JSON规范](http://json.org/)里描述的一样，但是该规范里也明确地说了 JSON 只是一个`数据交换语言`，只有我们将之用在 string 上下文的时候它才叫 JSON。

## 序列化与反序列化 

2 个程序（或服务器、语言等）需要交互通信的时候，他们倾向于使用 string 字符串因为 string 在很多语言里解析的方式都差不多。复杂的数据结构经常需要用到，并且通过各种各样的中括号{}，小括号()，叫括号`<>`和空格来组成，这个字符串仅仅是按照要求规范好的字符。

为此，我们为了描述这些复杂的数据结构作为一个 string 字符串，制定了标准的规则和语法。JSON 只是其中一种语法，它可以在 string 上下文里描述对象，数组，字符串，数字，布尔型和 null，然后通过程序间传输，并且反序列化成所需要的格式。[YAML](http://en.wikipedia.org/wiki/YAML) 和[ XML](https://en.wikipedia.org/wiki/XML)（甚至 [request params](http://benalman.com/news/2009/12/jquery-14-param-demystified/)）也是流行的数据交换格式，但是，我们喜欢 JSON，谁叫我们是 JavaScript 开发人员呢！

## 字面量

引用 Mozilla Developer Center 里的几句话，供大家参考：

1. 他们是固定的值，不是变量，让你从“字面上”理解脚本。 ([Literals](https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Core_Language_Features#Literals))
2. 字符串字面量是由双引号（"）或单引号（'）包围起来的零个或多个字符组成的。([Strings Literals](https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Core_Language_Features#String_Literals))
3. 对象字面量是由大括号（{}）括起来的零个或多个对象的属性名-值对。([Object Literals](https://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Core_Language_Features#Object_Literalshttps://developer.mozilla.org/en/Core_JavaScript_1.5_Guide/Core_Language_Features#Object_Literals))

## 何时是 JSON，何时不是 JSON？

JSON 是设计成描述数据交换格式的，他也有自己的语法，这个语法是 JavaScript 的一个子集。
{ "prop": "val" } 这样的声明有可能是 JavaScript 对象字面量也有可能是 JSON 字符串，取决于什么上下文使用它，如果是用在 string 上下文（用单引号或双引号引住，或者从 text 文件读取）的话，那它就是 JSON 字符串，如果是用在对象字面量上下文中，那它就是对象字面量。

```
// 这是JSON字符串
var foo = '{ "prop": "val" }';
```
 
```
// 这是对象字面量
var bar = { "prop": "val" };
```

而且要注意，JSON 有非常严格的语法，在 string 上下文里{ "prop": "val" } 是个合法的 JSON，但{ prop: "val" }和{ 'prop': 'val' }却是不合法的。所有属性名称和它的值都必须用双引号引住，不能使用单引号。另外，即便你用了转义以后的单引号也是不合法的，详细的语法规则可以到[这里查看](http://json.org/)。

## 放到上下文里来看

大家伙可能嗤之以鼻：难道 JavaScript 代码不是一个大的字符串？

当然是，所有的 JavaScrip t代码和 HTML（可能还有其他东西）都是字符串，直到浏览器对他们进行解析。这时候 .jf 文件或者 inline 的 JavaScript 代码已经不是字符串了，而是被当成真正的 JavaScript 源代码了，就像页面里的 innterHTML 一样，这时候也不是字符串了，而是被解析成 DOM 结构了。

再次说一下，这取决于上下文，在 string 上下文里使用带有大括号的 JavaScript 对象，那它就是 JSON 字符串，而如果在对象字面量上下文里使用的话，那它就是对象字面量。

## 真正的 JSON 对象 

开头已经提到，对象字面量不是 JSON 对象，但是有[真正的 JSON 对象](https://developer.mozilla.org/en/Using_native_JSON)。但是两者完全不一样概念，在新版的浏览器里 JSON 对象已经被原生的内置对象了，目前有 2 个静态方法：`JSON.parse` 用来将 JSON 字符串反序列化成对象，`JSON.stringify` 用来将对象序列化成 JSON 字符串。老版本的浏览器不支持这个对象，但你可以通过 [json2.js](http://json.org/) 来实现同样的功能。

如果还不理解，别担心，参考一下的例子就知道了：

```
// 这是JSON字符串，比如从AJAX获取字符串信息
var my_json_string = '{ "prop": "val" }';
 ```

```
// 将字符串反序列化成对象
var my_obj = JSON.parse( my_json_string );
alert( my_obj.prop == 'val' ); //  提示 true, 和想象的一样!
 ```

```
// 将对象序列化成JSON字符串
var my_other_json_string = JSON.stringify( my_obj );
```

另外，[Paul Irish](http://paulirish.com/)提到 Douglas Crockford 在 [JSON RFC ](http://www.ietf.org/rfc/rfc4627.txt?number=4627)里用到了“JSON object”，但是在那个上下文里，他的意思是“对象描述成 JSON 字符串”不是“对象字面量”。

## 更多资料

如果你想了解更多关于 JSON 的资料，下面的连接对你绝对有用：

- [JSON specification](http://json.org/)
- [JSON RFC](http://www.ietf.org/rfc/rfc4627.txt?number=4627)
- [JSON on Wikipedia](http://en.wikipedia.org/wiki/JSON)
- [JSONLint - The JSON Validator](http://www.jsonlint.com/)
- [JSON is not the same as JSON](http://james.padolsey.com/javascript/json-is-not-the-same-as-json/)
