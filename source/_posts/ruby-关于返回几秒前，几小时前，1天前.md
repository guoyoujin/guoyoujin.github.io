title: ruby 关于返回几秒前，几小时前，1天前......
author: TryCatch
tags:
  - ruby
categories:
  - ruby
date: 2017-05-17 14:33:00
---
这个需求是： 
当小于60秒的时候返回时间为xx秒前； 
当小于60分钟大于60秒的时候返回xxx小时前； 
当隔1天的时候显示一天前； 
当大于隔2天的时候，显示xx月xx日； 
当跨年的时候显示xxxx年xx月xx日；

下面是实现代码，具体的话你可以按照你的需求进行修改
```
module TimeFormat

  def self.time_text new_time=Time.now,old_time=Time.now
    if new_time.year == old_time.year
      hour_subtract = new_time.to_i/3600 - old_time.to_i/3600
      min_subtract = new_time.to_i/60 - old_time.to_i/60
      sec_subtract = new_time.to_i - old_time.to_i
      if new_time.at_beginning_of_day == old_time.at_beginning_of_day
        if min_subtract<60
          if sec_subtract<60
            "#{sec_subtract}秒前"
          else
            "#{min_subtract}分钟前"
          end
        else
          "#{hour_subtract}小时前"
        end
      else
        if (old_time.at_beginning_of_day+1.day) == new_time.at_beginning_of_day
          "1天前"
        else
          old_time.strftime("%m月%d日")
        end
      end
    else
      old_time.strftime("%Y年%m月%d日")
    end
  end
end
```