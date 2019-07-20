# Grid 布局

[Grid布局](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

- Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是二维布局。

- display: grid; /display: inline-grid; #指定一个容器采用网格布局。# 容器元素都是块级元素

- grid-template-columns (grid-template-columns: 100px 100px 100px;) 属性定义每一列的列宽
- grid-template-rows (grid-template-rows: 100px 100px 100px;) 属性定义每一行的行高

```javascript
--1
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
--2
.container {
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}

```

# repeat()

- 有时候，重复写同样的值非常麻烦，尤其网格很多时。这时，可以使用 repeat()函数，简化重复的值。

```javascript
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}

```

# auto-fill 关键字

- 有时，单元格的大小是固定的，但是容器的大小不确定. 如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用 auto-fill 关键字表示自动填充。

```javascript

.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}

```

# fr 关键字

- 1 为了方便表示比例关系，网格布局提供了 fr 关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为 1fr 和 2fr，就表示后者是前者的两倍

```javascript
-1
.container {
  display: grid;
  grid-template-columns: 1fr 1fr;
}
-2
.container {
  display: grid;
  grid-template-columns: 150px 1fr 2fr;
}
```

# minmax()

- minmax()函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```javascript
-1
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

- 上面代码中，minmax(100px, 1fr)表示列宽不小于 100px，不大于 1fr。

# auto 关键字

- auto 关键字表示由浏览器自己决定长度。

```javascript
-1
grid-template-columns: 100px auto 100px;

```

# 网格线的名称

- grid-template-columns 属性和 grid-template-rows 属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。

```javascript

.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}

```

# 布局实例

- grid-template-columns 属性对于网页布局非常有用。两栏式布局只需要一行代码。

```javascript
-1  下面代码将左边栏设为70%，右边栏设为30%。
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}

-2 传统的十二网格布局，写起来也很容易

grid-template-columns: repeat(12, 1fr);

```

- grid-row-gap 属性 设置行与行的间隔（行间距）

- grid-column-gap 属性 设置列与列的间隔（列间距）。

- grid-gap 是 grid-column-gap 和 grid-row-gap 的合并简写形式

```javascript

-1

.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}

-2
.container {
  grid-gap: 20px 20px;
}


```

# grid-template-areas 属性

- 网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。grid-template-areas 属性用于定义区域。

```javascript
-1
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
- 上面代码先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。
- 多个单元格合并成一个区域的写法如下
-2

grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';

grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";

grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';

```

# grid-auto-flow 属性

- 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行

- 顺序由grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"

- grid-auto-flow: column;

- grid-auto-flow: row dense; "先行后列"，并且尽可能紧密填满，尽量不出现空格

- grid-auto-flow: column dense; 表示"先列后行"，并且尽量填满空格

# justify-items 属性，

- justify-items属性设置单元格内容的水平位置（左中右）

# align-items 属性

- align-items属性设置单元格内容的垂直位置（上中下）

```javascript
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
这两个属性的写法完全相同，都可以取下面这些值。

start：对齐单元格的起始边缘。
end：对齐单元格的结束边缘。
center：单元格内部居中。
stretch：拉伸，占满单元格的整个宽度（默认值）。

.container {
  justify-items: start;
}

```

# place-items 属性 

- 属性是align-items属性和justify-items属性的合并简写形式。

```javascript

place-items: <align-items> <justify-items>;

place-items: start end;

```

```javascript
justify-content 属性，
align-content 属性，
place-content 属性


- justify-content属性是整个内容区域在容器里面的水平位置（左中右），align-content属性是整个内容区域的垂直位置（上中下）。


.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}


place-content属性是align-content属性和justify-content属性的合并简写形式。

place-content: <align-content> <justify-content>

```
