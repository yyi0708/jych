---
title: 开发工作流选择
date: 2023-07-21 19:00:47
tags: 
  - 前端
  - 开发工作流
category: developer
timeline: front-end
toc: true
abstract: 聊一聊在git提交代码，常见一些可以在git的hook的自动化操作
---

## 流程图
![流程图](https://pica.zhimg.com/80/v2-0bbb25856cf993d40022aaedeb392822_720w.jpeg?source=d16d100b)

## 写在前面
随着前端领域模块化发展，使开发人员更加专注，小而美、小而精的模块越来越多，有很多好的想法与概念，亦技术小技巧，总是让我由衷感叹贡献者。

模块化、思想概念快速产出的时代，也导致了学习者进步者的涉猎或者说接受新鲜知识范围广，也容易学而忘。

**开发工作流**

开发代码工作中，希望需要辅助工具/库，保证代码质量、统一风格，约束规定提交信息，及推送库之前运行测试用例, 此流程目的，避免🚫💩进入你的代码仓库,不便于后期维护。

其中，还有其他优秀库，可以进行替换。


## 如上图资源地址：
* [eslint](https://github.com/eslint/eslint) : Find and fix problems in your JavaScript code.
* [prettier](https://github.com/prettier/prettier)  : Prettier is an opinionated code formatter.
* [husky](https://github.com/typicode/husky)  : Git hooks made easy 🐶 woof!
* [lint-staged](https://github.com/okonet/lint-staged):    🚫💩 — Run linters on git staged files
* [commitlint](https://github.com/conventional-changelog/commitlint) : 📓 Lint commit messages
* [commitizen](https://github.com/commitizen-tools/commitizen):  Create committing rules for projects 🚀 auto bump versions ⬆️ and auto changelog generation 📂
* [cz-git](https://github.com/Zhengqbbb/cz-git) cz-git | czg 🛠️ DX first and more engineered, lightweight, customizable, standard output format Commitizen adapter and CLI
* [standard-version](https://github.com/conventional-changelog/standard-version):   🏆 Automate versioning and CHANGELOG generation, with semver.org and conventionalcommits.org
* [bumpp](https://github.com/antfu/bumpp) Interactive CLI that bumps your version numbers and more