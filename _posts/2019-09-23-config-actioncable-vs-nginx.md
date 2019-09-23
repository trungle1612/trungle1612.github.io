---
layout: post
title: Config Actioncable vs Nginx
date: 2019-09-23 23:54 +0700
---

Insert a `location` block which configures the Action Cable end point to work as expected
```
location /cable {
  passenger_app_group_name YOUR_APP_NAME_HERE_action_cable;
  passenger_force_max_concurrent_requests_per_process 0;
  }
```
