# asoul-record
## 简介
以数据库形式保存A-SOUL所有录播信息，方便检索，本项目尽量保证至少每周一次的更新频率。

数据库：sqlite3

同时提供csv格式的数据文件

## 数据表

`record_table`表结构

|字段名|数据类型|其他|说明|
|----|----|----|----|
|record_id|integer|主键自增|每场直播的唯一编号|
|title|text|not null|直播标题|
|time|integer|not null|直播时间<sup>[1]</sup>|
|type|text|not null|直播类型<sup>[2]</sup>|
|role|text| |出场成员<sup>[3]</sup>|
|episode|integer|not null|系列直播期数<sup>[4]</sup>|
|describe|text| |备注|

[1] 直播时间采用yyyy-MM-dd格式存储，并非时间戳。

[2] 直播类型为数字编号，具体对应类型详见`type_table`。

[3] 出场成员由成员英文名首字母顺序用空格隔开组成，例`A E`表示出场成员为向晚和乃琳，在一些测试直播中并未出现任何成员，所以该项可能为空；出场成员的判定不受时间限制，若该成员在直播画面中出现，无论出现时长都会被计入（只出现声音不会被计入）。

[4] 该期数与`type`值搭配使用，当期数为0时，说明该场直播不属于系列直播，当该值>=1时，则为该场直播在`type`中记录的直播类型的期数，例如当type=3,episode=4时，说明该场直播为A-SOUL游戏室第4期。

`type_table`表内容

|type_id|type_name|
|----|----|
|0|其他|
|1|单播|
|2|双播|
|3|A-SOUL游戏室|
|4|A-SOUL小剧场|
|5|A-SOUL夜谈|
|6|生日直播|
|7|线下互动直播|
|8|工商合作直播|
|9|特别活动直播|
|10|贝拉的舞蹈教室|

`type_table`表仅记录直播类型，所以不会频繁改动；`type_id`即为`record_table`表中的`type`字段。