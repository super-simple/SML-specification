# SML-specification

simple markup language specification  
简单标记语言规范

## 什么是sml

一个xml的变种,表达上和xml没有任何区别

## 为什么产生sml

xml的用途:

1. 人们需要标记语言,它的确有存在的价值,可以用来写文档,html就是依据xml,尤其是xhtml完全保留了xml的特性,语法非常规范
2. 可以用来当作配置文件,我知道一些项目,都是使用xml作为配置文件
3. xml可以用来传输数据,很多语言都实现了xml的库,来方便大家使用xml

但是,xml我个人认为有以下缺点:

1. 命名空间真的是很让人费解的东西
2. xml的语法很笨重,手写起来不方便

针对以上这些问题,我写了sml,sml针对以上这些问题,做了简化,但是sml从表达上完全是和xml等价的,所以,完全可以代替xml使用

## 内容

本规范,包含以下三个部分

1. 语法规范,使用antlr的g4语法描述,并且针对细节用语言补充
2. 语言实现规范,编程语言实现的时候,遵守的规范
3. 编辑器实现规范,文本编辑器以及ide实现sml语法支持,遵守的规范

## 概览

大致了解一下sml的语法

```xml
<books>
    <!-- 这是注释 -->
    <book number="123" date="2021-12-31" desription="just a 'book'">
        <name>how to get a new friend</name>
        <name>&nbsp;&nbsp;this can contain whitespace&nbsp;&nbsp;</name>
        <name/>
        <name/>
        <name>double quote and single quote &quot;&apos;</name>
    </book>
</books>
```

```sml
books {
//这是注释
    book (author = "123" date="2021-12-31" desription='just a "book"') {
        name{how to get a new friend}
        name{`  this can contain whitespace  `}
        name{}
        name()
        name{double quote and single quote "'}
    }
    /*
        这也是注释
    */
}
```

## 特点

使用c系的语法和注释风格

1. 支持unicode,使用"\uab12",使用'\'作为转义字符,非属性内容中,只有转义'{','}','(',')',属性内容中,按需转义单引号,或者双引号,支持"\n"等转义字符
2. 单行注视和多行注释"//","/**/"
3. 支持简写,node{},node(),node(){}均可,但是推荐node{},如果有属性的时候,可以使用node(foo="bar"),剩下两种不推荐
4. 如果非属性的值开头或者结尾包含空白字符,可以使用'`'  '`'来包含,也可以使用\s来包含,如`  I  can contain whtiespace  `,前面这段字符开头和结尾各自包含了两个空格

## 三种模式

sml根据用途,划分成三种模式

### 文档模式

写文档使用,sml所有的语法都支持

```sml
document(foo="bar" bar="foo"){
    hello document
    body{
        this is body
        h1{
            hello document
        }
    }
}
```

### 配置模式

用作配置文件,节点只能全部包含值或者只能全部包含节点,不能既包含节点又包含值

```sml
document(foo="bar" bar="foo"){
    //body 只包含节点
    body{
        //h1 只包含值
        h1{
            hello document
        }
        h2{
            hello document
        }
    }
}
```

### 数据模式

数据传输,不支持属性,节点只能全部包含值或者只能全部包含节点,不能既包含节点又包含值,这个模式可以完全等价与json

```sml
//没有任何节点包含属性
document{
    //body 只包含节点
    body{
        //h1 只包含值
        h1{hello document}
        h2{hello document}
    }
}
```