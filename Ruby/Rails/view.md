# Controller のメソッドをerb上から呼び出す
## helper_method をつける

```ruby
class ScreeningsController < ApplicationController
  def order_type
    '申込み' 
  end
  helper_method :order_type
end
```
