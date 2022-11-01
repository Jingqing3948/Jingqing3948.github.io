实际使用 Java 开发图形界面程序时 ，很少使用 AWT 组件，绝大部分时候都是用 Swing 组件开发的 。 

Swing是**由100%纯 Java实现的，不再依赖于本地平台的 GUI**， 因此可以在所有平台上都保持相同的界面外观。

独立于本地平台的Swing组件被称为**轻量级组件**;而依赖于本地平台的 AWT 组件被称为**重量级组件**。

*由于 Swing 的所有组件完全采用 Java 实现，不再调用本地平台的 GUI，所以导致 Swing 图形界面的显示速度要比 AWT 图形界面的显示速度慢一些，但相对于快速发展的硬件设施而言，这种微小的速度差别无妨大碍。*

Swing 组件不再依赖于本地平台的 GUI，无须采用各种平台的 GUI 交集 ，因此 Swing 提供了大量图形界面组件 ， 远远超出了 AWT 所提供的图形界面组件集。用户也可以选择自己喜欢的样式。

Swing 组件采用 MVC(Model-View-Controller， 即模型一视图一控制器)设计模式.

	模型(Model): 用于维护组件的各种状态；
	
	视图(View): 是组件的可视化表现；
	
	控制器(Controller):用于控制对于各种事件、组件做出响应 。
	
	当模型发生改变时，它会通知所有依赖它的视图，视图会根据模型数据来更新自己。Swing使用UI代理来包装视图和控制器， 还有一个模型对象来维护该组件的状态。例如，按钮JButton有一个维护其状态信息的模型ButtonModel对象 。 Swing组件的模型是自动设置的，因此一般都使用JButton，而无须关心ButtonModel对象。

![image-20220518132257874](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220518132257874.png)

**Swing组件和AWT组件的对应关系：**

​	大部分情况下，只需要在AWT组件的名称前面加个J，就可以得到其对应的Swing组件名称，但有几个例外：

​		1. JComboBox: 对应于 AWT 里的 Choice 组件，但比 Choice 组件功能更丰富 。
​		2. JFileChooser: 对应于 AWT 里的 FileDialog 组件 。
​		3. JScrollBar: 对应于 AWT 里的 Scrollbar 组件，注意两个组件类名中 b 字母的大小写差别。
​		4. JCheckBox : 对应于 AWT 里的 Checkbox 组件， 注意两个组件类名中 b 字母的大小 写差别 。
​		5. JCheckBoxMenultem: 对应于 AWT 里的 CheckboxMenuItem 组件，注意两个组件类名中 b字母的大小写差别。



**Swing组件按照功能来分类：**

	1. 顶层容器: JFrame、JApplet、JDialog 和 JWindow 。
	2. 中间容器: JPanel 、 JScrollPane 、 JSplitPane 、 JToolBar 等 。
	3. 特殊容器:在用户界面上具有特殊作用的中间容器，如 JIntemalFrame 、 JRootPane 、 JLayeredPane和 JDestopPane 等 。
	4. 基本组件 : 实现人机交互的组件，如 JButton、 JComboBox 、 JList、 JMenu、 JSlider 等 。
	5. 不可编辑信息的显示组件:向用户显示不可编辑信息的组件，如JLabel 、 JProgressBar 和 JToolTip等。
	6. 可编辑信息的显示组件:向用户显示能被编辑的格式化信息的组件，如 JTable 、 JTextArea 和JTextField 等 。
	7. 特殊对话框组件:可以直接产生特殊对话框的组件 ， 如 JColorChooser 和 JFileChooser 等。

# AWT组件的Swing实现

​	Swing 为除 Canvas 之外的所有 AWT 组件提供了相应的实现，Swing 组件比 AWT 组件的功能更加强大。相对于 AWT 组件， Swing 组件具有如下 4 个额外的功能 :

1. 可以为 Swing 组件设置提示信息。使用 setToolTipText()方法，为组件设置对用户有帮助的提示信息 。
2. 很多 Swing 组件如按钮、标签、菜单项等，除使用文字外，还可以使用图标修饰自己。为了允许在 Swing 组件中使用图标， Swing为Icon 接口提供了 一个实现类: Imagelcon ，该实现类代表一个图像图标。
3. 支持插拔式的外观风格。每个 JComponent 对象都有一个相应的 ComponentUI 对象，为它完成所有的绘画、事件处理、决定尺寸大小等工作。 ComponentUI 对象依赖当前使用的 PLAF ， 使用 UIManager.setLookAndFeel()方法可以改变图形界面的外观风格 。
4. 支持设置边框。Swing 组件可以设置一个或多个边框。 Swing 中提供了各式各样的边框供用户边 用，也能建立组合边框或自己设计边框。 一种空白边框可以用于增大组件，同时协助布局管理器对容器中的组件进行合理的布局。

每个 Swing 组件都有一个对应的UI 类，可以切换其外观。例如 JButton组件就有一个对应的 ButtonUI 类来作为UI代理 。每个 Swing组件的UI代理的类名总是将该 Swing 组件类名的 J 去掉，然后在后面添加 UI 后缀 。 

对于 awt 中的一些监听事件，swing 会有更简便的解决方法。

右键菜单：`f.setComponentPopupMenu(自己定义的右键菜单)`

关闭窗口：

`f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)`

更改样式：

```java
 private void changeFlavor(String command) throws Exception{
        switch (command){
            case "Metal 风格":
                UIManager.setLookAndFeel("javax.swing.plaf.metal.MetalLookAndFeel");
                break;
            case "Nimbus 风格":
                UIManager.setLookAndFeel("javax.swing.plaf.nimbus.NimbusLookAndFeel");
                break;
            case "Windows 风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsLookAndFeel");
                break;
            case "Windows 经典风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsClassicLookAndFeel");
                break;
            case "Motif 风格":
                UIManager.setLookAndFeel("com.sun.java.swing.plaf.motif.MotifLookAndFeel");
                break;
        }

        //更新f窗口内顶级容器以及所有组件的UI
        SwingUtilities.updateComponentTreeUI(f.getContentPane());
        //更新mb菜单条及每部所有组件UI
        SwingUtilities.updateComponentTreeUI(mb);
        //更新右键菜单及内部所有菜单项的UI
        SwingUtilities.updateComponentTreeUI(pop);
    }
```

# 图标展示

构造对象时，可以加一个图像参数。

```java
JButton b=new JButton("确定", new ImageIcon("相对路径"));
```

# 组件边框

![image-20220520212709348](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220520212709348.png)

**特殊的Border：**

1. TitledBorder:它的作用并不是直接为其他组件添加边框，而是为其他边框设置标题，创建该类的对象时，需要传入一个其他的Border对象；
2. ComoundBorder:用来组合其他两个边框，创建该类的对象时，需要传入其他两个Border对象，一个作为内边框，一个座位外边框

**给组件设置边框步骤：**



1. 使用BorderFactory或者XxxBorder创建Border的实例对象；
2. 调用Swing组件的setBorder（Border b）方法为组件设置边框；