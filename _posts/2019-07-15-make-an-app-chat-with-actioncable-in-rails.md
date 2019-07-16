---
layout: post
title: Make an app chat with ActionCable in rails
date: 2019-07-15 09:59 +0700
---

## Flow
![actioncable_flow](/images/actioncable/action_cable.png "actioncable_flow")

## Các components trong actioncable

1. Pub/Sub(Publish - Subscribe)
  - Đây là 1 cơ chế của redis  để Publishers gửi một thông điệp (message) đến người nhận(subscribers) mà không biết gì thông tin về người nhân, tương tự người nhận cũng sẽ không biết về thông tin người gởi.
  -  => Trong actioncable rails:
  - Client sẽ subscribe một hoặc nhiều channels, server gởi thông điệp (message) đến những channel này.

2. Connection
    - Đơn giản là tạo kết nối giữa client và server 

3. Channels
    - Dễ hiểu thì channel là nơi nhân hoặc gởi data, ví dụ khi bạn chat qua facebook với người khác thì đó là 1 channel, 
    dĩ nhiên bạn có thể với nhiều người cùng lúc nên bạn có thể tạo nhiều channel. => Kênh để bạn trao đổi thông tin.

4. Subscriptions
    - Khi bạn đăng kí đến 1 channel thì bạn xem như là 1 `subscription`.

5. Streams
    - Nơi trung chuyển messages/data đến những channel mà bạn đã subscribe.

6. Broadcasting
    - Là nơi đưa các messages/data đến điểm trung chuyển (streams) bởi publisher
