> 这篇文章通过对话的形式，由浅入深带你读懂 AVL 树，看完让你保证理解 AVL 树的各种操作，如果觉得不错，别吝啬你的赞哦。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109201442423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/201911092015241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/201911092015445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110920155745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

1、若它的左子树不为空，则左子树上所有的节点值都小于它的根节点值。



2、若它的右子树不为空，则右子树上所有的节点值均大于它的根节点值。



3、它的左右子树也分别可以充当为二叉查找树。



例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109201621919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109201745974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109201804986.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

例如，我现在想要查找数值为14的节点。由于二叉查找树的特性，我们可以很快着找到它，其过程如下：



1、和根节点9比较
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202001597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
2、由于 14 > 9，所以14只可能存在于9的右子树中，因此查看右孩子13
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202033230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

3、由于 14 > 13，所以继续查看13的右孩子15
		![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202058265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
4、由于 14 < 15，所以14只可能存在于15的左孩子中，因此查找15的左孩子14
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202128725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

5、这时候发现14正是自己查找的值，于是查找结束。



这种查找二叉树的查找正是二分查找的思想，可以很快着找到目的节点，查找所需的最大次数等同于二叉查找树的高度。



在插入的时候也是一样，通过一层一层的比较，最后找到适合自己的位置。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202157662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110920221075.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202217214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
初始的二叉查找树只有三个节点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202230413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
然后我们按照顺序陆续插入节点 4，3，2，1，0。插入之后的结构如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110920224617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202324483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202337561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202347391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202355595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202413850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
这是一种比查找二叉树还特别的树哦，这种树就可以帮助我们解决二叉查找树刚才的那种所有节点都倾向一边的缺点的。具有如下特性：



1. 具有二叉查找树的全部特性。
2. 每个节点的左子树和右子树的高度差至多等于1。



例如：图一就是一颗AVL树了，而图二则不是(节点右边标的是这个节点的高度)。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202455617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202504220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
对于图二，因为节点9的左孩子高度为2，而右孩子高度为0。他们之间的差值超过1了。

这种树就可以保证不会出现大量节点偏向于一边的情况了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202532518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)

听起来这种树还不错，可以对于图1，如果我们要插入一个节点3，按照查找二叉树的特性，我们只能把3作为节点4的左子树插进去，可是插进去之后，又会破坏了AVL树的特性，那我们那该怎么弄？

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202711741.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
#### 右旋



我们在进行节点插入的时候，可能会出现节点都倾向于左边的情况，例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202732131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
我们把这种倾向于左边的情况称之为 左-左型。这个时候，我们就可以对节点9进行右旋操作，使它恢复平衡。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202759355.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
即：**顺时针旋转两个节点，使得父节点被自己的左孩子取代，而自己成为自己的右孩子**



再举个例子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202819580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
节点4和9高度相差大于1。由于是左孩子的高度较高，此时是左-左型，进行右旋。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109202837152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
这里要注意，节点4的右孩子成为了节点6的左孩子了



我找了个动图，尽量这个动图和上面例子的节点不一样。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203211331.gif)

#### 左旋



左旋和右旋一样，就是用来解决当大部分节点都偏向右边的时候，通过左旋来还原。例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110920324235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
我们把这种倾向于右边的情况称之为 右-右型。



我也找了一张动图。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110920332691.gif)
#### 例子讲解

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203356364.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203403556.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
初始状态如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203418270.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
然后我们主键插入如下数值：1,4,5,6,7,10,9,8



插入 1
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203430892.png)
左-左型，需要右旋调整。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203452129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
插入4
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203506215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
继续插入 5
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203520958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
右-右型，需要左旋转调整。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203537453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
继续插入6
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203550343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
右-右型，需要进行左旋
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203604442.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
继续插入7
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203617776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
右-右型，需要进行左旋
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203632198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
继续插入10
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203643702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
继续插入9
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203655956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
出现了这种情况怎么办呢?对于这种  右-左型 的情况，单单一次左旋或右旋是不行的，下面我们先说说如何处理这种情况。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203712400.png)
这种我们就把它称之为 右-左 型吧。处理的方法是先对节点10进行右旋把它变成右-右型。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203730371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
然后在进行左旋。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203744453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
所以对于这种 右-左型的，我们需要进行一次右旋再左旋。



同理，也存在 左-右型的，例如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203757412.png)
对于左-右型的情况和刚才的 右-左型相反，我们需要对它进行一次左旋，再右旋。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203815183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
回到刚才那道题
![在这里插入图片描述](https://img-blog.csdnimg.cn/201911092038273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
对它进行右旋再左旋。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203840588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
到此，我们的插入就结束了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203856460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203904502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
#### 总结一下



在插入的过程中，会出现一下四种情况破坏AVL树的特性，我们可以采取如下相应的旋转。



1、左-左型：做右旋。

2、右-右型：做左旋转。

3、左-右型：先做左旋，后做右旋。

4、右-左型：先做右旋，再做左旋。



不知道大家发现规律没，这个规则还是挺好记。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203924990.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203939943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203950122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109203958883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)
#### 代码实现

```java
//定义节点
class AvlNode {
   int data;
   AvlNode lchild;//左孩子
   AvlNode rchild;//右孩子
   int height;//记录节点的高度
}

//在这里定义各种操作
public class AVLTree{
   //计算节点的高度
   static int height(AvlNode T) {
       if (T == null) {
           return -1;
       }else{
           return T.height;
       }
   }

   //左左型，右旋操作
   static AvlNode R_Rotate(AvlNode K2) {
       AvlNode K1;

       //进行旋转
       K1 = K2.lchild;
       K2.lchild = K1.rchild;
       K1.rchild = K2;

       //重新计算节点的高度
       K2.height = Math.max(height(K2.lchild), height(K2.rchild)) + 1;
       K1.height = Math.max(height(K1.lchild), height(K1.rchild)) + 1;

       return K1;
   }

   //进行左旋
   static AvlNode L_Rotate(AvlNode K2) {
       AvlNode K1;

       K1 = K2.rchild;
       K2.rchild = K1.lchild;
       K1.lchild = K2;

       //重新计算高度
       K2.height = Math.max(height(K2.lchild), height(K2.rchild)) + 1;
       K1.height = Math.max(height(K1.lchild), height(K1.rchild)) + 1;

       return K1;
   }

   //左-右型，进行左旋，再右旋
   static AvlNode R_L_Rotate(AvlNode K3) {
       //先对其孩子进行左旋
       K3.lchild = R_Rotate(K3.lchild);
       //再进行右旋
       return L_Rotate(K3);
   }

   //右-左型，先进行右旋，再左旋
   static AvlNode L_R_Rotate(AvlNode K3) {
       //先对孩子进行右旋
       K3.rchild = L_Rotate(K3.rchild);
       //在左旋
       return R_Rotate(K3);
   }

   //插入数值操作
   static AvlNode insert(int data, AvlNode T) {
       if (T == null) {
           T = new AvlNode();
           T.data = data;
           T.lchild = T.rchild = null;
       } else if(data < T.data) {
           //向左孩子递归插入
           T.lchild = insert(data, T.lchild);
           //进行调整操作
           //如果左孩子的高度比右孩子大2
           if (height(T.lchild) - height(T.rchild) == 2) {
               //左-左型
               if (data < T.lchild.data) {
                   T = R_Rotate(T);
               } else {
                   //左-右型
                   T = R_L_Rotate(T);
               }
           }
       } else if (data > T.data) {
           T.rchild = insert(data, T.rchild);
           //进行调整
           //右孩子比左孩子高度大2
           if(height(T.rchild) - height(T.lchild) == 2)
               //右-右型
               if (data > T.rchild.data) {
                   T = L_Rotate(T);
               } else {
                   T = L_R_Rotate(T);
               }
       }
       //否则，这个节点已经在书上存在了，我们什么也不做
       
       //重新计算T的高度
       T.height = Math.max(height(T.lchild), height(T.rchild)) + 1;
       return T;
   }
}
```

学习更多**算法** + **计算机基础知识**，欢迎关注我的微信公众号，每天准时推送技术干货

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223728524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)



