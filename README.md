Templet
=======

Templet是一个小巧灵活的javascript前端模版引擎，主要借鉴了mustache的语法以及
django模板中的filter思想而形成

#为什么要写这个模版引擎

我初次接触前端开发的时候，有大部分工作需要拼接HTML然后通过.innerHTML属性赋值从而
达到界面填充的任务，当时代码中大概有几种方法来完成这样的工作，第一种是在javascript 
代码中拼接html片段，第二种是在html里面声明一种模版，然后通过一种模版填充的方法来
把数据填充到模版里面去。

当然第一种方法肯定是需要被废弃的，在经典的web架构中，UI与逻辑是尽量需要分开的。
第二种方法是比较好的方法，但是我们当时的那个模版填充的方法总觉得不是那么直接，中
间感觉多了几层思维上面的转换，导致每次用的人都需要先去看那个方法，而且也是很久都
看不懂，每次需要填充的时候都非常纠结。在那之后，发现一个比较好的模版填充函数，类
似与c语言中的printf，只不过用名字来标识需要填充的字段，模板大概长这样子

	"This is a tempalte and {property} will be filled by property of data obj"

对于一些比较简短的字符串拼接，这是一个好方法，但是如果模版层级比较多，又需要循环
的情况，这个方法就不胜任了，当时由于没有找到好的方法，大部分都是遇到循环的时候就
直接写循环来实现，当然这种代码多了之后就会觉得基本上是重复的机械劳动。

一次偶然的机会，认识到了[mustache]，一下子被它的介绍给吸引住了:"Logic-Less
Template"，它的设计原则就是无逻辑的模版，这针对的当然是大多数后端模版引擎，比如
velocity，django自带模版，jsp里面的tag和php等等，因为这些模版大部分都有一套自己
的语法，然后可以在模版里面写if和else等语句，我自己之前也接触过jsp以及django等模版，
个人感觉这种模版和逻辑混合写的模式觉得特别的不舒服，也不知道哪里不舒服，就感觉把
模版已经逻辑糅合到一起了，维护以及修改起来会比较麻烦。而[mustache]这种模式呢个人
觉得有如下优点：模版自身语义较少，比较易学；模版自身逻辑比较少，出错的时候比较容易
debug；模版中比较少语法的标签，模版就比较容易维护以及修改。

可能有人会问，为什么还要自己弄一个模版引擎，而不直接用[mustache]呢，因为我们的业
务为了做一些统计，需要知道数组循环的时候的数据索引，还有一些简单的条件现实的子模
版[mustache]也没有支持，第三个就是不大能理解mustache里面的函数的支持感觉不大好理
解，我们这里就采用了filter模式。最后我觉得不采用mustache的原因是因为mustache的主
要目的是为了跨语言的模版，所以导致有些地方做的不够灵活，而Templet主要是针对javascript
的前端渲染的模版引擎，实现起来更加灵活简洁一点。

#支持的特性

1.	子模版，子模版与数据对象对应的属性进行渲染
	
	"hello {#sec} template will be rendered use properties object with name "sec" {/sec}"

2.	属性填充，属性值填充到属性tag的位置，其中$表示是当前元素，而不去找对象的属性
	
	"This is a tempalte and {property} will be filled by data obj"

3.	子模版数组循环渲染，如果子模版对于的对象是一个数组的话，就用数组中每个元素对
	子模版进行循环渲染

	"hello {#seclist} if the seclist object is a array, this will be repeated
rendered and also have an inner properties named @idx to represent the index of
the current item in the seclist data {/seclist} abc"

4.	属性过滤器，对于属性填充，支持与属性平级的对象中的方法以及全局的方法来对属性
	进行过滤，过滤器一般都是接受一个字符串，返回另外一个字符串的方法

5.	数组index支持，使用@idx内建属性表示数组中的第几个

6.	支持条件渲染模块，比如{?$.abc==1} {/?}这种模式，$表示当前对象。

[mustache]:https://github.com/janl/mustache.js
