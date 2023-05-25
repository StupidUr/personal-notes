---
title: 'Github使用' # 左侧标题
sidemenu: true # 左侧菜单
order: 4 # 排序
---


## 同步Gitee

1. 同步导入Gitee，并手动同步
   > 登陆 Gitee 账号，点击右上角的 + 号，点击「从 GitHub 导入仓库」，在跳转的页面中授权 Gitee 访问。

   ![](https://cdn.jsdelivr.net/gh/stupidur/images@main/notes/20220526170832.png)

   手动同步：
  
   ![](https://cdn.jsdelivr.net/gh/stupidur/images@main/notes/20220526170858.png)

2. Gitee 和 Github 同步更新
   > 如果是本地仓库，只在需要命令行添加用不同名称标识的 Gitee  和 Github 远程库。
  - 修改配置文件`.git/config`

   ![](https://cdn.jsdelivr.net/gh/stupidur/images@main/notes/20220526115915.png)

  - 使用git remote添加远程仓库
  - 使用工具添加远程仓库(sourcetree)
  ----
  
  **添加别名同步提交多个远程仓库**

  ![](https://cdn.jsdelivr.net/gh/stupidur/images@main/notes/20220526120741.png)

3. 通过GitHub的Actions创建workflows实现提交自动同步到Gitee仓库

```yaml
# 通过 Github actions， 在 Github 仓库的每一次 commit 后自动同步到 Gitee 上
name: gitee-sync
on:
  push:
    branches:
      - master
jobs:
  repo-sync:
    env:
      dst_key: ${{ secrets.GITEE_PRIVATE_KEY }}
      dst_token: ${{ secrets.GITEE_TOKEN }}
      gitee_user: ${{ secrets.GITEE_USER }}
    runs-on: ubuntu-latest
    steps:
      - name: sync github -> gitee
        uses: Yikun/hub-mirror-action@master
        if: env.dst_key && env.dst_token && env.gitee_user
        with:
          # 必选，需要同步的 Github 用户（源）
          src: 'github/${{ github.repository_owner }}'
          # 必选，需要同步到的 Gitee 用户（目的）
          dst: 'gitee/${{ secrets.GITEE_USER }}'
          # 必选，Gitee公钥对应的私钥，https://gitee.com/profile/sshkeys
          dst_key: ${{ secrets.GITEE_PRIVATE_KEY }}
          # 必选，Gitee对应的用于创建仓库的token，https://gitee.com/profile/personal_access_tokens
          dst_token:  ${{ secrets.GITEE_TOKEN }}
          # 如果是组织，指定组织即可，默认为用户 user
          # account_type: org
          # 直接取当前项目的仓库名
          static_list: ${{ github.event.repository.name }}
          # 还有黑、白名单，静态名单机制，可以用于更新某些指定库
          # static_list: 'repo_name,repo_name2'
          # black_list: 'repo_name,repo_name2'
          # white_list: 'repo_name,repo_name2'
          account_type: user  # 账户类型
          clone_style: "ssh"  # 使用https方式进行clone，也可以使用ssh
          debug: true  # 启用后会显示所有执行命令
          force_update: true  # 启用后，强制同步，即强制覆盖目的端仓库
          # static_list: "python-nianbao-struct"  # 静态同步列表，在此填写需要同步的仓库名称，可填写多个
          timeout: '600s'  # git超时设置，超时后会自动重试git操作

```

- 涉及到三个变量`GITEE_PRIVATE_KEY`, `GITEE_TOKEN`, `GITEE_USER`
  - `GITEE_PRIVATE_KEY`:通过ssh提交到Gitee的私钥
  - `GITEE_TOKEN`:
    > 在gitee打开个人设置—>安全设置—>私人令牌，新建一个私人令牌，命名随意，复制生成的令牌值
    ![](https://cdn.jsdelivr.net/gh/stupidur/images@main/notes/20220526152532.png)
    
  - `GITEE_USER`:仓库用户名

   

