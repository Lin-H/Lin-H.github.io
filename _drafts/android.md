
- NestedScrollView	
- ScrollView	
- CardView	像卡片一样的一个个条目，通常用来展示文章的标题和一张图片
- RecyclerView	ListView的升级版
- CollapsingToolbarLayout 
- AppBarLayout	Toolbar使用的layout，需要设置NoActionBar，且setSupportActionBar()
- CoordinatorLayout	协同layout
- layout_behavior	在RecyclerView中定义滑动方式


## RecyclerView

重写ViewHolder

重写getItemCount()返回的int就是item的数量
重写getItemViewType(int position)根据position判断返回的View的类型，目的是达到显示不同View的item目的，返回的值是一个整数，并会在onCreateViewHolder(ViewGroup parent, int viewType)方法中使用到

Toolbar中的menu
menuItem 通过add(int)添加item的过程中附加ID，然后可以通过ID使用getItemId()来返回相应的item，并且在监听事件中，也是通过这个ID来判断点击的是哪个item

## Navigation
It is also possible for us to add menu items programmatically, we just have to call getMenu() to retrieve our menu and then items can be added to that instance.

In order to capture click events on our menu items we just need to set an OnNavigationItemSelectedListener, this will allow us to react to any touch events that take place on our menu.

onCreateOptionsMenu()在app启动时创建菜单，只运行一次，除非重新刷新菜单
在app运行后动态修改菜单使用onPrepareOptionsMenu()，可以得到一个当前的Menu，可以随意修改还需要调用刷新invalidateOptionsMenu()

# 目前的问题是

在滑动RecyclerView的时候有轻微卡顿，怀疑是图片加载的问题，网上的解决办法是使用cache，google`recyclerview cache` 
并且使用图片缓存LruCache类,卡顿已经解决。

现在需要解决图片不会随着滑动而重新加载的问题。其实在滑动中，onBindViewHolder()会触发，之前发现图片重新加载，是因为ImageLoader对象的创建写在了onBindViewHolder()中，导致每次触发onBindViewHolder()时都要重新创建，导致缓存和性能受影响。解决的办法是使用单例模式设计ImageLoader(和 Volley的请求队列RequestQueue)。

然后还有如何处理包含HTML元素的文本,最后发现Html.fromHtml()这个方法所支持的HTML标签太少，而且无法加载CSS和图片，现在试试使用WebView
参考页面：http://stackoverflow.com/questions/4950729/rendering-html-in-a-webview-with-custom-css
htmlData = "<link rel=\"stylesheet\" type=\"text/css\" href=\"style.css\" />" + htmlData;
// lets assume we have /assets/style.css file
webView.loadDataWithBaseURL("file:///android_asset/", htmlData, "text/html", "UTF-8", null);
file:///android_asset/就是工程中的assets目录

assets目录若不存在，需要手动创建，在AndroidStudio中就没有，跟res目录同级

collapsingToolbar在滑动时，标题会变？！！去掉onCreate()方法中对title的设置就行。

# 处理加入CSS后文章头部留出的一部分空白的问题。OK！

如何在app启动后再向RecyclerView中添加条目，是否还需要adapter。初步想法是在RecyclerViewAdapter类中添加一个方法，用来添加后续加载的文章列表。

知乎日报中的文章以日期来分开，已经实现的做法是，创建一个新的包含日期标题的layout，然后将需要显示日期标题的item的位置(position)添加到一个列表中，最后根据这个列表来判断是否选用包含日期标题的layout，因此也需要新建一个新的ViewHolder。并通过判断viewtype来选择加载哪个ViewHolder。

### 加载更多 SwipeRefreshLayout？

RecyclerView.LayoutManager 可以管理回收的item是否还对用户可见
RecyclerView.OnScrollListener 用来监听滑动RecyclerView时候的操作