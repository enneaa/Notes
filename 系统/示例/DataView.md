# DataView

### DataView 查询语法

````
```dataview
<QUERY-TYPE> <WITHOUT ID> <字段>
FROM <来源>
<WHERE> <条件表达式>
<SORT> <排序依据 排序方式>
<GROUP BY> <分组依据>
<LIMIT> <限定显示记录数>
<FLATTEN> <拆分表达式>
```
````

### QUERY-TYPE 展示方式
### 1\. TABLE 表格
以表格的形式显示符合查询条件的文件清单和相关属性。

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  aliases as "别名",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
SORT file.cday DESC
```
````

### 2\. LIST 列表
以无序列表的形式显示符合查询条件的文件清单。

````
```dataview
LIST
FROM "200-学习箱/210-知识库搭建"
```
````

### 3\. TASK 任务
仅以任务列表的形式显示符合查询条件的任务列表。

````
```dataview
TASK
FROM "200-学习箱/210-知识库搭建"
```
````

### 4\. CALENDAR 日历
以日历视图的形式显示查询结果，日历现在还有个 Bug，经常会显示两个重复月份的日历。

````
```dataview
CALENDAR file.cday
WHERE contains(file.name, "PicGo")
```
//必须加日期型的字段作为日历中的定位
CALENDAR file.cday
WHERE contains(file.name, "PicGo")
````

### 字段说明
### 1\. 文件自带的属性

| 文件属性 | 字段类型 | 属性说明 |
| --- | --- | --- |
| file.name | Text | 文件名 |
| file.folder | Text | 所在文件夹 |
| file.path | Text | 完整路径 + 完整文件名 |
| file.ext | Text | 扩展名 |
| file.link | Link | 链接至本文件 |
| file.size | Number | 文件大小 (bytes) |
| file.ctime | Date Time | 创建时间 |
| file.cday | Date | 创建日期 |
| file.mtime | Date Time | 最后修改时间 |
| file.mday | Date | 最后修改日期 |
| file.tags | List | 文中的 标签 和 YAML 中的 tags |
| file.etags | List | 文中的 标签 和 YAML 中的 tags |
| file.inlinks | List | 反向链接 |
| file.outlinks | List | 正向链接 |
| file.tasks | List | 文中的任务列表 |
| file.lists | List | 文中的列表 (包含任务列表) |
| file.frontmatter | List | 文件中的 YAML 块内容 |
| file.starred | Boolean | 加星 |

### 2\. YAML 定义属性
`:` 之前的为属性名，直接使用，不需要 `file.属性`

### 3\. 属性的字段类型
| 字段类型 | 表达方式 | 举例 |
| --- | --- | --- |
| Text | 用 "" 括起来的字符 | " 这就是 Text 类型 " |
| Number | 由符号 + - 数字 0 ~ 9 和小数点 . 组成 | \-0.98 |
| Boolean | 只有 true 和 false 两个值，表示是与否 | true |
| Date | 由日期和时间组成，遵循 ISO 8601 标准
格式为 YYYY-MM\[-DDTHH:mm:ss.nnn+ZZ\] | 2021-04-18 |
| Link | 使用 \[\[内部文件链接\]\] 的链接 | "\[\[内部文件链接\]\]" |
| List | 有序/无序/任务列表 | \- 无序列表 |

查询实例

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  aliases as "别名",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
SORT file.cday DESC
```
````

### FROM 数据来源
### 1\. Tags 标签
````
```dataview
LIST
FROM #DataView 
```
````

### 2\. Folders 文件夹
````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  aliases as "别名",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
SORT file.cday DESC
```
TABLE WITHOUT ID
  file.link as "文件名称",
  aliases as "别名",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
SORT file.cday DESC
```
````

### 3\. Specific Files 指定文件
````
```dataview
TABLE WITHOUT ID
  file.link as "文件名",
  file.tasks.text as "任务名",
  choice(file.tasks.completed, "是", "否") as "已完成"
FROM "200-学习箱/210-知识库搭建/Markdown for macOS"
```
````

### 4\. Links 链接
````
```dataview
LIST
FROM [[内部文件链接]]   
```
//查询 [[内部文件链接]] 被哪些文件链接，即入链

```dataview
LIST
FROM outgoing([[Markdown for macOS]]) 
```
````

### 5\. Combing Sources 多重来源
即将 1、2、4 来源联合起来使用

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  aliases as "别名",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建" and outgoing([[Markdown for macOS]])
SORT file.cday DESC
```
````

### WHERE 过滤条件
### 1\. Text 类条件
- **1\. 包含指定文本**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE icontains(file.name,"obsidian")
```

// contains(file.name,"obsidian") 大小写敏感
// icontains(file.name,"obsidian") 大小写不敏感
````

- **2\. 不包含指定文本**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE !icontains(file.name,"obsidian")
```
````

- **3\. 以特定文本开头**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE startswith(file.name,"Obsidian")
```
````

- **4\. 以特定文本结尾**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE endswith(file.name,"COS")
```
````

- **5\. 英文大小写转换**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE endswith(lower(file.name),"cos")
```

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE startswith(upper(file.name),"OBSIDIAN")
```
````

### 2\. Number 类条件

- **1\. 等于与不等于**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE number1 = 9
```

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE !(number1 = 9)
```
````

- **2\. 大于或大于等于**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE number1 >= 8
```
````

___

- **3\. 小于或小于等于**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE number1 <= 9
```

// 当文件 YAML 没有定义该属性时，该属性值默认为 0
````

- **4\. 数字的四则运算**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE number1 - number1 <= 0
```
````

### 3\. Date 类条件
- **1\. 日期的格式化**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  dateformat(file.ctime,"HH:mm: ss") as "创建时间",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday <= date("2023-02-20")
```

//日期格式化通常只作为输出显示格式定义，不作为条件
````

- **2\. 等于指定日期**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday = date("2023-02-19")
```
````

- **3\. 大于等于指定日期**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday >= date("2023-02-19")
```
````

- **4\. 小于等于指定日期**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday <= date("2023-02-20")
```
````

- **5\. 常用的日期属性**

````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday.month = date(today).month
```

// year
// month
// day
// date (today)
// date (now)
// date (tomorrow)
// date (yesterday)
// date (sow)
// date (eow)

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.cday >= date(sow) and file.cday <= date(eow)
```
````

### 4\. Boolean 类条件
````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE file.starred
```

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE !file.starred
```

```dataview
TASK
WHERE !fullyCompleted
```
// 如果本级任务未完成，下级任务已完成，会将下级已完成的一起显示

```dataview
TASK
WHERE fullyCompleted
```
````

Boolean 类字段的格式化，就是将 true、false 转换为更偏于用户阅读的是 / 否，或者是 yes / no

### 5\. List 类条件
````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE contains(tags,"DataView")
```

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE !contains(tags,"DataView")
```
// 如果 YAML 中没有定议 tags，则默认为空
````

### 6\. Link 类条件
````
```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE contains(file.outlinks, [[内部文件链接]])
```

```dataview
TABLE WITHOUT ID
  file.link as "文件名称",
  dateformat(file.cday,"yyyy-MM-dd") as "创建日期",
  choice(file.starred, "是", "否") as "加星"
FROM "200-学习箱/210-知识库搭建"
WHERE contains(file.inlinks, [[Markdown for macOS]])
```
````

### 7\. And 条件连接符
```
<条件1> and <条件2>

1. 只满足<条件1>，不会被显示到查询结果中
2. 只满足<条件2>，不会被显示到查询结果中
3. 同时满足<条件1>和<条件2>，会显示到查询结果中
```

### 8\. Or 条件连接符
```
<条件1> or <条件2>

1. 只满足<条件1>，会被显示到查询结果中
2. 只满足<条件2>，会被显示到查询结果中
3. 同时满足<条件1>和<条件2>，会显示到查询结果中
```

### 9\. () 定义条件优先级
```
仅发生在需要同时运用 and 和 or 两个边接符时，才需要使用括号定义优先级

（<条件1> or <条件2>) and <条件3>

1. 只满足<条件1>，不会被显示到查询结果中
2. 只满足<条件2>，不会被显示到查询结果中
3. 只满足<条件3>，不会被显示到查询结果中
4. 同时满足<条件1>和<条件2>，不会显示到查询结果中
5. 同时满足<条件1>和<条件3>，会显示到查询结果中
6. 同时满足<条件2>和<条件3>，会显示到查询结果中
7. 同时满足<条件1>、<条件2>和<条件3>，会显示到查询结果中
仅发生在需要同时运用 and 和 or 两个边接符时，才需要使用括号定义优先级

（<条件1> and <条件2>) or <条件3>

1. 只满足<条件1>，不会被显示到查询结果中
2. 只满足<条件2>，不会被显示到查询结果中
3. 只满足<条件3>，会被显示到查询结果中
4. 同时满足<条件1>和<条件2>，会显示到查询结果中
5. 同时满足<条件1>和<条件3>，会显示到查询结果中
6. 同时满足<条件2>和<条件3>，会显示到查询结果中
7. 同时满足<条件1>、<条件2>和<条件3>，会显示到查询结果中
```
