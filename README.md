# jquery.tag

演示地址：https://cnzhujie.github.io/jquery-tag/

## jquery.tag使用文档
在页面中引入相关js、css

```html
<script type="text/javascript" src="jquery.min.js"></script>
<script type="text/javascript" src="jquery.tag.js"></script>
<link rel="stylesheet" href="jquery.tag.css" type="text/css">
```

### 1、简单用法

**html:**

```html
<div id="testtag1" tagname="test1"></div>
```

注意当页面中有多个tag container时，可以指定tagname进行区分。若页面中只有一个，则tagname可以省略

**js:**
```javascript
$(document).ready(function(){
	$("#testtag1").tagsInit();
});
```

### 2、配置参数

<table cellspacing="0" cellpadding="5">
	<thead><tr>
		<th class="parameter">名称</th>
		<th class="type">类型</th>
		<th class="default">默认值</th>
		<th class="note">描述</th>
	</tr></thead>
	<tbody>
		<tr>
			<td>input</td>
			<td>boolean</td>
			<td>true</td>
			<td>是否创建标签input(在input上回车生成标签)</td>
		</tr>
		<tr>
			<td>inputNote</td>
			<td>string</td>
			<td>空</td>
			<td>显示在input上面的提示（例如'输入后请回车'）,只有input参数为true时才有效</td>
		</tr>
		<tr>
			<td>beforeInput</td>
			<td>function</td>
			<td>null</td>
			<td>input回车添加标签之前执行的函数,函数形式为:function beforeInput(tagValue){...}
			第一个参数是要添加的标签文字。当函数返回false的时候，表示拒绝添加此标签。
			</td>
		</tr>
		<tr>
			<td>afterInput</td>
			<td>function</td>
			<td>null</td>
			<td>input回车添加标签成功后执行的函数,函数形式为:function afterInput(tagValue){...}
			第一个参数是要添加的标签文字
			</td>
		</tr>
		<tr>
			<td>del</td>
			<td>boolean</td>
			<td>true</td>
			<td>是否可以删除标签</td>
		</tr>
		<tr>
			<td>onDel</td>
			<td>function</td>
			<td>null</td>
			<td>删除标签之前执行的函数,函数形式为:function onDel(tagValue,tagId){...}
			第一个参数是要删除的标签文字，第二个参数是要删除的标签id
			当函数返回false的时候，表示拒绝删除此标签。
			</td>
		</tr>
		<tr>
			<td>repeat</td>
			<td>boolean OR string</td>
			<td>true</td>
			<td>添加标签时候是否检测重复,false表示不检查，true和空字符串表示检查但不弹出对话框提示，非空字符串为检查且表示提示的内容</td>
		</tr>
		<tr>
			<td>onRepeat</td>
			<td>function</td>
			<td>null</td>
			<td>添加标签时候检测到重复标签时触发的函数，函数形式为function onRepeat(tagValue,tagId){}
			</td>
		</tr>
		<tr>
			<td>max</td>
			<td>number</td>
			<td>0</td>
			<td>此标签容器所能容纳的最大标签数目，0为不受限制</td>
		</tr>
		<tr>
			<td>action</td>
			<td>boolean OR object OR function</td>
			<td>false</td>
			<td>每个标签的点击动作。false：点击没反映。object{href:链接(%v%表示tagValue,%id%表示tagId),target:...}：打开链接。function：点击时执行此函数，函数参数为tagValue,tagId</td>
		</tr>
		<tr>
			<td>theme</td>
			<td>string</td>
			<td>blue</td>
			<td>css模版主题</td>
		</tr>
	</tbody>
</table>

### 3、高级用法

#### 3.1、添加之前进行判断，符合条件再添加

**html:**

```html
<div id="testtag2" tagname="test2"></div>
```

**js:**

```javascript
$("#testtag2").tagsInit({
	inputNote:"输入后回车",
	del:false,
	beforeInput:function(tagValue){
		if(tagValue=='aaa'){
			alert("不允许输入aaa");
			return false;
		}
	}
});
```

#### 3.2、给每个标签上加链接或者js函数

**html:**
```html
<div id="testtag3" tagname="test3"></div> <div id="testtag4" tagname="test4"></div>
```

**js:**
```javascript
$("#testtag3").tagsInit({
	repeat:'此标签列表中已经存在啦',
	action:{href:'http://www.baidu.com/s?wd=%v%',target:'_blank'}
});
$("#testtag4").tagsInit({
	onDel:function(){alert("不允许删除");return false;},
	action:function(tagValue){alert(tagValue);},
	theme:'black'
});
```

#### 3.3、添加标签的时候与后台交互

**html:**
```html
<div id="testtag5" tagname="test5"></div>
```

**js:**
```javascript
var tag=$("#testtag5").tagsInit({
	beforeInput:function(tagValue){
		$.post('http://www.baidu.com/s',{wd:tagValue},function(result){
			alert(result);
			tag.tagsAdd(tagValue);
		});
		return false;
	}
});
```

### 4、其他函数

#### 4.1、tagsAdd

通过此函数可以手动向标签容器添加标签
函数形式:tagsAdd=function(tagValue,tagId){...}

#### 4.2、tagsDel

通过此函数可以手动在标签容器中删除标签
函数形式:tagsDel=function(tagValue,tagId){...}

#### 4.3、tagsClear

通过此函数可以清空标签容器中的标签
函数形式:tagsClear=function(){...}

#### 4.4、tagsCount
通过此函数可以获取标签容器中标签的个数
函数形式:tagsCount=function(){...}

#### 4.5、tagsIndexof
通过此函数可以获取某标签在标签容器中的位置
函数形式:tagsIndexof=function(tagValue,tagId){...}。如果tagValue不为空，按照tagValue查找，否则按照tagId进行查找

#### 4.6、tagsStr
通过此函数可以将所有标签转换为字符串(通过split相隔)
函数形式:tagsStr=function(split){...}

#### 4.7、tagsJson
通过此函数可以将所有标签转换为json字符串
函数形式:tagsJson=function(){...}

#### 4.8示例

**html:**
```html
<div>
	<input type="button" onclick="mytagsAdd()" value="tagsAdd"/>
	<input type="button" onclick="mytagsDel()" value="tagsDel"/>
	<input type="button" onclick="mytagsClear()" value="tagsClear"/>
	<input type="button" onclick="mytagsCount()" value="tagsCount"/>
	<input type="button" onclick="mytagsIndexof()" value="tagsIndexof"/>
	<input type="button" onclick="mytagsStr()" value="tagsStr"/>
	<input type="button" onclick="mytagsJson()" value="tagsJson"/>
</div>
<div id="testtag6" tagname="test6"></div>
```

**js:**
```javascript
var tag6=$("#testtag6").tagsInit();
function mytagsAdd(){
	tag6.tagsAdd(prompt("输入添加内容"));
}
function mytagsDel(){
	tag6.tagsDel(prompt("输入删除内容"));
}
function mytagsClear(){
	tag6.tagsClear();
}
function mytagsCount(){
	alert(tag6.tagsCount());
}
function mytagsIndexof(){
	alert(tag6.tagsIndexof(prompt("输入查找内容")));
}
function mytagsStr(){
	alert(tag6.tagsStr());
}
function mytagsJson(){
	alert(tag6.tagsJson());
}
```
