---
layout: post
title:  "Effective Ruby - 哈希"
date:   2016-04-27
categories: Ruby
---

#### Ruby基础
[Ruby基础](http://www.cnblogs.com/lizunicon/p/3721085.html)

#### 第20条：考虑使用默认哈希值
第一种情况

```
2.2.1 :001 > h = Hash.new([]) //使用一个空数组作为默认值
 => {}
2.2.1 :002 > h
 => {}
2.2.1 :003 > h[:missing_key]
 => []
2.2.1 :004 > h
 => {}
2.2.1 :005 > h[:missing_key] << "Hey there!"
 => ["Hey there!"]
2.2.1 :006 > h.keys
 => []
2.2.1 :007 > h
 => {}
2.2.1 :008 > h[:missing_key]
 => ["Hey there!"]
```

第二种情况

```
2.2.1 :083 >   h = Hash.new{ [] } //[]作为return返回值
 => {}
2.2.1 :084 > h[:a] = h[:a] << "a"
 => ["a"]
2.2.1 :085 > h[:b]
 => []
2.2.1 :086 > h.keys
 => [:a]
```

第三种情况

```
2.2.1 :113 >   h = Hash.new{ |hash, key| hash[key] = [] }  //每次都会执行
 => {}
2.2.1 :114 > h[:a] << "a"
 => ["a"]
2.2.1 :115 > h[:b]
 => []
2.2.1 :116 > h.keys
 => [:a, :b]
2.2.1 :117 > h
 => {:a=>["a"], :b=>[]}
2.2.1 :118 >
```

fetch方法

```
2.2.1 :161 >   h = {}
 => {}
2.2.1 :162 > h[:a] = h.fetch(:a, []) << "a"
 => ["a"]
2.2.1 :163 > h.fetch(:a, [])
 => ["a"]
2.2.1 :164 > h.fetch(:b)
KeyError: key not found: :b
	from (irb):164:in `fetch'
	from (irb):164
	from /Users/Wj/.rvm/rubies/ruby-2.2.1/bin/irb:11:in `<main>'
```

小科普：为什么热水比冷水结冰快。(姆佩巴效应是一个谜)














