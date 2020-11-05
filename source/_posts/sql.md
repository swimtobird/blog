---
title:  SQLAdvisor总结
date: 2018-03-31 11:12:23
tags:
- sqladvisor
- sql优化
categories:
- 技术
comments: true
---
## 背景

1.开发人员大体完成数据表与CURD操作就没有下文了，等后续出现查询效率问题才迟迟开始优化

2.开发人员对索引的不合理使用，或者不会合理设计索引，导致索引本身在查询上没有发挥作用

##  总结

目前看到可以基本简单解决以上场景问题的SQL的优化手段中,效果最显著的就是索引优化，而[SQLAdvisor](https://github.com/Meituan-Dianping/SQLAdvisor)就是通过对你的SQL语句和索引项进行把脉,从而对你提出合理化的建议

## 快速开始

### 1.docker部署

`docker pull imred/sqladvisor`

### 2.执行命令

`docker run --rm imred/sqladvisor -h xx  -P xx  -u xx -p 'xx' -d xx -q "sql" -v 1`

## 使用体验

- 整体满足大部分查询索引优化要求，并快速得到优化建议
- 需要手动提供语句执行查询，体验上相对不友好，期盼是通过读取mysql日志自动执行并提供优化结果
- 对sql查询优化核心在索引上，更进一步觉得可以结合对提供语句提供优化建议

##  优化方案

- 自动化获取mysql日志并提供索引优化建议

  初步想法：

  1.开启mysql输出查询日志

  2.agent(logstash)去采集过滤采集，将sql语句上报

  3.编写接收sql语句,并过滤重复语句，首次出现语句调用SQLAdvisor执行，并输出结果

- 输出sql语句优化建议

  初步想法：

  暂时是先收集日常比较通用优化规则，基于整理后规则编写优化