# Sirius 组件模型工具构建

## Sirius Demo

> 参考链接https://wiki.eclipse.org/Sirius/Tutorials/StarterTutorial，视频<https://www.youtube.com/channel/UCKQFNFA2hoLTrilWlmfO_7w>

1. [Eclipse J2EE 64位](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2019-03/R/eclipse-jee-2019-03-R-win32-x86_64.zip) + Java 8 

2. 打开Eclipse，在 Eclipse Marcket 中安装Ecoretools diagram editor 4.0工具和 Sirius 6.1环境（选项默认）

3. File > New > Example，选择Basic Family Metamodel Definition, 并创建

4. 在org.eclipse.sirius.sample.basicfamily的run configuration中新建Eclipse Application的默认配置（右键org.eclipse.sirius.sample.basicfamily项目 > run as > run configurations，弹出配置窗口左边栏双击Eclipse Application新建配置）并运行。

5. 在新的Eclipse siruis窗口中右上角切换sirius视图

6. 新建sample model，File > New > Example，选择*Basic Family Sample Model*，导入该模型。![1559029027394](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559029027394.png)此模型包含已有的对象（组件实例）和他们的连接关系，以xml格式打开**example.basicfamily**可发现：![1559029235148](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559029235148.png)而**representation.aird**文件用来描述更多视图信息如size、font、color、location等，点击可打开元素连接视图

7. 右键Model Explorer空白处新建*Viewpoint Specification Project*，*Viewpoint Specification Project* 包含了建模工作台的定义![1559032034650](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559032034650.png)在persons中包含了视图中各个元素的表示方式

8. 在上面项目中的 *MANIFEST.MF* 文件dependency中指定相关联的元模型![1559032472265](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559032472265.png)、

9. 在persons中创建元素或对象的图像表达**Diagram Description**![1559032590403](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559032590403.png)并在此diagram的属性metamodel和general中指定与其关联的元模型basicfamily，和Domain Class（对应元模型中的类）

10. 右键Default新建Node，并指定其ID和所属Domain Class，右键New Style选择节点样式

11. 右键Default新建Relation Based Edge，并在属性中指定约束![1559033948581](C:\Users\Hyu\AppData\Roaming\Typora\typora-user-images\1559033948581.png)

    



## 创建元模型并生成（导入）编辑器

1. 使用Ecores工具创建OSAI组件元模型。参考链接：https://wiki.eclipse.org/Sirius/Tutorials/DomainModelTutorial

   另附编辑视图样式参考链接：https://wiki.eclipse.org/Sirius/Tutorials/AdvancedTutorial

2. 安装Ecores工具
   1. *Help/Install New Software* 点击*add* , 填入Name：Ecores Tools，Location：https://www.eclipse.org/ecoretools/download.html
   2. 安装好之后创建模型，*New->Other->Eclipse Modeling Project->Ecore Modeling Project* 

3. 拖拽创建元模型，添加各类的属性

4. 修改*Properties/Ns URI* 和 genmodel中package的*Base Package* 名称和命名路径一致。

5. 右键元模型视图空白处右键 *Generate->All*, 将生成对应的edit和editor项目

6. *Run as -> Eclipse Application*

7. 切换sirius视图后新建Modeling Project，在该Project上右键*New->Other->Example EMF Modeling Creation Wizards->之前创建的元模型* ，并选择根节点，右键改节点上右键可以创建子节点，并在Properties中编辑属性。

8. 根据上文中的方式定义各个元素的representation和style，在interpreter中选择Sirius解释器。

9. 双击*representations.aird->Representation->New* 选择根节点representation。

10. 右键Modeling Project并选择ViewPoints，右键根节点并新建diagram



### Palladio Project

这是sirius gallery中的开源项目，基于sirius实现的组建模型编辑器

1. 下载：https://www.palladio-simulator.com/tools/download/

   源码：https://www.palladio-simulator.com/tools/source_code/

2. 源码下载使用svn，[TortoiseSVN](http://tortoisesvn.net/downloads)使用方法链接：https://blog.csdn.net/zhangdong305/article/details/46444899