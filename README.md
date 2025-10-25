[合集 - Blazor入门到实战(3)](https://github.com)

[1.ASP.NET Core Blazor简介和快速入门二（组件基础）10-20](https://github.com/shenchuanchao/p/19151711)[2.ASP.NET Core Blazor简介和快速入门一（基础篇）10-15](https://github.com/shenchuanchao/p/19142586)

3.ASP.NET Core Blazor简介和快速入门三（布局和路由）10-25

收起

​大家好，我是[码农刚子](https://github.com)。本文介绍了Blazor中的布局、路由和条件渲染功能。在布局方面，详细讲解了如何创建和应用布局组件（继承LayoutComponentBase），包括默认布局MainLayout的使用、嵌套布局的实现方式以及如何控制特定页面不应用布局（如登录页）。在路由和导航部分，简要提及了基本配置方法。最后，重点阐述了条件渲染（@if语句）和循环渲染（@foreach等）的语法和实际应用场景，通过学生信息列表等示例展示了数据绑定和动态UI生成的实现方式。这些核心功能共同构成了Blazor组件化开发的基础框架。

## 一、创建和应用Blazor 布局

网站应用往往有许多公共的视图部分，比如顶部导航nav,底部的footer，管理系统的左边的menu菜单等等。Layout可以轻松实现以上的效果。

Blazor 布局是一个 Razor 组件，它与引用该布局的组件共享标记。 布局可以使用数据绑定、依赖关系注入和组件的其他功能。

### 1、创建布局（可以理解为母版页）

新建一个razor文件，文件顶部使用@inherits LayoutComponentBase 表示继承自LayoutComponentBase ，说明这是个母版页，使@Body作为占位。

![](https://i-blog.csdnimg.cn/img_convert/17db26a2fc59811f252472259a25564c.png)​

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")编辑

```
@inherits LayoutComponentBase

Doctor Who® Database



# Doctor Who® Database




    Main Episode List
    Search
    Add Episode


@Body


    @TrademarkMessage


@code {
    public string TrademarkMessage { get; set; } ="CSharp精选营";
}
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

### 2、MainLayout 组件

在从 Blazor 项目模板创建的应用中，`MainLayout` 组件就是应用的默认布局。

Blazor 的 CSS 隔离功能将独立 CSS 样式应用于 `MainLayout` 组件。 按照惯例，样式由相同名称的随附样式表 `MainLayout.razor.css` 提供。

![](https://i-blog.csdnimg.cn/img_convert/4e72d0845945faae717cbd8bc4e4dc47.png)​

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")编辑

![](https://i-blog.csdnimg.cn/img_convert/e2f8774f69788dd6c9469c6be0cee86a.png)​

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")编辑

### 3、应用布局

在razor文件顶部申明“@layout 模板页名”来设置母版页，

![](https://i-blog.csdnimg.cn/img_convert/bc1edcdda470ec5d61fc422465d5e24e.png)​

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")编辑

如果一个页面没有设置模板页，（就像blazor默认项目那样），他就会使用在app.razor文件中定义的默认模板页

![](https://i-blog.csdnimg.cn/img_convert/07c69bc84df89396eb47a0f95384f6b9.png)​

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")编辑

**如何****不设置任何模板页?**

比如我们的登录的页面是不需要加通用模板的，通过@if 控制 RouteView的DefaultLayout即可

```
	
		@if (routeData.PageType == typeof(Pages.LayoutSample))
		{
			
		}
		else
		{
			
			
		}
	
	
		Not found
		

Sorry, there's nothing at this address.


	
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

### 4、嵌套布局

组件可以引用一个布局，该布局又可以引用另一个布局。 例如，嵌套布局可用于创建多级菜单结构。

以下示例演示如何使用嵌套布局:

```
@inherits LayoutComponentBase
@layout ProductionsLayout

Doctor Who® Database


# Doctor Who® Database



    Main Episode List
    Search
    Add Episode


@Body


    @TrademarkMessage


@code {
    public string TrademarkMessage { get; set; } =
        "CSharp精选营";
}
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

`ProductionsLayout` 组件包含顶级布局元素，其中包含有标头 (`...`) 和页脚 (`...`) 元素。 带有`DoctorWhoLayout`组件的 `Episodes` 会在`@Body`出现的位置显示。

```
@inherits LayoutComponentBase



# Productions




    Main Production List:nuts坚果
    Search
    Add Production


@Body


    Footer of Productions Layout
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

以下呈现的 HTML 标记由前面的嵌套布局生成:

```
# Productions


Main Production List
    Search
    Add Production



# Doctor Who® Database


Main Episode List
    Search
    Add Episode

CSharp精选营

    Footer of Productions Layout
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

更多参考：

[https://learn.microsoft.com/zh-cn/aspnet/core/blazor/components/layouts?view=aspnetcore-9.0](https://learn.microsoft.com/zh-cn/aspnet/core/blazor/components/layouts?view=aspnetcore-9.0 "https://learn.microsoft.com/zh-cn/aspnet/core/blazor/components/layouts?view=aspnetcore-9.0")

## 二、路由配置和导航

**[ASP.NET Core Blazor 路由配置和导航 - 码农刚子 - 博客园](https://github.com)**

## 三、条件渲染和循环渲染

第一章中已经讲过了Blazor的语法。

### 1、Blazor 条件渲染

Blazor 中的 @if 语法用于根据条件动态渲染页面元素。它类似于 C# 的 if 语句，但专门用于处理 UI 渲染。请看以下示例：

```
@if (isLoading)
{

加载中...

}
else
{

加载完成！

}
@code {
   private bool isLoading = true;
   protected override void OnInitialized()
   {
       // 模拟加载完成
       Task.Delay(2000).ContinueWith(_ =>
       {
           isLoading = false;
           InvokeAsync(StateHasChanged);
       });
   }
}
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

你可以嵌套多个 @if 或结合 else if 使用：

```
@if (userRole == "Admin")
{

欢迎管理员！

}
else if (userRole == "User")
{

欢迎普通用户！

}
else
{

请登录。

}
@code {
   private string userRole = "Admin";
}
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

### 2、Blazor 循环渲染

下面我们有一个list需要显示多个学生信息,@for,@do…while,@while 与foreach类似这里就不在赘述

```
### 用户列表



| Name | Age | Gender |
| --- | --- | --- |
@foreach (var item in list)
		{| @item.Name | @item.Age |@switch (item.Gender)
				{
					case 0:
						{ 男 |break;
						}
					case 1:
						{ 女 |break;
						}
					case 2:
						{ 未知 |break;
						}

				}
}

@code {
	List list = new List();
	User User1 = new User()
		{
			Name = "John",
			Age = 20,
			Gender = 2,
		};
	User User2 = new User()
		{
			Name = "Sub",
			Age = 22,
			Gender = 0,
		};
	protected override void OnInitialized()
	{
		list.Add(User1);
		list.Add(User2);
	}

	public class User
	{
		public string Name { get; set; } = string.Empty;
		public int Age { get; set; }
		public int Gender { get; set; } // 0: 男, 1: 女, 2: 未知
		
	}
}
```

![](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025181716895-51938865.gif "点击并拖拽以移动")

![image](https://img2024.cnblogs.com/blog/1466017/202510/1466017-20251025182254148-1718218348.png)

 以上就是《ASP.NET Core Blazor简介和快速入门三（布局和路由）》的所有内容，感谢你的阅读，希望对你有所收获。如果可用的话，给我点个赞👍吧。
