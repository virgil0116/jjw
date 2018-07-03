---
layout: post
title:  "Rails测试"
date:   2017-03-14
categories: Rails
---

## Rails测试

### 参考
[使用cucumber编写用户故事](http://www.jianshu.com/p/b964d374f1fd)
\| 
[cucumber-rails](https://github.com/cucumber/cucumber-rails)
\|
[RSpec驱动API测试框架-airborne](https://github.com/brooklynDev/airborne)
\| 
[Capybara模拟真实用户交互](https://github.com/teamcapybara/capybara)
\| 
自动化测试工具Selenium WebDriver [和我一起学 Selenium WebDriver（1）——入门篇](https://my.oschina.net/dyhunter/blog/94090)
\| 
[测试匹配插件shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers)
\| 
[Ruby On Rails 集成测试](http://www.cnblogs.com/limx/p/6556239.html)

### Rspec
* [rspec-core]
* [rspec-expectations]
* [rspec-mocks]
* [rspec-rails]

### [RSpec on Rails 101](https://xdite.gitbooks.io/rspec-101/content/)
#### 测试的分类
简单分成“单元测试”、“整合测试”以及“验收测试”。
#### RSpec基础
* describe / it / expect 角色区别
* context 使用方法
* before 使用方法
* let / let! / subject 使用方法
* pending 和 skip 区别

describe “描述”一组测试，用 it 来写一个测试。it内的expect则是用来检查“左值”是否与“右值”相等：expect(“左值”).to eq(“右值”)。所以，describe里可以有多个it，it里可以有多个期望语句。每个测试都是由RSpec.describe描述要测试的类别Class、模块Module。describe可以嵌套使用，比如用来描述类别内的不同的方法。
每个方法内有if-else，不同的情况，可以用context来区分。



### Gem包支持

```
  gem 'factory_girl_rails'
  gem 'factory_girl-seeds'
  gem 'faker'
  #  测试
  gem 'rspec-rails', '~> 3.5'
  gem 'airborne'
  gem 'database_cleaner
```


### 步骤
```
rails g rspec:request api/v1/[name] --request-specs   

```






