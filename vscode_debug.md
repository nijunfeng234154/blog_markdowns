## VScode Debug方法

我们首先需要配置一个launch.json文件，在左侧边栏运行按钮里进行创建，vscode会在项目文件夹下创建一个.vscode文件夹，里面放了launch.json文件

<img src="C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240826181732884.png" alt="image-20240826181732884" style="zoom: 80%;" />

那么如何去写这个launch.json文件，我们需要重点关注加参数的json列写法

![image-20240826182240572](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240826182240572.png)

那么整体上配置运行debug的环境就解决了

然后我们在上面框里输>python 选择python解释器 来配置python运行环境

如果有conda环境或者python虚拟环境可以直接选

之后一定要点击左边侧边栏debug里的绿色按钮来debug才能应用launch.json里的内容

![image-20240827172500968](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172500968.png)

继续 直接执行到断点那一行暂停

![image-20240827172243747](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172243747.png)

逐过程 有函数不进入 执行一行

![image-20240827172321815](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172321815.png)

单步调试 可以进函数内部一行行执行

![image-20240827172337170](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172337170.png)

单步跳出 从当前函数跳出

![image-20240827172352931](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172352931.png)

编译按钮界面左下可以看被暂停的进程状态，左上可以看各个变量的内容

![image-20240827172439237](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240827172439237.png)

### 使用vscode的全局字符串搜索功能

![image-20240830175346698](C:\Users\nijunfeng\AppData\Roaming\Typora\typora-user-images\image-20240830175346698.png)

左侧边栏第二个，可以直接搜字符串找变量名进行debug