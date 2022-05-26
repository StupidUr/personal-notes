---
title: '左侧标题' # 左侧标题
sidemenu: true # 左侧菜单
order: 1 # 排序
group:
  title: 分组名称
---

# Linux 基础


## Linux 常见问题

1. SSH出现`Host key verification failed.`
  - 详细描述：
      ```bash
         ECDSA host key for 108.61.163.242 has changed and you have requested strict checking.
         Host key verification failed.
      ```
   - 原因分析：
      一般这个问题，是你重置过你的服务器后。你再次想访问会出现这个问题。
   - 解决步骤： 
     ```
     ssh-keygen -R 你要访问的IP地址
     ```
     
     updated .ssh/known_hosts
2. 
