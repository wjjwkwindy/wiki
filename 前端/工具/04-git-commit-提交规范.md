# Git Commit 提交规范

业界比较推崇 Angular 的 commit 规范 http://suo.im/4rsYee 
Commit message 包括三个部分：Header，Body 和 Footer。完整格式如下： 
```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

## 1.type
提交 commit 的类型，包括以下几种：
* `feat`: 新功能
* `fix`: 修复问题
* `docs`: 修改文档
* `style`: 修改代码格式，不影响代码逻辑
* `refactor`: 重构代码，理论上不影响现有功能
* `perf`: 提升性能
* `test`: 增加修改测试用例
* `chore`: 修改工具相关（包括但不限于文档、代码生成等）

## 2.scope
修改文件的范围，比如：视图层、控制层、docs、config, plugin

## 3.subject
subject 是 commit 目的的简短描述（用一句话清楚的描述这次提交做了什么），不超过50个字符

##  4.body
补充 subject 添加详细说明，可以分成多行，适当增加原因、目的等相关因素，也可不写

##  5.footer
* 当有非兼容修改(Breaking Change)时必须在这里描述清楚
* 关闭issue或是链接到相关文档，如 Closes #1 或 Closes #2, #3


## Revert

> 还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header。  
```
revert: feat(pencil): add ‘graphiteWidth’ option 
This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```