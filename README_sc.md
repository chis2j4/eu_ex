[English](README.md) | 简体中文 | [正體中文](README_tc.md)

# 🚨⚠️ Warning: No longer maintained! ⚠️🚨

**As of January 29th, 2022, eu_ex is no longer maintained, and no further changes will be made to it.**

# eu_ex

eu_ex 是 EUserv_extend 的简写。一个 Python 脚本，可以帮你续期免费 EUserv IPv6 VPS。

这个脚本可以自动检查你账户中的 VPS 数量，如果可以续期，就为 VPS 续期。

## 如何使用

1. 安装 Python3 和必要的依赖，以下命令在 debian/ubuntu 为例，

   ```bash
   #Install Python3
   apt install python3 python3-pip -y
   #Intstall dependences
   pip install requests beautifulsoup4
   ```

2. 不建议在 `main.py` 文件第 37-38 行直接用你的真实值去替换`USERNAME`和`PASSWORD`。可以使用环境变量传入。

   你可以添加多个账户，并以一个空格分隔。

3. 你可以添加多个 mailparser.io 解析数据的下载 URL ID，并以一个空格分隔。下载的 URL ID 在`https://files.mailparser.io/d/<download_url_id>`。

4. 将 **Actions secrets** 传入你的 GitHub Action 运行环境的环境变量。例如，以下环境变量是必需的。

   ```
   env:
       USERNAME: ${{ secrets.USERNAME }}
       PASSWORD: ${{ secrets.PASSWORD }}
       # https://mailparser.io   
       MAILPARSER_DOWNLOAD_URL_ID: ${{ secrets.MAILPARSER_DOWNLOAD_URL_ID }}
   ```

## 邮件转发和 mailparser 设置
### 邮件转发

以 gmail 为例, 转发邮件至 [mailparser](https://mailparser.io)。可以是非 gmail 邮箱，前提是可以收到 euserv 的邮件。目前 outlook/hotmail 是收不到的。

- ![gmail_filter_keys](./images/gmail_filter_keys.png)

- ![gmail_filter_setting](./images/gmail_filter_setting.png)

- ![gmail_forward_setting](./images/gmail_forward_setting.png)

### Mailparser 设置

- 首先创建新的收件箱。
- 创建数据解析规则。
  - 数据解析规则，pin 为必需，其他可选。
   ![mailparser_data_parsing_rules](./images/mailparser_data_parsing_rules.png)
  - pin 的解析规则
  ![mailparser_data_parsing_rules_pin](./images/mailparser_data_parsing_rules_pin.png)
  - subject 的解析规则
  ![mailparser_data_parsing_rules_subject](./images/mailparser_data_parsing_rules_subject.png)
  - sender 的解析规则
  ![mailparser_data_parsing_rules_sender](./images/mailparser_data_parsing_rules_sender.png)
  - receiver 的解析规则
  ![mailparser_data_parsing_rules_receiver](./images/mailparser_data_parsing_rules_receiver.png)
- 创建解析数据下载 URL
  - 解析数据下载 URL
  ![mailparser_parsed_data_downloads](./images/mailparser_parsed_data_downloads.png)
- 解析数据下载设置
  ![mailparser_parsed_data_downloads_setting](./images/mailparser_parsed_data_downloads_setting.png)
- mailparser 收件箱设置（可选，为减少收到 spam 邮件的风险，最好设置）
  - mailparser 收件箱设置 1
  ![mailparser_inbox_setting_1](./images/mailparser_inbox_setting_1.png)
  - mailparser 收件箱设置 2
  ![mailparser_inbox_setting_2](./images/mailparser_inbox_setting_2.png)

## 最终效果
效果如图,

![mailparser_inbox_setting_2](./images/the_final_effect.png)

## 待办事项

- [x] ~~验证 mailparser 解析的`receiver'字段，以减少恶意邮件的干扰。~~ 由于 mailparser *Inbox Settings - Email Reception*，所以不做了。
- [x] 日志国际化和本地化。
- [ ] ~~当验证码识别 API 失效时开放预训练的模型，在本地，不调用第三方接口就解决验证码识别的问题。~~
- [ ] ~~考虑 euserv 访问超时重试，较低优先级。~~

## 鸣谢

- 感谢 EUserv 提供免费的 IPv6 VPS 供我们学习。
- 感谢 CokeMine 和其仓库贡献者为我们提供的最初 *EUserv_extend* 脚本。互联网永远不会忘记，但人们会。

## FAQs

1. **Q**: 可以非 gmail 邮箱吗?

   **A**: 可以是非 gmail 邮箱，前提是可以收到 euserv 的邮件。目前 outlook/hotmail 是收不到的。

2. **Q**: n 个邮箱能在用同一个 mailparser 还是需要申请 n 个 mailparser 与之一 一对应吗？

   **A**: mailparser free 账号最多可以设置 10 个收件箱，这 10 个收件箱就可以对应 10 个 euserv 账号，也就有了 10 个  mailparser parsed data download URL(id)。所以，这得看你 n>10, 还是 n<10  了。n<10， 一个 mailparser 账号即可，然后 parsed data download URL id 一 一对应  euserv 的注册邮箱账号。

3. **Q**: 续期脚本是如何工作的？

   **A**: EUserv 从 2021 年 9 月底开始，设置了第一道门槛，那就是登录出现验证码(成功验证状态维持 24 小时)，所以目前就用 TrueCaptcha 提供的 API(每天都有免费额度) 进行识别。没过多久，大概是 2021 年11月初，EUserv 又设置了第二道门槛，就是续期时的邮件 PIN 码验证，这里解决办法就大概两种：a. 登录邮箱获取含 EUserv PIN 的邮件。b. 将邮件转化为 HTTP REST API，自动获取。这里采取的是方案 b。方案 b 中可选的存在免费额度的，好像目前仅有 [Mailparser](https://mailparser.io) 和 [Zapier Emails Parser](https://parser.zapier.com/)。方案 b 明显要优于方案 a。

## 参考信息

### EUserv "PIN for the Confirmation of a Security Check" 原始邮件

```
From：	     EUserv Support <support@euserv.de>
To：	         xyz@example.com
Subject：	 EUserv - PIN for the Confirmation of a Security Check
Content-Type: text/plain; charset = utf-8
Dear XYZ,

you have just requested a PIN for confirmation of a security check at EUserv. If you have not requested the PIN then ignore this email.

PIN:
123456

PLEASE NOTE: If you already have requested a new PIN for the same process this PIN is invalid. Also this PIN is only valid within the session in which it has been requested. This means the PIN is invalid if you for example change the browser or if you logout and perform a new login.


Sincerely,
Your customer support EUserv

--
Web ................: http://www.euserv.com
Login control panel.: https://support.euserv.com
FAQ ................: http://faq.euserv.com
Help & Guides.......: http://wiki.euserv.com
Community / Forum...: http://forum.euserv.com
Mailing-Liste ......: http://www.euserv.com/en/?show_contact=mailinglist
Twitter ............: http://twitter.com/euservhosting
Facebook ...........: http://www.facebook.com/euservhosting
--

EUserv Internet
is a division of
ISPpro Internet KG

Postal address:
ISPpro Internet KG
Division EUserv Internet
P.O. Box 2224
07622 Hermsdorf
GERMANY

Support-Phone: +49 (0) 3641 3101011 (English speaking)

Administration:
ISPpro Internet KG
Neue Str. 4
D-07639 Bad Klosterlausnitz
GERMANY

Management...............: Dirk Seidel
Register.................: AG Jena, HRA 202638
VAT Number...............: 162/156/36600
Tax office ..............: Jena
International VAT Number.: DE813856317
```

