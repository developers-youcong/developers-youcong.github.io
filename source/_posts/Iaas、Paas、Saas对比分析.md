---
title: Iaas、Paas、Saas对比分析
date: 2020-05-21 14:49:41
tags: "计算机"
---
# Iaas、Paas、Saas对比分析
从Iaas、Paas到Saas，客户所要管理负责由多到少，成本也越来越低。下面所列举的，足以看出。
以我所待的公司，无论是外包还是创业公司也好。基本上采用的都是Saas这种模式，Sass对于客户而言省事省时省钱(从Applications到Compute无需管理)。对于供应商，也就是我们而言，由于从应用开发到部署，再到维护都由我们负责，从一定程度上降低一些问题排错的时间成本(过去写一套系统部署在不同的操作系统，不同的操作系统的版本环境存在差异，可能会遇到在这个系统正常运行，在那个系统出现一些问题，不排除是因为代码的原因，但有的时候也与环境有极大的关系)。
<!--more-->

## 传统架构

### 客户管理
- Applications(应用)
- Data(数据)
- Runtime(运行环境)
- Middleware(中间件)
- O/S(操作系统)
- Virtualization(虚拟化)
- Servers(服务)
- Storage(存储)
- Compute(计算机)

## Infrastructure as s Service(基础设施即服务)

### 客户管理
- Applications
- Data
- Runtime
- Middleware
- O/S

### 供应商管理
- Virtualization
- Servers
- Storage
- Compute

## Platform as a Service(平台即服务)

### 客户管理
- Applications
- Data

### 供应商管理
- Runtime
- Middleware
- O/S
- Virtualization
- Servers
- Storage
- Compute

## Software as s Service(软件即服务)

### 供应商管理
- Applicationis
- Data
- Runtime
- Middleware
- O/S
- Virtualization
- Servers
- Storage
- Compute
