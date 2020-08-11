## Serve酱自动推送热门

> 🔝 通过Serve酱自动发送微博、知乎、v2ex热门内容到微信，可自定义发送周期


自动将V站、微博、知乎热门发送到指定的微信账号。可配置 workflow 的触发条件为 schedule，实现周期性定时发送热门内容。

### 示例

clone 此 GitHub 仓库，修改.github/workflows/ 文件夹下一个 main.yml 文件，内容如下：

```
name: 'Push News'

on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 */3 * * *' # 定义 cron 表达式
  watch:
    types: [started] # 定义star是自动发送

env:
  TZ: Asia/Shanghai

jobs:
  Gitfolio-Spider:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: 'Set up Python'
        uses: actions/setup-python@v1
        with:
          python-version: 3.8
      - name: 'Install dependencies'
        run: python -m pip install --upgrade pip
      - name: 'Install requirements'
        run: pip install -r ./requirements.txt
      - name: 'Working'
        env:
          SECRET: ${{ secrets.SECRET }}
        timeout-minutes: 350
        run: |
          echo SECRET=$SECRET > .env
          python main.py
          rm -f .env

```

**注意**

- cron 是 UTC 时间，使用时请将北京时间转换为 UTC 进行配置。
- 请在项目的 Settings -> Secrets 路径下配置好SECRET(server酱密钥)，不要直接在 .yml 文件中暴露地址跟密钥

### 效果

![](https://cdn.jsdelivr.net/gh/wangningkai/wangningkai/assets/20200811085352.png)