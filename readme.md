[mxnet](https://github.com/dmlc/mxnet)训练自己的数据
-----

作为符号式编程和命令式编程的集大成者，mxnet是caffe强有力的替代者，但是由于参考资料很少，现在并没有大规模的普及开来。

虽然此前有文章讲解了如何使用mxnet跑mnist数据集和作画，但是我们需要的是跑自己的数据集，这儿dmlc做的并不是很系统，各个点分散在好几处，给人的感觉是很乱，没有重点：

[用MXnet实战深度学习之一:安装GPU版mxnet并跑一个MNIST手写数字识别](http://phunter.farbox.com/post/mxnet-tutorial1)

[用MXnet实战深度学习之二:Neural art](http://phunter.farbox.com/post/mxnet-tutorial2)

一直想找一个mxnet跑自己数据的教程，无奈没找到，只好自己造轮子了。整个工程结构如下图所示：

![structures](figures/structures.png)


其实无非就下面几个步骤：

1.安装

在linux下安装mxnet非常方便，直接make就好了，移植到anroid也费不了多少工夫，在windows下要麻烦一些，不过也是可以搞定的，在windows下安装mxnet，参见[mxnet VS2013编译.pdf](mxnet VS2013编译.pdf)文件，里面很详细的介绍了怎么编译安装mxnet0.5。

2.制作样本

chars文件夹是我从车牌识别的开源项目[easypr](https://github.com/liuruoze/EasyPR)中提取的数字0和1的字符样本，为了简单起见，我只提取了2类样本，其实一共有65类之多（字母+数字+汉字），不过只需要把图片文件拷贝过去就好，后面的操作基本一致。

双击generatetrain.bat就可以了，里面的内容很简单，无非是制作图片列表，然后用im2rec装换成它所需的格式，稍微注意下参数的含义，里面
python make_list.py chars chars/chars --recursive=True
"x64/release/im2rec" chars/chars.lst chars/ chars/train.rec

make_list.py后参数的含义是：数据存放的文件夹、所生成数据列表的文件名前缀（后缀默认是lst）、是否递归处理（也就是进入子文件夹）。
im2rec是自己编译生成的exe文件，后面参数含义是：上面生成的lst文件、数据所在文件夹、以及生成的记录文件rec。

3.定义网络

使用python可以很容易的定义所需的网络结构，无非就是几个symbol而已。

4.开始训练

双击打开VS2013的解决方案mxnettools.sln，设置mxnettools为启动项，charstrain.py为启动文件，Ctrl+F5运行即可，一点需要注意的是每次更换数据集别忘了把均值文件mean.bin删掉让它重新生成

![train](figures/train.png)

5.进行测试

charsbatchtest是用来批量测试的样本，为了简单起见，我提取了前20个样本作为测试集，实际应用中避免这样做。
制作测试集合训练集的过程大致相同，把charstest.py设为启动文件就可以测试了,经测试精度为100%（修正了之前的错误）。

![test](figures/test.png)

6.从文件夹进行测试

本次更新增加了从文件夹进行测试输出预测结果的脚本文件，见charsdirtest.py，结果保存在result.txt里，最开始输出类别名，然后输出每个文件预测的结果和最大概率。

7.工程化应用

charstest.py里面已经包含了用于预测输出的代码，提取需要的部分加入到自己的工程中去吧。

8.FAQ

Win7编译c++生成的测试例子不能运行，在win10下能正常运行，原因不明。

相关：
[MatConvNet使用指南,训练自己的数据](https://github.com/imistyrain/MatConvNet-mr)

很开心被[mxnet例子](https://github.com/dmlc/mxnet/tree/master/example)收录了，不过作者写错了。
