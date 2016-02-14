title: AnyNote数据库与API设计
date: 2015-07-27 22:20:39
categories:
- 移动开发 
tags:
－ MySQL
---
参考Typecho设计数据库

note: 

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
  `note_date` DATETIME NULL COMMENT 'publish time',
  `note_modified` DATETIME NULL COMMENT '',
  `note_created` DATETIME NULL COMMENT '',
  `note_parent` BIGINT(20) NULL COMMENT 'can be used in many ways',
  `note_type` VARCHAR(20) NOT NULL DEFAULT 'post' COMMENT 'revision, comment, chat, …',
  `note_template` VARCHAR(45) NULL COMMENT '',
  `note_mine_type` VARCHAR(100) NULL COMMENT 'image, doc…',
  `note_comment_status` ENUM('open', 'closed', 'registerd') NOT NULL DEFAULT 'open' COMMENT '',
  `note_inherit_status` ENUM('open', 'close') NOT NULL DEFAULT 'open' COMMENT '',
  `note_pwd` VARCHAR(20) NULL COMMENT '',
  PRIMARY KEY (`note_ID`)  COMMENT '');
```
[wp怎么实现的post多版本？个人思路：type="revision"]

Revisions are stored in the posts table.
Revisions are stored as children of their associated post (the same thing we do for attachments). They are given a post_status of 'inherit', a post_type of 'revision', and a post_name of {parent ID}- revision(-#) for regular revisions and {parent ID}-autosave for autosaves.
By default, WP keeps track of the changes to title, author, content, excerpt.

revisions: 

- rid
- nid-revision-# / nid-autosave-#
- uid
- content
- diff


post: [post与note的一对多 github？]

- pid 自增
- title
- content 由cid生成，包括style信息
- nid[] ? 这块比较复杂，是否允许临时修改(redis缓存)

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

```
CREATE TABLE IF NOT EXISTS `an_users` (
`id` INTEGER NOT NULL auto_increment , 
`user_ID` BIGINT(20) NOT NULL, 
`user_name` VARCHAR(60) NOT NULL, 
`user_pwd` VARCHAR(20) NOT NULL, 
`user_url` VARCHAR(100) NOT NULL, 
`user_email` VARCHAR(100) NOT NULL, 
`user_nickname` VARCHAR(50) NOT NULL, 
`user_created` DATETIME NOT NULL, 
`user_status` INTEGER NOT NULL, 
`user_authcode` VARCHAR(80), 
`user_privilige` INTEGER NOT NULL, 
 PRIMARY KEY (`id`)) ENGINE=InnoDB;
```

#API设计
url(/user)

* POST /user/signin
* POST /user/signout
* POST /user/logout

url(/notes/:id)

* POST /notes 创建
* GET /notes 列表，分页问题
* GET /notes/:id 获得
* PUT/POST /notes/:id 更新
* DELETE /notes/:id 删除