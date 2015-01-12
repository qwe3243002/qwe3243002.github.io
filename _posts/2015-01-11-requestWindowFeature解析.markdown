[截图]({{site.url}}assets/20150111204342.png)

Android官方文档中关于requestWindowFeature(int featureId)的介绍。（未完待续）
requestWindowFeature()必须在addContent之前调用，不然会报这个错误。

[截图]({{site.url}}/assets/20150111210342.png)

关于输入参数及产生情况，见下方列表(测试版本Android)。

1. DEFAULT_FEATURES（The default features enabled）

	系统默认情况，不解释。

	[截图]({{site.url}}/assets/S50111-205928.jpg)

2. FEATURE_ACTION_BAR

	显示ActionBar，在Android3.0之后引入，用于取代标题栏

	[截图]({{site.url}}/assets/S50111-211811.jpg)

3. FEATURE_ACTION_BAR_OVERLAY

	官网解释为ActionBar会覆盖住内容本身，但现在在手机上实验结果与官方不同，并没有覆盖住内容本身。

	[截图]({{site.url}}/assets/S50111-211811.jpg)

	解决方法：

		1. 导入ActionBarsherlock
		
		2. import com.actionbarsherlock.view.Window;

	[截图]({{site.url}}/assets/S50112-110616.jpg)

4. FEATURE_ACTION_MODE_OVERLAY
5. FEATURE_ACTIVITY_TRANSITIONS
6. FEATURE_NO_TITLE
	
