---
title: UITableView & UIColloectionView iOS8上的异常情况
date: 2019-02-15 18:06:04
categories: Bug List
tags: git
---

# UITableView & UIColloectionView iOS8上的异常情况

最近项目的灰度包中出现了一个crash, fabric信息如下：

```
Fatal Exception: NSInvalidArgumentException
-[__NSMallocBlock__ count]: unrecognized selector sent to instance 0x1bca4330
```

出现在```-[xxx collectionView:numberOfItemsInSection:] ```中，这是一个UICollectionView的一个delegate方法，里面返回了一个数组的count，数组是懒加载的，按理说不会是一个Block。看了下报告只有iOS8上上报了这个crash，原来这里存在一个被忽略了的问题，就是在iOS8上，UITableView、UICollectionView的delegate、dataSource是assign的，一旦对应的viewController已经被释放，但是tableView还没被释放的情况下，容易引发crash。

iOS8以前：

```
// UITableView iOS 8 之前
@property(nonatomic, assign) id<UITableViewDataSource> dataSource
@property(nonatomic, assign) id<UITableViewDelegate> delegate

// UICollectionView iOS 8 之前
@property(nonatomic, assign)id<UICollectionViewDelegate> delegate
@property(nonatomic, assign) id<UICollectionViewDataSource> dataSource
```

iOS9中，使用weak、nullable来替代了assign
/Users/lixingdong/Lxd/git/lxd2502.github.io/source/_posts/git基本操作.md
```
// UITableView iOS 9中使用
@property(nonatomic, weak, nullable) id<UITableViewDataSource> dataSource
@property(nonatomic, weak, nullable) id<UITableViewDelegate> delegate

// UICollectionView iOS 9中使用
@property(nonatomic, weak, nullable) id<UICollectionViewDelegate> delegate
@property(nonatomic, weak, nullable) id<UICollectionViewDataSource> dataSource
```
所以，如果你的应用兼容iOS8及以下版本，应该在viewController的dealloc中将对应的tableView的delegate和dataSource置为nil：

```
// 解决方案：

- (void)dealloc
{
_tableView.delegate = nil;
_tableView.dataSource = nil;
}


```
