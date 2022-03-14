<div align="center">
<h1 align="center">自动健康填报助手</h1>
</div>

## 简介

每天 17:00, 19:00 GMT+8 触发任务，并延迟随机时间后完成健康填报。

如想修改定时运行的时间，可修改 `.github/workflows/healthReport.yml` 中 `schedule` 属性。

Tips：请勿将本项目用于学习以外的用途。



请先手动在 App 填报一次，之后将默认取昨天的地址进行填报。



**如果当日有信息异常或地点更改，请手动在 APP 端填写！！！**

**如果当日有信息异常或地点更改，请手动在 APP 端填写！！！**

**如果当日有信息异常或地点更改，请手动在 APP 端填写！！！**



愿疫情早日结束！`( p′︵‵。)`



## Github Actions 启用步骤

### 1. Fork 本项目

Fork 本项目: [actions-NjuHealthReport](https://github.com/zhangt2333/actions-NjuHealthReport) (Star 自然是更好)

### 2. 准备需要的参数

* 模板：
    ```json
    {
        "username": "学号",
        "password": "自己的统一认证密码", 
        "deadline": "这里填报截止日期（开区间），超过该天则停止填报并报错"
    }
    ```

* 一个样例：
    ```json
    {
        "username": "123456",
        "password": "abcdefg", 
        "deadline": "2021-9-11"
    }
    ```
* 样例解释：
    * 学号为 `123456`
    * 密码为 `abcdefg`
    * 在 2021 年 9 月 11 日（不包括当天）之前都会自动填报，超出日期则取消填报，并报错发邮件给用户

### 3. 启用 Github Actions

![image-20210216140844300](README/image-20210216140844300.png)

### 4. 将参数填到 Secrets

将填好的参数加入到 Secrets 中：其中 name 为 `DATA`，value 为步骤 2 中填好的多行字符串

![image-20210216140557947](README/image-20210216140557947.png)

### 5. 配置自己账号的邮件提醒

如下图正确配置，这样运行失败的 Github Actions 事件会自动邮件通知你
![](README/img5.png)

## 为多人打卡

1. 依照 `Github Actions 启用步骤` 第 2、4 步，添加新的 Secret，假设命名为 `DATA2`  

2. 复制一遍 `.github/workflows/healthReport.yml` 文件中最后 5 行并将其中的 `secrets.DATA` 中的 `DATA` 改为新 secrets 的键值。

3. 修改后的文件内容尾部应类似如下（假设新键为`DATA2`）

```yaml
  # ...
      - name: Run Spider
        env:
          DATA: ${{ secrets.DATA }}
        run: |
          python health_report_helper/main.py

      - name: Run Spider
        env:
          DATA: ${{ secrets.DATA2 }} # 这里是新增加的 DATA2 键
        run: |
          python health_report_helper/main.py
```

## License

MIT：被授权人有权利在软件和软件的所有副本中包含版权声明和许可声明的前提下，使用、复制、修改、合并、出版发行、散布、再授权及贩售软件及软件的副本。授权人不为被授权人行为承担任何责任，且无义务对著作进行更新。