### UITableView 取消 Section Header 顶部停留

> iOS 7 之下版本，UITableView 增加新特性，滚动时Section Header 在顶部停留，有些界面比如设置界面，这些新特性将显得多余。

解决方案：将`Section Header`置于与`UITableViewCell`同一层级上，即自定义`Section Header`添加到`UITableView`上，让它随`UITableViewCell`一起滚动。做法就是拦截`UITableView`返回`Section Header View`的代理方法，但返回的是一个`CGRectZero`的`View`，因为这个`View`在`UITableView`滚动时会停留在顶部的，不需要它显示，我们需要做的是自定义一个`View`添加到`UITableView`上，让它作为`Section Header`。这里涉及的关键问题就是让每个自定义的`View`都能添加到对应的`Section`位置上，`UITableView`有一个对象方法可供获取每个`Section Header`所属的位置：

```objective-c
- (CGRect)rectForHeaderInSection:(NSInteger)section;
```

具体做法：

```objective-c
-  (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section 
{
    NSInteger tag = 1009 + section;
    UIView *header = [tableView viewWithTag:tag];
    if (!header){ 
        CGRect rect = [tableView rectForHeaderInSection:section];
        UIView *view = [[UIView alloc] initWithFrame:rect];
        view.tag = tag; // 将View打上标签，避免刷洗tableView时重复添加

        CGRect label_f = (CGRect){15, 0, rect.size.width - 15, 40.f};
        UILabel *label = [[UILabel alloc] initWithFrame:label_f];
        label.textColor = [UIColor lightGrayColor];
        label.font = [UIFont systemFontOfSize:14.0];
        label.text = self.items[section][@"sectionTitle"];
        [view addSubview:label];
        [tableView addSubview:view];
    }
    return [[UIView alloc] init];
}
```

