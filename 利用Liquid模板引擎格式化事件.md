# 利用Liquid模板引擎格式化事件
## Liquid模板引擎
liquid是[Shopify](http://shopify.com/)开发的一款模板引擎，it was designed to have a simple markup and to be non evaluating and secure。更多信息请阅读[主页](http://liquidmarkup.org/)或[Github项目](https://github.com/Shopify/liquid/)。
## 在Huginn中的基本使用
一些简单的使用可参见源代码中的[HipChat](https://github.com/cantino/huginn/blob/07243cee345060316ff2b27530e20e38e72d7713/app/models/agents/hipchat_agent.rb)或[Pushbullet](https://github.com/cantino/huginn/blob/07243cee345060316ff2b27530e20e38e72d7713/app/models/agents/pushbullet_agent.rb) agents。假设agents产生的playload中有一个键为`message`，你只需要使用`{{message}}`就可以动态地表示`message`的值。
为了格式化输出，你可以使用过滤器，它们的工作原理很像UNIX管道：
{{ 'hello' | [replace: 'hello', 'hi'](https://docs.shopify.com/themes/liquid-documentation/filters/string-filters#replace) | [upcase](https://docs.shopify.com/themes/liquid-documentation/filters/string-filters#upcase) }}将会输出'HI'。
内置过滤器的完整列表请参见[Shopify](http://docs.shopify.com/themes/liquid-basics/output)上的文档。
## Huginn 提供的对象（objects）
### Event（事件）对象
Event（事件）对象本质上就是上游Agent产生的哈希（hash），下面是它所包含的一些特殊的键：
* **\_Location\_**：包含事件所对应的位置信息，包含以下特殊的键：`atitude` (简称`lat`)、`longitude` (简称`lng`)、`radius`、 `course` 和`speed`，一些处理位置的agents中需要设置它。
* **agent**代表创建事件的上游Agent。
* **created_at**代表事件的时间戳。
### Agent对象
Agent对象主要含有以下键：
* **type**返回类型名，比如`"HipchatAgent"`；
* **name**返回Agent的名称；
* **options**返回选项哈希（hash）；
* **memory**返回记忆哈希（hash）；
* **sources**返回包含上游Agents的数组；
* **receivers**返回包含下游Agents的数组；
* **disabled**返回布尔值，当Agent为disabled的状态，返回true。
## Huginn提供的过滤器（Filters）
* **uri_escape**返回一个URI的转义字符串，该过滤器在生成URL查询字符时很有用；
* **to_uri**将输入解析成一个URI对象，可基于基本的URI有选择地分解它，一个URI对象包含以下属性：`scheme`、`userinfo`、 `host`、`port`、`registry`、`path`、`opaque`、`query`和`fragment`，你需要将这些属性赋值给变量。
* **uri_expand**可通过5次以内的递归重定向返回被给URL的最终URL；如果被给的字符串不是一个有效的HTTP URL（地址），或者重定向次数太多，将返回原始字符串；如果在重定向的过程中发生网络或协议错误，将返回最后一次重定向得到的URL。例如，当变量`short_url` 包含一个URL："https://goo.gl/tfDrI"，则`{{ short_url | uri_expand }}`将返回URL（地址）"https://github.com/cantino/huginn"。
* **unescape**不转义字符串中的基本HTML实体，目前它只能解码以下实体：`&apos;`, `&quot;`, `&lt;`, `&gt;`, `&amp;`, `&#dd;` and `&#xhh;`（**感觉有点别扭，待商榷**）。
* **to_xpath**返回WebsiteAgent中原始字符串所对应的Xpath文字或表达式；例如：假如你正在制作一个WebsiteAgent，该Agent接受了一个包含`word`的事件，用来查询网页词汇，你可以像下面这样创建一个动态的Xpath表达式：
  ```
  //dl/dt[text()={{word | to_xpath}}][1]/following-sibling::dd[1]/text()
  ```
  这种使用过滤器的方法比直接使用`'{{word}}'`或`"{{word}}"`更加可靠，因为即使`word`中包含省略号或引号，该方法也是有效的。

* **regex_replace&regex_replace_first**将用其它的字符串代替通过正则表达式匹配到的所有字符串或者匹配的第一个字符串，与内置的replace过滤器类似。例如，假如`foobar`包含字符串"foobaz foobaz"，那么`{{ foobar | regex_replace: '(\S+)baz', 'qux\1' }}`将返回"quxfoo quxfoo"。你可以使用Ruby`Regexp` 类支持的任何正则表达式，具体参见[文档](http://ruby-doc.org/core/doc/regexp_rdoc.html)。在过滤器中可以使用转义字符，比如`\r`、`\n`、`\xXX`和`\u{XXXX}`。在用来替代的字符串中，支持正则表达式组的概念（`\1`..`\9`或`\k<name>`、`\&`、`\'`，等等）。值得注意的是，Liquid模板本身没有反斜杠的概念，你不能使用像这样`"\""`的转义字符串；当需要在双引号中使用双引号时，你可以使用`\x22`，像这样`{{ text | regex_replace: "\x22(.*?)\x22", "\1" }}`。
* **json** ：将对象转化成json字符串：`{{ data | json}}`。
## Huginn提供的标签
* **credential**通过定义的凭证名返回存储的用户凭证，使用方法为：{% credential USER_CREDENTIAL_NAME %}，注意这里没有使用引号，凭证名区分大小写，必须与存储的凭证名一模一样。
  * **line_break**代表文本中的换行符，使用方法为：{% line_break %}。注意这里也没有引号或反引号（在HTML邮件中，你可能需要使用`<br>`来代替）。
  * **regex_replace&regex_replace_first**replace every occurrence of a given regex pattern in the first "in" block with the result of the "with" block in which the variable `match` is set for each iteration, which can be used as follows：
    * `match[0]` 或`match`：整个匹配的字符串；
    * `match[1]`..`match[n]`: strings matching the numbered capture groups
    * `match.size`: total number of the elements above (n+1)
    * `match.names`: array of names of named capture groups
    * `match[name]`..: strings matching the named capture groups
    * `match.pre_match`: string preceding the match
    * `match.post_match`: string following the match
    * `match.***`: equivalent to `match['***']` unless it conflicts with the existing methods above
      If named captures (`(?<name>...)`) are used in the pattern, they are also made accessible as variables.  Note that if numbered captures are used mixed with named captures, you could get unexpected results.
      Example usage:
  ```
  {% regex_replace "\w+" in %}Use me like this.{% with %}{{ match | capitalize }}{% endregex_replace %}
  {% assign fullname = "Doe, John A." %}
  {% regex_replace_first "\A(?<name1>.+), (?<name2>.+)\z" in %}{{ fullname }}{% with %}{{ name2 }} {{ name1 }}{% endregex_replace_first %}

  Use Me Like This.
  John A. Doe
  ```
## 更复杂的应用
在这个例子中，HipchatAgent处理BasecampAgent提供的事件。
Basecamp事件如下（在完整事件上有所删减）：
```
{
  "creator": {
    "name": "Dominik Sander",
  },
 "excerpt": "test test",
 "summary": "commented on whaat",
 "action": "commented on",
 "target": "whaat",
 "html_url": "https://basecamp.com/12456/projects/76454545-explore-basecamp/messages/76454545-whaat#comment_76454545"
}
```
现在我们利用Liquid模板引擎生成格式化的message，则HipchatAgent的选项如下：
```
{
  'auth_token' => 'token',
  'room_name' => 'test',
  'username' => "{{creator.name}}",
  'message' => '{{summary}}<br>{{excerpt}}<br><a href="{{html_url}}">View it on Basecamp</a>',
  'notify' => false,
  'color' => 'yellow',
}
```
### More Examples
从字符串中移除换行符：`{{foo | strip_newlines}}`
在Liquid模板中处理日期：日期被定义为从格林威治时间 1970 年 01 月 01 日 00 时 00 分 00 秒起到现在的总秒数，可以被作为整型使用过滤器处理。
例如，一个存储日期的整型变量dateVar像下面这样格式化输出，其先被除以1000，再经过Liquid模板的日期格式化过滤器处理：
```
{{ dateVar |divided_by: 1000 |date: \"%c\" }}
```
其他输出的日期格式请参见：https://docs.shopify.com/themes/liquid-documentation/filters/additional-filters#date
```

```