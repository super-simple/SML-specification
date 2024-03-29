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

## 内容

本规范,包含以下三个部分

1. 语法规范,使用自然语言描述
2. 语言实现规范,编程语言实现的时候,遵守的规范
3. 编辑器实现规范,文本编辑器以及ide实现sml语法支持,遵守的规范

## 概览

大致了解一下sml的语法

```xml
<?xml version="1.0"?>
<?xml-stylesheet href="catalog.xsl" type="text/xsl"?>
<catalog>
    <product description="Cardigan Sweater" product_image="cardigan.jpg">
        <catalog_item gender="Men's">
            <item_number>QWZ5671</item_number>
            <price>39.95</price>
            <size description="Medium">
                <color_swatch image="red_cardigan.jpg">Red</color_swatch>
                <color_swatch image="burgundy_cardigan.jpg">Burgundy</color_swatch>
            </size>
            <size description="Large">
                <color_swatch image="red_cardigan.jpg">Red</color_swatch>
                <color_swatch image="burgundy_cardigan.jpg">Burgundy</color_swatch>
            </size>
        </catalog_item>
        <catalog_item gender="Women's">
            <item_number>RRX9856</item_number>
            <price>42.50</price>
            <size description="Small">
                <color_swatch image="red_cardigan.jpg">Red</color_swatch>
                <color_swatch image="navy_cardigan.jpg">Navy</color_swatch>
            </size>
            <size description="Medium">
                <color_swatch image="red_cardigan.jpg">Red</color_swatch>
                <color_swatch image="navy_cardigan.jpg">Navy</color_swatch>
            </size>
        </catalog_item>
    </product>
</catalog>
```

```sml
sml()
catalog[
    product(description="Cardigan Sweater" product_image="cardigan.jpg")[
        catalog_item(gender="Men's"){
            item_number{QWZ5671}
            price{39.95}
            size(description="Medium")[
                color_swatch(image="red_cardigan.jpg"){Red}
                color_swatch(image="burgundy_cardigan.jpg"){Burgundy}
            ]
            size(description="Large")[
                color_swatch(image="red_cardigan.jpg"){Red}
                color_swatch(image="burgundy_cardigan.jpg"){Burgundy}
            ]
        }
        catalog_item(gender="Women's"){
            item_number{RRX9856}
            price{42.50}
            size(description="Small"){
                color_swatch(image="red_cardigan.jpg"){Red}
                color_swatch(image="navy_cardigan.jpg"){Navy}
            }
            size(description="Medium"){
                color_swatch(image="red_cardigan.jpg"){Red}
                color_swatch(image="burgundy_cardigan.jpg"){Burgundy}
            }
        }
    ]
]
```

## 特点

文档

```sml
sml()//和xml保持一直,以后扩展使用
root{
    /*
        多行注释,
    */
    //单行注释
    attribute1[ //表示是个数组
        some(name=111){1}
        some(name="111 222"){2} //属性的值如果包含空格,那么必须用双引号或者单引号包裹起来
        some(name='\"123\"'){3}
    ]
    aatribute2{
        attribute5{
            `this is not node aaa(name='aa'){123}` // ``反引号表示常量,类似CDATA
        }
        attribute6{
            sss\(sss\{sss\)sdsds\\sdsds\\t\\\r\t //转义字符,和json类似
        }
        attribute7{
            \u1234\u1111\u2334 //支持unicode
        }
    }
}
```

使用c系的语法

1. 去除命名空间
2. 支持unicode,使用"\uab12"
3. 使用'\'作为转义字符,可以转义'"\/bfnrt
3. 单行注释和多行注释"//","/**/"
4. 元素名字可以包含空白字符串,这个并不限制,如果包含空白字符串,元素的名字应该使用双引号或者单引号包裹
5. 支持数组[],主要针对数据模式的时候使用,暗示这是一个数组

## 关键字

xml不会限制除了关键字不能被包含进名字中以外,还有空白字符本身不能包含以外,其余的都可以包含

```txt
(
)
{
}
[
]
=
"
'
`
\

/*
*/
//
```

总共14,其中11个是关键字,剩下3个是注释,这11个可以使用\来转义  
sml不会限制你的名字和内容,尽可能的给你宽松,但是考虑到,你的名字需要和编程语言的变量名对应,
所以,你还是尽量使用变量名的规范来命名,不要十分怪异的使用,防止以后不兼容

## 二种模式

sml根据用途,划分成二种模式

### 混合模式 mixed

写文档使用,sml所有的语法都支持

```sml
sml()
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

### 数据模式 data

用作配置文件,节点只能全部包含值或者只能全部包含节点,不能既包含节点又包含值,类似与不支持xml的混合模式

```sml
sml(){} //保留,用于以后扩展
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