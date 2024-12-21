# 1 模板语法继承

## 1.1 模板页面

* ### model_tmp.html

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <style>模板固定样式</style>
      {% block css %}
  	{% endblock %}
  </head>
  <body>
  <div>导航栏</div>	
  {% block content %}
  {% endblock %}
  <div>其他固定内容</div>
  <script>模板固定的js内容</script>
  {% block js %}
  {% endblock %}
  </body>
  </html>
  ~~~

  

## 1.2 模板继承

* ### index.html

  ~~~html
  {% extends "index/model_tmp.html" %}
  
  {% block css %}
      <style>
          #div-i1{
              width: 200px;
              height: 200px;
              background-color: orange;
              text-align: center;
          }
      </style>
  {% endblock %}
  
  {% block content %}
      <div class="container">
          
      </div>
  {% endblock %}
  ~~~

  

# 2 项目开端

## 2.1 表设计

* ### 部门表

  |  ID  | 部门名称 |
  | :--: | :------: |
  |  1   |  销售部  |
  |  2   |  财务部  |
  |  3   |  市场部  |

* ### 员工表

  |  ID  | 姓名 | 性别 | 年龄 |  部门  |
  | :--: | :--: | :--: | :--: | :----: |
  |      |      |      |      | 销售部 |
  |      |      |      |      |   1    |
  |      |      |      |      |   3    |

  ~~~python
  员工表部门字段
  	ID:   优势：节省开支  缺点：影响性能
  	名称： 优势：加快新能  缺点：加大开支
  员工表部门字段使用部门表
  	on_delete
  		级联删除： CASCADE，当部门表当中的部门删除之后，所有部门的员工全部删除
  		置空    ： SET_NULL, 当部门表当中的部门删除之后，所有部门的员工重新修改部门
  表字段属性
  null=True, blank=True
  	1、在做外键约束的时候，添加
      2、迁移完成后，增加字段也要添加
          
          
  员工表性别字段：
  	1：男， 2：女
  gender_choice = (
          (1, "男"),
          (2, "女")
      )
  gender = models.SmallIntegerField(verbose_name="性别", choices=gender_choice)
  ~~~




# 3 部门页面

## 3.1 删除数据

~~~python
depart/<int:nid>/del/

def depart_del(request, nid):
    models.Department.objects.filter(id=nid).delete()
    return redirect("/depart/")

<a href="/depart/{{ data.id }}/del/">
~~~

~~~python
depart/del/

def depart_del(request):
	nid = request.GET.get("nid")
	models.Department.objects.filter(id=nid).delete()
	return redirect("/depart/")

<a href="/depart/del/?nid={{ data.id }}">
<a href="http://127.0.0.1:8000/depart/del/?nid={{ data.id }}">

~~~

## 3.2 修改数据

~~~python
def depart_modify(request, nid):
    if request.method == "GET":
        title = "修改信息"
        name = models.Department.objects.filter(id=nid).first()
        print(name)
        return render(request, "depart/depart_add_modify.html", {'title': title, "name": name})

    title = request.POST.get("title")
    models.Department.objects.filter(id=nid).update(title=title)
    return redirect('/depart/')
~~~

# 4 员工页面

## 4.1 信息展示

* ### 部门信息展示

  ~~~html
  <-->性别展示</-->
  <-->get_字段_display</-->
  <td>{{ data.get_gender_display }}</td>
  
  <-->部门展示</-->
  <td>{{ data.depart.title }}</td>
  
  <-->日期展示</-->
  <td>{{ data.create_time|date:"Y-m-d" }}</td>
  ~~~

  

## 4.2 信息添加

* ### user_add.py

  ~~~python
  def user_add(request):
      if request.method == "GET":
          content = {
              "gender_choices": models.Userinfo.gender_choice,
              "depart_list": models.Department.objects.all()
          }
          return render(request, "user/user_add.html", content)
      name = request.POST.get("name")
      age = request.POST.get("age")
      gender = request.POST.get("gender")
      salary = request.POST.get("salary")
      dtime = request.POST.get("dtime")
      depart = request.POST.get("depart")
      models.Userinfo.objects.create(name=name, age=age, gender=gender,salary=salary, create_time=dtime,depart_id=depart)
      return redirect("/user/")
  ~~~

  

* ### user_add.html

  ~~~html
  <-->常规写法</-->
  <div class="form-group">
      <label for="exampleInputEmail1">收入</label>
      <input type="email" class="form-control" placeholder="请输入收入">
  </div>
  <-->常规写法</-->
  <-->引入lauyi当中的css，js，font</-->
  <div class="form-group">
      <label for="exampleInputEmail1">入职日期</label>
      <input type="email" class="form-control" id="d_time" placeholder="请输入入职日期">
  </div>
  <script>
  layui.use(function () {
      var laydate = layui.laydate;
      // 渲染
      laydate.render({
          // 将日期框的id复制进来即可
          elem: '#d_time'
      });
          });
  </script>	
  
  <-->特殊写法</-->
  <div class="form-group">
      <label for="exampleInputEmail1">性别</label>
      <select name="gender" class="form-control" >
          {% for item in gender_choices %}
          <option value="{{ item.0 }}">{{ item.1 }}</option>
          {% endfor %}
      </select>
  </div>
  <div class="form-group">
      <label for="exampleInputEmail1">部门</label>
      <select name="depart" class="form-control">
          {% for item in depart_list %}
          <option value="{{ item.id }}">{{ item.title }}</option>
          {% endfor %}
      </select>
  </div>
  
  ~~~

  

# 5 ModelForm

## 5.1 优势

* ## 传统方式

  * 1、没有做数据校验
  * 2、数据表字段很多，每个字段都要手写

## 5.2 信息添加

* ## views.py

  ~~~python
  def user_add_modelform(request):
      if request.method == "GET":
          title = "添加信息2.0"
          form = UserModelForm()
          return render(request, "user/user_add_modelform.html", {"form": form, "title":title})
  
      # 接受表单提交过来的数据
      form = UserModelForm(data=request.POST)
      # 校验数据是否完整
      if form.is_valid():
          # 存储到数据库
          form.save()
          return redirect("/user/")
      # 如果数据不完整，则返回当前添加页面，展示错误信息
      return render(request, "user/user_add_modelform.html", {"form": form})
  ~~~

*  ## user_modelform.html 添加和修改公用

  ~~~html
  <form method="post" novalidate>
      {% csrf_token %}
      {% for filed in form %}
          <label for="exampleInputEmail1">{{ filed.label }}</label>
          	{{ filed }}
      	<-->只有在数据校验不完整的时候，filde当中才有error</-->
          <span style="color: red">{{ filed.errors.0 }}</span><br>
      {% endfor %}
      <button type="submit" class="btn btn-success">提交</button>
  </form>
  ~~~

* ## 补充

  ~~~python
  class Demo():
      def __init__(self):
          self.name = "张三"
  
      def func(self):
          return "我是func"
  
      def __str__(self):
          return "我是类的返回值"
  
  abc = Demo()
  print(abc)
  ~~~

## 5.3 信息修改

~~~python
def user_modify(request, nid):
    obj = models.Userinfo.objects.filter(id=nid).first()
    if request.method == "GET":
        title = "修改信息2.0"
        form = UserModelForm(instance=obj)
        content = {
            "form": form,
            "title": title
        }
        return render(request, "user/user_modelform.html", content)

    form = UserModelForm(data=request.POST, instance=obj)
    if form.is_valid():
        form.save()
        return redirect("/user/")
    # 如果数据不完整，则返回当前添加页面，展示错误信息
    return render(request, "user/user_modelform.html", {"form": form})
~~~

# 6 资产管理

## 6.1 信息添加

*   ### 批量添加

    ~~~python
    import random
    for num in range(200):
        mobile = "153" + str(random.randint(11111111, 99999999))
        status = random.randint(1, 2)
        times = "2024-12-24"
        price = random.randint(5000, 8000)
        user_id = random.randint(1, 2)
        models.Assets.objects.create(mobile=mobile, status=status, times=times, price=price, user_id=user_id)
    ~~~

    

*   ### 数据前端校验

    ~~~python
    mobile = forms.CharField(
            label="手机号",
            validators=[RegexValidator(r"^1[3-9]\d{9}", "请输入正确格式的手机号")]
        )
    ~~~

*   ### 数据表当中的校验

    ~~~python
    # 钩子方法 clean_字段名
    # def clean_mobile(self):
        def clean_mobile(self):
            # print(self.cleaned_data) 以字典的形式，获取当前字段
            new_mobile = self.cleaned_data["mobile"]
            # 在数据表当中判断是否存在
            # .exists() 返回布尔值，判断当前查询的结果是否存在
            exists = models.Assets.objects.filter(mobile=new_mobile).exists()
            if exists:
                # 自定义异常
                raise ValidationError("手机号已经存在")
            # 如果没有return，浏览器会认为当前窗口没有数据
            return new_mobile
    ~~~

## 6.2 信息展示

*   ### 不同状态，在html当中显示不同颜色

    ~~~html
    {% if data.status == 1 %}
    	<td style="color: green;">{{ data.get_status_display }}</td>
    {% else %}
    	<td style="color: red;">{{ data.get_status_display }}</td>
    {% endif %}
    ~~~

    

    

## 6.3 信息修改

*   ### 修改数据的时候，避免当前id的电话号码和数据表当中的相同id的数据做对比

    ~~~python
    # 当前字段禁止编辑
    price = forms.CharField(disabled=True, label="手机号")
    ~~~

    

    ~~~python
    def clean_mobile(self):
    	# print(self.cleaned_data) 以字典的形式，获取当前字段
    	new_mobile = self.cleaned_data["mobile"]
    	# 修改数据，提交之后，他不会和自己去做比较
    	# exclude(id=self.instance.pk) 修改id为1的数据，则校验的时候，排除id为1的数据
    	exists = models.Assets.objects.exclude(id=self.instance.pk).exists()
    	if exists:	
    		raise ValidationError("手机号已经存在")
    	return new_mobile
    ~~~


# 7 搜索与翻页

## 7.1 搜索

*   传统的搜索方式

    ~~~python
    models.Assets.objects.filter(mobile="输入的mobile")
    
    dict_data= {"mobile":"搜索的内容"}
    # **代表所有数据，如果dict_data有条件，则返回与条件相符的数据
    # 如果dict_data没有条件，则返回全部的数据
    models.Assets.objects.filter(**dict_data)
    ~~~

*   ### 针对整数

    ~~~python
    # 价格等于100
    models.Assets.objects.filter(price=100)
    
    # 价格大于100
    models.Assets.objects.filter(price__gt=100)
    # 价格大于等于100
    models.Assets.objects.filter(price__gte=100)
    
    # 价格小于100
    models.Assets.objects.filter(price__lt=100)
    # 价格小于等于100
    models.Assets.objects.filter(price__lte=100)
    ~~~

*   ### 针对字符串

    ~~~python
    # 以135开头
    models.Assets.objects.filter(mobile__startswidth=135)
    
    # 以135结尾
    models.Assets.objects.filter(mobile__endswidth=135)
    
    # 包含135
    models.Assets.objects.filter(mobile__contains=135)
    ~~~

*   ### 搜索

    ~~~python
    def asset_list(request):
        dict_data = {}
        # 获取搜索框当中的信息
        value = request.GET.get("search")
        if value:
            # 搜索mobile字段当中，包含value的数据
            dict_data["mobile__contains"] = value
    
        data_asset_list = models.Assets.objects.filter(**dict_data)
        return render(request, "assets/asset_list.html", {"data_asset_list": data_asset_list})
    ~~~

## 7.2 翻页

*   ### 基本逻辑

    ~~~python
    # 假设每一页展示10条数据
    data_asset_list = models.Assets.objects.filter(**dict_data)[0: 10]     1
    data_asset_list = models.Assets.objects.filter(**dict_data)[10: 20]    2
    data_asset_list = models.Assets.objects.filter(**dict_data)[20: 30]    3
    ~~~

    
