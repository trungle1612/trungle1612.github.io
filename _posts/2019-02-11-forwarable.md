---
layout: post
categories: ['ruby', 'rails']
tag: ['ruby, ', 'rails']
title: Forwardable & Delegate
---

1. Giới thiệu
 - Forwardable là một module có thể được sử dụng để thêm những tất cả methods của 1 `instance class`

2. `def_delegator` method
 - Định nghĩa một method như `delegator instance method` với một alias name

2. `def_delegators` method
 - Định nghĩa một mảng method như `delegator instance methods`

4. Ví dụ

```ruby
# people.rb
class People
  attr_reader :first_name, :last_name

  def inititalize(fist_name, last_name)
    @first_name = first_name
    @last_name  = last_name
  end

  def birthday
    '01/01/1900'
  end

  def country
    'Viet Nam'
  end

  def full_name
    @first_name + ' ' + last_name
  end
end
```

```ruby
# student.rb
require 'people'
require 'forwardable'

class Student
  extend Forwardable
  def_delegators @people, :birthday, :country
  def_delegator @people, :full_name, :name

  def inititalize(fist_name, last_name)
    @people = People.new(fist_name, last_name)
  end
end

student = Student.new("Peter", "Pan")
puts student.full_name
puts student.country
```

Result

```
Peter Pan
Viet Nam
```

- Hàm `def_delegator` sẽ delegate lại method và cho phép bạn đặt tên mới từ một instance class(People)
  - `name` là 1 alias của `full_name`
- Hàm `def_delegators` sẽ delegate lại  một mảng methods từ một instance class(People)
  - Giống như hàm `def_delegator` nhưng hàm này không cho phép tạo alias, hàm này có thể delegate 1 mảng methods của instance class
