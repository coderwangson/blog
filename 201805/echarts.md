使用Echarts、Servlet进行一个后台数据的前台统计显示

前几天需要做一个前台统计的页面，大概内容就是后台给数据到前台，然后前台使用柱状图或者饼状图、折线图进行渲染显示。如果自己写，这真是困难，但是后来发现了百度的一个开源前端组件Echarts（不得不说，有时候百度也做一些还可以的事情）。这个组件可以显示各种统计图。该组件链接在这里[Echarts官网](http://echarts.baidu.com/ "Echarts官网")。你可以在上面自行选择下载相应js文件，然后导入到你的项目中。

准备工作做完，就可以开始进行开发了，这个小demo使用了ajax技术、jquery技术以及servlet技术。所以要有这些基础才能看的懂。

1. 先引入相应js文件到项目中，然后建立一个jsp文件，并且引入js。
2. 在jsp的body中写入一下代码。

    <body>
    <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>

    <script type="text/javascript">
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.getElementById('main'));
      
     //这个option的json格式是官方提供的，只需要按照这个格式写就行了
    
      var option={
    		  tooltip: {
      show: true
      },
      legend: {
      data:['销量']
      },
      xAxis : [
      {
      type : 'category',
      
      }
      ],
      yAxis : [
      {
      type : 'value'
      }
      ],
      series : [
      {
      "name":"销量",
      "type":"bar",
      
      }
      ]
    };
    //调用load函数，用于从后台获得数据并且传给option中的xAxis以及series（这两个用于显示柱状图）
    load(option);
    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option);
    
    //利用ajax技术从后台获取数据并且给option
    function load(option){
    $.ajax({
       type : "post",
       async : false, //同步执行
       url : "/Echarts/Echarts",   //后台处理的servlet路径
       data : {},
       dataType : "json", //返回数据形式为json
       success : function(result) {  //如果ajax成功则后台json返回到result中
      if (result) {
     //初始化option.xAxis[0]中的data，就是给option中的xAxis加入data[]
      option.xAxis[0].data=[];
      for(var i=0;i<result.length;i++){
    option.xAxis[0].data.push(result[i].name);
      }
      //初始化option.series[0]中的data
      option.series[0].data=[];
      for(var i=0;i<result.length;i++){
    option.series[0].data.push(result[i].num);
      }
       }
    }
    });   
       }
    
    </script>
    
    </body>

3. 上面完成后就相当于完成了前台，但是前台的数据还是没有，所以需要创建一个servlet用于从数据库里面找数据，并且处理成json格式。  所以新建一个servlet，其映射地址和ajax中url要相同。
4. 在servlet中写一下代码

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
           //使用list模拟数据，这里就不从数据库中取了
    		List<Bar> list = new ArrayList<>();
          //这个Bar是一个实体类，用于封装一个柱状图的基本信息，即每个柱的名字和数量
    		Bar bar = new Bar();
    		bar.setName("衬衫");
    		bar.setNum(10);
    		list.add(bar);
    		bar = new Bar();
    		bar.setName("外套");
    		bar.setNum(20);
    		list.add(bar);
    	response.setContentType("text/html; charset=utf-8");
    //调用JSONArray.fromObject方法把array中的对象转化为JSON格式的数组
    //一定要导入相关jar包
    JSONArray json=JSONArray.fromObject(list);
    System.out.println(json.toString());
    //写数据到前台
    PrintWriter out = response.getWriter();  
    out.println(json);  
    out.flush();  
    out.close(); 
    }
    
其中Bar的代码如下
```

public class Bar {
	private String name;
	private Integer num;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Integer getNum() {
		return num;
	}
	public void setNum(Integer num) {
		this.num = num;
	}
	
}

```

做完上面的工作，整个项目就完成了，你部署之后就会发现
![](https://i.imgur.com/H4dNy3I.jpg)这个图片，就证明成功了。


总结：其实这个主要就是ajax技术和json技术要了解，因为前台的一些数据是要从后台获取，这里使用了ajax技术，而前后台数据的交换使用的单位就是json所以要对json串有所了解。
在在做完这种柱状图后，其他的什么折线图都能够做了。
优化部分：可以把load()函数提取成一个单独js，这样直接引入就行了，避免总是写。