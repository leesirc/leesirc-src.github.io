+++
title="子网掩码"
tags=["GO浪第三方库"]
categories=["GO浪第三方库"]
date="2020-10-11T12:00:00+08:00"
toc=true
+++

<!--摘要 -->
GO浪第三方库: github.com/fatih/color
<!--more-->


# 序
今天在看TIUP的时候, 发现了这个玩意, 简单记录下, 不喜勿喷哦

它通过ANSI转译码输出相关彩色文字

# 安装

```shell
go get github.com/fatih/color

```

# 分析

```go
package main

import (
	"github.com/fatih/color"
	"fmt"
)

func main()  {
	r := "red"
	fmt.Println(color.RedString("this is %v", r))
	fmt.Println(color.RedString("this is red"))
}

```

从代码里我们不难看出 RedString 函数如下:
```go
type Attribute int
// ...

// Foreground text colors
const (
	FgBlack Attribute = iota + 30
	FgRed
	FgGreen
	FgYellow
	FgBlue
	FgMagenta
	FgCyan
	FgWhite
)
// ...

// RedString is a convenient helper function to return a string with red
// foreground.
func RedString(format string, a ...interface{}) string { return colorString(format, FgRed, a...) }

```

从示例中可以看出来, color.FgRed为一个常量31, 且类型为color.Attribute, 即int的自定义类型
colorString 函数如下:
```go



var (
    colorsCache   = make(map[Attribute]*Color)
    colorsCacheMu sync.Mutex // protects colorsCache
)

type Color struct {
	params  []Attribute
	noColor *bool
}

func (c *Color) Add(value ...Attribute) *Color {
	c.params = append(c.params, value...)
	return c
}

func New(value ...Attribute) *Color {
	c := &Color{params: make([]Attribute, 0)}
	c.Add(value...)
	return c
}

func getCachedColor(p Attribute) *Color {
	colorsCacheMu.Lock()
	defer colorsCacheMu.Unlock()

	c, ok := colorsCache[p]
	if !ok {
		c = New(p)
		colorsCache[p] = c
	}

	return c
}

func colorString(format string, p Attribute, a ...interface{}) string {
	c := getCachedColor(p)

	if len(a) == 0 {
		return c.SprintFunc()(format)
	}

	return c.SprintfFunc()(format, a...)
}
```

从 color.colorString 函数中可以得知, 在 getCachedColor函数中: 
1. 通过 colorsCacheMu变量作了一个互斥锁, 此时并发读写堵塞;
2. 通过colorsCache 全局map, map是引用类型, key为 color.Attribute类型, value为 color.Color指针, 若不存在key为对应颜色的map, 就新建一个color.Color结构体指针
3. 新建的Color结构体指针中, params成员是一个包含对应颜色的切片
4. 将生成的新 Color结构体指针存储到全局map里 colorCache

```go
const escape = "\x1b"
const (
	Reset Attribute = iota
	Bold
	Faint
	Italic
	Underline
	BlinkSlow
	BlinkRapid
	ReverseVideo
	Concealed
	CrossedOut
)

func (c *Color) sequence() string {
	format := make([]string, len(c.params))
	for i, v := range c.params {
		format[i] = strconv.Itoa(int(v))
	}

	return strings.Join(format, ";")
}

func (c *Color) format() string {
	return fmt.Sprintf("%s[%sm", escape, c.sequence())
}

func (c *Color) unformat() string {
	return fmt.Sprintf("%s[%dm", escape, Reset)
}

func (c *Color) wrap(s string) string {
	if c.isNoColorSet() {
		return s
	}

	return c.format() + s + c.unformat()
}

func (c *Color) SprintFunc() func(a ...interface{}) string {
	return func(a ...interface{}) string {
		return c.wrap(fmt.Sprint(a...))
	}
}

func (c *Color) SprintfFunc() func(format string, a ...interface{}) string {
	return func(format string, a ...interface{}) string {
		return c.wrap(fmt.Sprintf(format, a...))
	}
}
```

从 SprintfFunc 方法中, 我们可以看出:
1. 这个应该是一个闭包, 不过这个是一个特殊的闭包
2. Redstring 最终拿到的字符串为: \x1b[31mxxxxxxx\x1b[0m

# 总结

```shell
1.闭包的用法
2.map是引用类型
3.切片转string: strings.join(slice,";")
4.int转string: strconv.itoa
```

## 简单使用

```go
package main

import (
	"github.com/fatih/color"
	"fmt"
)

func main()  {
	r := "red"
	fmt.Println(color.RedString("this is %v", r))
	fmt.Println(color.RedString("this is red"))
}

```

