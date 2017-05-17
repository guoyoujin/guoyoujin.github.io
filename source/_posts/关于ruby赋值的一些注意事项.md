title: 关于ruby赋值的一些注意事项
author: TryCatch
tags:
  - ruby
categories:
  - ruby
date: 2017-05-17 14:37:00
---
看下面代码:
```ruby
params = {a:"1",b:"2",c:"3"}
 => {:a=>"1", :b=>"2", :c=>"3"} 
pa = params
 => {:a=>"1", :b=>"2", :c=>"3"} 
pa.delete(:a)
 => "1" 
params
 => {:b=>"2", :c=>"3"} 
pa
 => {:b=>"2", :c=>"3"}  

```
发现了吗？我执行```pa.delete(:a)```之后，居然连params也变了。我也是醉了，这个问题坑了我一次，大家谨记千万别这么写了，你可以像下面这么写
```ruby
params = {a:"1",b:"2",c:"3"}
 => {:a=>"1", :b=>"2", :c=>"3"} 
pa = params.clone
 => {:a=>"1", :b=>"2", :c=>"3"} 
pa.delete(:a)
 => "1" 
pa
 => {:b=>"2", :c=>"3"} 
params
 => {:a=>"1", :b=>"2", :c=>"3"} 
```
对没错就是上面的clone方法。当然你也可以使用ruby的深拷贝

让我们在看下方法里面传参数的使用
```ruby
params = {a:"1",b:"2",c:"3"}
 => {:a=>"1", :b=>"2", :c=>"3"} 
def format_params pa=nil
   pa.delete(:a)
end
 => :format_params 
format_params(params)
 => "1" 
params
 => {:b=>"2", :c=>"3"} 
```
我擦哦，方法穿参数居然也是这么坑，有木有，所以大家自己注意点
再来看数组
```ruby
params = [1,2,3,4,5]
 => [1, 2, 3, 4, 5] 
def format_params pa=nil
   pa[0]=100
end
 => :format_params 
format_params(params)
 => 100 
params
 => [100, 2, 3, 4, 5] 
```
数组也会这样啊，醉了

不过大家别害怕，这些问题只是存在于一些hash ，json，array数据才会出现的，下面看代码
```ruby
a = 100
 => 100 
def format_a p=nil
   p = 200
end
 => :format_a 
 format_a(a)
 => 200 
2.2.5 :042 > a
 => 100 
```
这个还好不会变哈