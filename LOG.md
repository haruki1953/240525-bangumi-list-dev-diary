### 240525
新建开发记录文件夹

### 240526
开始写项目文档
新建github仓库

### 240527
开始修改后端

### 240528
后端一期完成
获取了现有数据
前端创建项目

### 240529
暗黑模式、自定义主题
项目初始化

### 240530
- 整体路由设计
- layout页面布局：
- 暗黑模式引起的自定义样式问题
- 响应式边距
- 菜单栏

### 240531
- 菜单栏响应式
- 后端精简，不保存评论和角色图片了

### 240601
- 数据请求与保存
- 正在加载标识，控制装饰点动画

### 240602
首页：响应式布局、封装bgmCard

### 240603
- 懒加载占位优化
- 解决切换暗黑模式的卡顿问题
- 获取在首页显示的番剧
- 多种排序，BgmList封装排序选择
- 排序切换时还是有问题

### 240604
- 解决了切换排序的问题
- 完成了分组显示的基本框架
- 按星期分组显示

### 240605
- 日期分组、评分分组、封装BgmGroup
- 全部番剧页面：
- 控制显示数量，滚动触发（无限滚动）
- 搜索
- 关于页面写了一点

### 240606
- 完成关于页
- 完成部署
- 解决若干bug

### 240607
- 二期后端：
- 改用模板引擎
- 爬取标签、别名

### 240608
- 二期后端：
- 优化结构、alistPath改为完整网址
- 爬取标签、别名
- 页面完善、适配暗黑模式
- 二期前端：
- 版本控制
- 通知功能
- 关于页：联系信息、友情链接、查看通知、重置数据

### 240609
- 二期前端：
- 番剧搜索升级（标签别名）
- 收藏功能、猜你喜欢
- 部署：部署前端

### 240610
- 部署后端
- 写了后端部分的文档

### 240611
- 马马虎虎写完了文档
- 尝试广告接入

### 240612
Google AdSense 正在接受審查
不管收款之类的是否可行，先试试吧

### 240629改进
- 番剧卡片下方添加番剧名
- 控制 番剧名 与 嵌入的Discord 显示隐藏

### 240710改进
尝试番剧封面反向代理，失败了

### 240713改进
- 前端重写数据中的链接
- 解决特殊字符显示问题

### 240714改进
- 关于页面添加可动态控制的内容
- html头部优化
- 版本控制优化

### 240803改前端广告赚米
已将soyo.mom独立为一个前端

### 240804改前端广告赚米
试着接入monetag

### 240827改进
添加底栏广告位

### 240911改进
- 完善layout
	- 菜单栏封装
	- 调整全局css样式
	- 重新设计广告栏
- 新增小工具：推特图片拼接

### 240912改进
**拼接功能增加**
- 新增二分、三分图片功能
- 可设置间隔

**字幕拼接优化**
- 利用services中的函数重构
- 可强制约束为固定比例（消除手机截屏两侧的黑边）
- 优化样式

### 240914推特图片拼接
配置持久化、配置完善
- 配置项初始化时，赋值为store中的值
- 在图片生成成功时，保存配置项至store
- 在点击重置按钮时，重置组件配置项，并重置store中的值
- 新增多个配置项，以更好地控制图片生成

### 241004改进图片拼接
- 改进图片拼接，进行封装
- 图片比例为低于或高于16:9时，改进拼接使其不会错位，这也可以完全防止边缘溢出
- 比例高于16:9时横向拼接主图与副图
- 改进保存

### 241011大改进
**优化Store，优化方法，优化组件**，使其适应更多功能

### 241026实现更新提示
完成了qbauto相关的脚本

### 241027实现更新提示
完成

### 241029实现更新提示
- 添加番剧更新页
- 优化样式

### 241103改进
- 优化首页番剧顺序
- 优化社交媒体图标链接
- RSS实现