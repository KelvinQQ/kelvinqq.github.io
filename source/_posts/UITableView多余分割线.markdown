---
title: "UITableView多余分割线"
date: 2014-12-04 14:35:20 +0800
tags: 
    - UITableView
categories:
    - iOS

---


`UITableView`会有多余的分割线,不论是否有内容都会显示出来,看着很心塞.
于是这样:

* 设置`UITableView`的`style`为`Group`
* 实现以下`DataSource`

<!--more-->

```
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
{
	return 0.01f;
}
		
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
{
	return 0.01f;
}
```

整个世界安静了.