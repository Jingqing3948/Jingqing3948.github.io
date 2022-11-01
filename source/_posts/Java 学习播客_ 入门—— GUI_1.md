java 也可以在前端进行开发，使用 AWT 和 SWING 完成图形化界面编程。

# AWT

Abstract Window Toolkit 抽象窗口工具集。它是抽象的，程序只指定某个模块的位置和行为，具体实现（样式等）是由操作系统完成。

主要包括两个基类： Component 和 MenuComponent。其中 Component 中的 Container 组件可以作为容器，包含其他组件。

![image-20220513151527543](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220513151527543.png)

此外，AWT 还有一个比较重要的知识点 LayoutManager，负责处理布局。

![image-20220513151540412](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220513151540412.png)

## Component

| 方法                                           | 说明               |
| ---------------------------------------------- | ------------------ |
| setLocation(int x, int y)                      | 设置组件位置       |
| setSize(int width, int height)                 | 设置组件大小       |
| setBounds(int x, int y, int width, int height) | 同时设置位置和大小 |
| setVisible(Boolean b)                          | 设置该组件的可见性 |

### Window

Frame 是继承自 Window 的类。

```java
Frame frame=new Frame("标题");
frame.setLocation(100,100);//位置
frame.setSize(500,300);//宽高
frame.setVisible(true);
```

现在的窗口是没法点击x号关闭的，需要手动停止运行才能结束。

### Container

| 方法                                   | 说明                   |
| -------------------------------------- | ---------------------- |
| Component add(Component comp)          | 向该组件中添加其他组件 |
| Component getComponentAt(int x, int y) | 返回指定位置的组件     |
| int getComponentCount()                | 返回该容器中组件的数量 |
| Component[] getComponents()            | 返回该容器中所有的组件 |

容器不能单独存在，必须依附于 Window 存在。

编写时，先添加 window 窗口，然后添加容器，再在容器中添加组件，最后把容器添加到 Window 中，调整 Window 位置。自己也可以写组件。

#### Panel

```java
//1. 创建 window 对象
Frame myFrame=new Frame();
//2. 创建 Panel 对象
Panel p=new Panel();
//创建文本框和按钮，放入 Panel 对象
p.add(new TextField("测试文本"));
p.add(new Button("测试按钮"));
//出现乱码的话，左上角 PanelTry-添加 VM 选项-输入 -Dfile.encoding=gbk
myFrame.add(p);
myFrame.setBounds(100,100,500,300);
myFrame.setVisible(true);
```

#### ScrollPane

元素过多时会添加滚动条。

```java
Frame frame=new Frame("ScrollPane");
ScrollPane sp=new ScrollPane(ScrollPane.SCROLLBARS_ALWAYS);//默认是总是有滚动条
sp.add(new TextField("测试文本"));
sp.add(new Button("测试按钮"));
frame.add(sp);
frame.setBounds(100,100,500,300);
frame.setVisible(true);
```

但是实际上按钮把整个页面占满了。测试文本并没有出现。这就涉及到布局管理器的知识了。

## LayoutManager

相比手动在不同平台上设置各个组件的大小，LayoutManager 会根据不同平台自动调整组件大小，使得程序员即便在不同的平台上也可以做到相同的布局使用体验。

*最佳大小：组件刚好和内容一样大小。通过 pack() 方法设置。*


### FlowLayout

组件像水流一样流动（排列），碰到障碍（边界）就会折回重新开始。

| 构造方法                                  | 说明                                   |
| ----------------------------------------- | -------------------------------------- |
| FlowLayout()                              | 使用默认的对齐方式、垂直间距、水平间距 |
| FlowLayout(int align)                     | 设置对齐方式（常量 LEFT RIGHT CENTER） |
| FlowLayout(int align, int hgap, int vgap) | 设置对齐方式、垂直间距、水平间距       |

```java
Frame f=new Frame("Float 测试");
f.setLayout(new FlowLayout(FlowLayout.LEFT,20,20));
for(int i=1;i<=100;i++){f.add(new Button("按钮"+i));}
f.pack();//最佳大小设置
f.setVisible(true);
```

### BorderLayout

分为5各部分，东西南北中。

往 BorderLayout 中添加组件需要指明放在了哪个区域中。默认在 CENTER 里。

如果往一个区域中添加了多个组件，后添加的会覆盖新添加的。

*因此想如果往一个区域中添加多个组件，不能一个个 add 到这个区域。可以把他们都 add 到容器中，然后把容器 add 到那个区域。*

| 构造方法                         | 说明                 |
| -------------------------------- | -------------------- |
| BorderLayout()                   |                      |
| BorderLayout(int hgap, int vgap) | 指定水平垂直间距创建 |

```java
Frame f=new Frame("BorderLayoutTry");
f.setLayout(new BorderLayout(30,10));
f.add(new Button("北侧按钮"),BorderLayout.NORTH);
f.add(new Button("南侧按钮"),BorderLayout.SOUTH);
f.add(new Button("中侧按钮"),BorderLayout.CENTER);
f.add(new Button("西侧按钮"),BorderLayout.WEST);
f.add(new Button("东侧按钮"),BorderLayout.EAST);
f.pack();
f.setVisible(true);
```

没添加内容的区域会被其他区域占用。如西侧没添加，中间的部分就会靠左占掉西侧。

可以和其他排版结合使用~

### GridLayout

将容器分割成纵横线分隔的网格，每个网格站的区域大小相同。

添加组件默认从左往右、从上往下添加。

| 构造方法                                           | 说明                   |
| -------------------------------------------------- | ---------------------- |
| GridLayout(int rows, int cols)                     | 指定行数列数，间距默认 |
| GridLayout(int rows, int cols, int hgap, int vgap) | 指定行数、列数、间距   |

### GridBagLayout

可以让部分组件占用更多的网格（1\*2,3\*3……）

通过 API GridBagConstraints 类设置。

用起来很复杂，就不用了……因为在 swing 中有更强大的解决方法。

### CardLayout

卡片布局，把加入容器的所有组件看做一叠卡片，类似队列，最先添加的在最上面，后添加的在下面。

| 方法                                | 功能                         |
| ----------------------------------- | ---------------------------- |
| CardLayout()                        |                              |
| CardLayout(int hgap, int vgap)      | 指定卡片与容器左右、上下边距 |
| first(Container targer)             | 第一张卡片                   |
| last(Container targer)              | 最后一张卡片                 |
| previous(Container target)          | 前一张卡片                   |
| next(Container target)              | 后一张卡片                   |
| show(Container target, String name) | 显示指定名字的卡片           |

制作一个卡片展示器，有五张卡片，然后5个按钮可以快速查看上一张、下一张、中间一张、第一张、最后一张。

展示卡片部分是 p 容器，5个按钮部分是 p2 容器。

这里涉及到一点事件监听器的知识了。

```java
Frame f=new Frame("卡片布局");
f.setLayout(new BorderLayout());
Panel p=new Panel();
CardLayout c=new CardLayout();//需要创建卡片对象
p.setLayout(c);
String[] names={"第一张","第二张","第三张","第四张"};
for(int i=0;i<4;i++)p.add(names[i],new Button(names[i]));
f.add(p);
Panel p2=new Panel();
Button b1 = new Button("上一张");
Button b2 = new Button("下一张");
Button b3 = new Button("第三张");
Button b4 = new Button("第一张");
Button b5 = new Button("最后一张");
ActionListener l=new ActionListener() {//事件监听器
    @Override
    public void actionPerformed(ActionEvent e) {
        String actionCommand=e.getActionCommand();//获取按钮上的文字
        switch(actionCommand){
            case "上一张":
                c.previous(p);
                break;
            case "下一张":
                c.next(p);
                break;
            case "第三张":
                c.show(p,"第三张");
                break;
            case "第一张":
                c.first(p);
                break;
            case "最后一张":
                c.last(p);
                break;
        }
    }
};
b1.addActionListener(l);
b2.addActionListener(l);
b3.addActionListener(l);
b4.addActionListener(l);
b5.addActionListener(l);
p2.add(b1);
p2.add(b2);
p2.add(b3);
p2.add(b4);
p2.add(b5);
f.add(p2,BorderLayout.SOUTH);
f.pack();
f.setVisible(true);
```

### BoxLayout

是 swing 引入的一个布局管理器，可以简化开发。

BoxLayout(Container target, int axis)，axis有BoxLayout.X_AXIS 横向和BoxLayout.Y_AXIS 纵向。

# Box

java.swing 中还提供了一个更简单的容器：Box，默认布局管理器就是 BoxLayout。

| 方法                             | 说明                       |
| -------------------------------- | -------------------------- |
| static Box createHorizontalBox() | 创建一个水平排列组件的容器 |
| static Box createVerticalBox()   | 创建一个垂直排列组件的容器 |

```java
Box b1=Box.createHorizontalBox();
b1.add(new Button("1"));
b1.add(new Button("2"));
f.add(b1);
Box b2=Box.createVerticalBox();
b2.add(new Button("3"));
b2.add(new Button("4"));
f.add(b2,BorderLayout.SOUTH);
```

还可以生成一些间隔。

| 方法                                              | 说明                     |
| ------------------------------------------------- | ------------------------ |
| static Component createHorizontalGlue()           | 创建一条水平 Glue        |
| static Component createVerticalGlue()             | 创建一条垂直 Glue        |
| static Component createHorizontalStrut(int width) | 创建一条指定宽度的 Strut |
| static Component createVerticalStrut(int height)  | 创建一条指定高度的 Strut |

直接 add 到 Box 中就可以。

**注意：NORTH SOUTH 高度是固定的，和 Glue 连用的时候要注意。**

Glue 随着窗口放缩会变换间距。Strut 间距是固定的。
