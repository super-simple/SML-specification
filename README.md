# SML-specification

simple markup language specification  
简单标记语言规范

## 什么是sml

一个xml的变种,表达效果上和xml没有任何区别,大部分语法沿用c系的语法,鉴于c系语言的流行,sml的语法本身也会让人看起来很熟悉

## 为什么产生sml

xml的用途:

1. 人们需要标记语言,它的确有存在的价值,用来写文档,html就是文档标记语言
2. 可以用来当作为配置文件,很多项目,都是使用xml作为配置文件
3. xml可以用来传输数据,很多语言都实现了xml的库,来方便大家使用xml描述数据

但是,xml我个人认为有以下缺点:

1. 命名空间理解起来不方便,而且并不是用
2. xml的语法很笨重,看起来总显得那么臃肿

针对以上这些问题,我写了sml,sml针对以上这些问题,做了简化,但是sml从表达上完全是和xml等价的,所以,完全可以代替xml使用

## 内容

本规范,包含以下三个部分

1. 语法规范,使用antlr的g4语法描述,并且针对细节用自然语言补充
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
sml(){} //保留,用于以后扩展
books {
//这是注释
    book (author = "123" date="2021-12-31" desription='just a "book"') {
        name{how to get a new friend}
        name{`  this can contain whitespace  `}
        name{} //一个空元素
        name(){} // 仍然是一个空元素
        name{double quote and single quote "'}
    }
    /*
        这也是注释
    */
}
```

## 特点

使用c系的语法

1. 去除命名空间
2. 支持unicode,使用"\uab12",使用'\'作为转义字符,非属性内容中,只有转义'{','}','(',')',属性内容中,按需转义单引号,或者双引号,支持"\n"等转义字符
3. 单行注释和多行注释"//","/**/"
4. 如果非属性的值开头或者结尾包含空白字符,可以使用'`'  '`'来包含,也可以使用\s来包含,如`  I  can contain whtiespace  `,前面这段字符开头和结尾各自包含了两个空格

## 两种种模式

sml根据用途,划分成两种模式

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

### 数据模式

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