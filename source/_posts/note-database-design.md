title: AnyNote数据库设计
date: 2015-07-27 22:20:39
categories:
- 移动开发 
tags:
－ MySQL
---
参考Typecho设计数据库

note: [wp怎么实现的post多版本？个人思路：type="revision", ]

- nid 自增
- title
- name
- content
- created
- modified
- date
- order ?
- uid
- type 类型
- template 使用的模版 ?
- status
- pwd
- parent 多种用法

```
CREATE TABLE `test`.`an_notes` (
  `note_ID` BIGINT(20) NOT NULL AUTO_INCREMENT COMMENT 'note id',
  `user_ID` BIGINT(20) NULL COMMENT '',
  `note_title` TEXT NULL COMMENT '',
  `note_name` VARCHAR(200) NULL COMMENT '',
  `note_content` TEXT NULL COMMENT '',
  `note_status` ENUM('publish', 'draft', 'private', 'static', 'object', 'attachment', 'inherit', 'future', 'pending') NOT NULL DEFAULT 'draft' COMMENT '',
  `note_date` DATETIME NULL COMMENT 'created time',
  `note_modified` DATETIME NULL COMMENT '',
  `note_created` DATETIME NULL COMMENT '',
  `note_parent` BIGINT(20) NULL COMMENT 'can be used in many ways',
  `note_type` VARCHAR(20) NOT NULL DEFAULT 'post' COMMENT 'revision, comment, chat, …',
  `note_template` VARCHAR(45) NULL COMMENT '',
  `note_mine_type` VARCHAR(100) NULL COMMENT 'image, doc…',
  `comment_status` ENUM('open', 'closed', 'registerd') NOT NULL DEFAULT 'open' COMMENT '',
  `note_inherit_status` ENUM('open', 'close') NOT NULL DEFAULT 'open' COMMENT '',
  `note_pwd` VARCHAR(20) NULL COMMENT '',
  PRIMARY KEY (`note_ID`)  COMMENT '');
```

post: [post与note的一对多 github？]

- pid 自增
- title
- cid[] ? 这块比较复杂，是否允许临时修改

meta:

- mid 自增
- name
- type
- desc
- order ?
- count

notemeta: []

- mcid 自增 仅优化用？
- mid
- nid

postmeta:

- pcid
- pid
- mid


options:

- oid 自增
- key
- value
- usr 对应uid

user:

- uid 自增
- name
- pwd
- url
- mail
- screenName
- created
- logged
- group
- authCode
- privilige 权限系统

