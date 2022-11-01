# AWT 常用组件

Button

Canvas：用于绘图的画布

Checkbox，CheckboxGroup：单选框

Choice：下拉选择框（选择条目通过 add() 添加）

Frame：窗口

Label：标签

List：列表框组件，可以添加多条项目。（项目通过 add() 添加）

Panel：容器

ScrollBar：滚动条

ScrollPane：带滚动条的容器组件

TextArea：多行文本域

TextField：单行文本框

# 开发逻辑

所有组件在成员变量处定义，在方法中组装界面，在 main 方法中调用方法。

布局：从整体到局部。

写代码时：由局部到整体。

```java
public class ComponentTry {
    Frame f=new Frame();
    TextArea ta=new TextArea(5,20);
    Choice colorChooser=new Choice();
    CheckboxGroup cbg=new CheckboxGroup();
    Checkbox male=new Checkbox("男",cbg,true);
    Checkbox female=new Checkbox("女",cbg,false);
    Checkbox isMarried=new Checkbox("是否已婚？");
    TextField tf=new TextField(30);
    Button ok=new Button("确认");
    List colorList=new List(6,true);//支持多选

    public void init(){
        Box bBox = Box.createHorizontalBox();
        bBox.add(tf);
        bBox.add(ok);
        f.add(bBox,BorderLayout.SOUTH);
        colorChooser.add("红色");
        colorChooser.add("蓝色");
        colorChooser.add("绿色");
        Box cBox=Box.createHorizontalBox();
        cBox.add(colorChooser);
        cBox.add(male);
        cBox.add(female);
        cBox.add(isMarried);
        Box topLeft=Box.createVerticalBox();
        topLeft.add(ta);
        topLeft.add(cBox);
        colorList.add("红色");
        colorList.add("绿色");
        colorList.add("蓝色");
        Box top=Box.createHorizontalBox();
        top.add(topLeft);
        top.add(colorList);
        f.add(top);
        f.pack();
        f.setVisible(true);
    }

    public static void main(String[] args) {
        new ComponentTry().init();
    }
}
```

# Dialog

Dialog 是 Window 类的子类，是 一个容器类，属于特殊组件 。 对话框是可以独立存在的顶级窗口， 因此用法与普通窗口的用法几乎完全一样，但是使用对话框需要注意下面两点：

- 对话框通常依赖于其他窗口，就是通常需要有一个父窗口；
- 对话框有非模式(non-modal)和模式(modal)两种，当某个模式对话框被打开后，该模式对话框总是位于它的父窗口之上，在模式对话框被关闭之前，父窗口无法获得焦点。

| 方法名称                                         | 方法功能                                                     |
| ------------------------------------------------ | ------------------------------------------------------------ |
| Dialog(Frame owner, String title, boolean modal) | 创建一个对话框对象：<br/>owner:当前对话框的父窗口<br/>title:当前对话框的标题<br/>modal：当前对话框是否是模式对话框，true/false |

```java
Frame frame = new Frame("这里测试Dialog");

Dialog d1 = new Dialog(frame, "模式对话框", true);

//往对话框中添加内容
Box vBox = Box.createVerticalBox();

vBox.add(new TextField(15));
vBox.add(new JButton("确认"));
d1.add(vBox);

Button b1 = new Button("打开模式对话框");

//设置对话框的大小和位置
d1.setBounds(20,30,200,100);


//给b1绑定监听事件
b1.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        d1.setVisible(true);
    }
});


//把按钮添加到frame中
frame.add(b1);

//设置frame最佳大小并可见
frame.pack();
frame.setVisible(true);
```

![image-20220514191933447](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220514191933447.png)

## FileDialog

Dialog 的一个子类，可以进行文件操作。

| 方法名称                                         | 方法功能                                                     |
| ------------------------------------------------ | ------------------------------------------------------------ |
| FileDialog(Frame parent, String title, int mode) | 创建一个文件对话框：<br/>parent:指定父窗口<br/>title:对话框标题<br/>mode:文件对话框类型，**如果指定为FileDialog.load，用于打开文件，如果指定为FileDialog.SAVE,用于保存文件** |
| String getDirectory()                            | 获取被打开或保存文件的绝对路径                               |
| String getFile()                                 | 获取被打开或保存文件的文件名                                 |

Frame frame = new Frame("这里测试FileDialog");

```java
FileDialog d1 = new FileDialog(frame, "选择需要加载的文件", FileDialog.LOAD);
FileDialog d2 = new FileDialog(frame, "选择需要保存的文件", FileDialog.SAVE);

Button b1 = new Button("打开文件");
Button b2 = new Button("保存文件");

//给按钮添加事件
b1.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        d1.setVisible(true);
        //打印用户选择的文件路径和名称
        System.out.println("用户选择的文件路径:"+d1.getDirectory());
        System.out.println("用户选择的文件名称:"+d1.getFile());
    }
});

System.out.println("-------------------------------");
b2.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        d2.setVisible(true);
        //打印用户选择的文件路径和名称
        System.out.println("用户选择的文件路径:"+d2.getDirectory());
        System.out.println("用户选择的文件名称:"+d2.getFile());
    }
});

//添加按钮到frame中

frame.add(b1);
frame.add(b2,BorderLayout.SOUTH);

//设置frame最佳大小并可见
frame.pack();
frame.setVisible(true);
```

# 事件处理

为什么之前的窗口点击 x 号不能关闭？

窗口、组件是不自带事件处理的。

就像按钮点击触发事件，需要写一个事件监听器；x 号并没有写点击后相应的事件监听器。要添加相应的事件处理才能触发。

## GUI 事件处理机制

当在某个组件上发生某些操作的时候，会自动的触发一段代码的执行。

**事件源(Event Source)**：操作发生的场所，通常指某个组件，例如按钮、窗口等；
**事件（Event）**：在事件源上发生的操作可以叫做事件，GUI会把事件都封装到一个Event对象中，如果需要知道该事件的详细信息，就可以通过Event对象来获取。
**事件监听器(Event Listener)**:当在某个事件源上发生了某个事件，事件监听器就可以对这个事件进行处理。

**注册监听**：把某个事件监听器(A)通过某个事件(B)绑定到某个事件源(C)上，当在事件源C上发生了事件B之后，那么事件监听器A的代码就会自动执行。

![image-20220515062437913](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220515062437913.png)

实现方式：内部类，当这个事件监听器使用次数不只一次时建议。

```java
Frame f=new Frame("新窗口");
Button b=new Button("输出");
TextArea ta=new TextArea();
public void init(){
    b.addActionListener(new MyActionListener());
    f.add(ta);
    f.add(b,BorderLayout.SOUTH);
    f.pack();
    f.setVisible(true);
}
private class MyActionListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e){
        System.out.println("输出！");
        ta.setText("输出文本");
    }
}
public static void main(String[] args) {
    new ActionTry().init();
}
```

实现方式：匿名对象，当该事件监听器使用次数较少时建议。

```java
Frame f=new Frame("新窗口");
Button b=new Button("输出");
TextArea ta=new TextArea();
public void init(){
    b.addActionListener(new ActionListener(){
        @Override
        public void actionPerformed(ActionEvent e){
        System.out.println("输出！");
        ta.setText("输出文本");
    }
    });
    f.add(ta);
    f.add(b,BorderLayout.SOUTH);
    f.pack();
    f.setVisible(true);
}
public static void main(String[] args) {
    new ActionTry().init();
}
```

## 低级事件

基于某个特定动作的事件，如鼠标点击，拖放等。

| 事件           | 触发时机                                                     |
| -------------- | ------------------------------------------------------------ |
| ComponentEvent | 组件事件 ， 当 组件尺寸发生变化、位置发生移动、显示/隐藏状态发生改变时触发该事件。 |
| ContainerEvent | 容器事件 ， 当容器里发生添加组件、删除组件时触发该事件 。    |
| WindowEvent    | 窗口事件， 当窗 口状态发生改变 ( 如打开、关闭、最大化、最 小化)时触发该事件 。 |
| FocusEvent     | 焦点事件 ， 当组件得到焦点或失去焦点 时触发该事件 。         |
| KeyEvent       | 键盘事件 ， 当按键被按下、松开、单击时触发该事件。           |
| MouseEvent     | 鼠标事件，当进行单击、按下、松开、移动鼠标等动作 时触发该事件。 |
| PaintEvent     | 组件绘制事件 ， 该事件是一个特殊的事件类型 ， 当 GUI 组件调 用 update/paint 方法 来呈现自身时触发该事件，该事件并非专用于事件处理模型 。 |

## 高级事件

不指向某个特定动作，而是由事件代表的含义表示。

| 事件           | 触发时机                                                     |
| -------------- | ------------------------------------------------------------ |
| ActionEvent    | 动作事件 ，当按钮、菜单项被单击，在 TextField 中按 Enter 键时触发 |
| AjustmentEvent | 调节事件，在滑动条上移动滑块以调节数值时触发该事件。         |
| ltemEvent      | 选项事件，当用户选中某项， 或取消选中某项时触发该事件 。     |
| TextEvent      | 文本事件， 当文本框、文本域里的文本发生改变时触发该事件。    |

## 事件监听器

不同的事件需要使用不同的监听器监听，不同的监听器需要实现不同的监听器接口， 当指定事件发生后 ， 事件监听器就会调用所包含的事件处理器(实例方法)来处理事件 。

| 事件类别        | 描述信息                 | 监听器接口名        |
| --------------- | ------------------------ | ------------------- |
| ActionEvent     | 激活组件                 | ActionListener      |
| ItemEvent       | 选择了某些项目           | ItemListener        |
| MouseEvent      | 鼠标移动                 | MouseMotionListener |
| MouseEvent      | 鼠标点击等               | MouseListener       |
| KeyEvent        | 键盘输入                 | KeyListener         |
| FocusEvent      | 组件收到或失去焦点       | FocusListener       |
| AdjustmentEvent | 移动了滚动条等组件       | AdjustmentListener  |
| ComponentEvent  | 对象移动缩放显示隐藏等   | ComponentListener   |
| WindowEvent     | 窗口收到窗口级事件       | WindowListener      |
| ContainerEvent  | 容器中增加删除了组件     | ContainerListener   |
| TextEvent       | 文本字段或文本区发生改变 | TextListener        |

```java
Frame f=new Frame();
TextField tf=new TextField(30);
Choice c=new Choice();
c.add("1");
c.add("2");
c.add("3");
tf.addTextListener(new TextListener() {
    @Override
    public void textValueChanged(TextEvent e) {
        System.out.println("文本内容改变"+tf.getText());
    }
});
c.addItemListener(new ItemListener() {
    @Override
    public void itemStateChanged(ItemEvent e) {
        Object item=c.getSelectedItem();
        System.out.println("选择框列表改变"+item);
    }
});
f.addContainerListener(new ContainerListener() {
    @Override
    public void componentAdded(ContainerEvent e) {
        Component child=e.getChild();
        System.out.println("新添加的组件："+child);
    }

    @Override
    public void componentRemoved(ContainerEvent e) {

    }
});
```

### 点击 x 号关闭

思路：很简单，frame 绑定对应的 WindowListener 即可。

但是问题是 WindowListener 里面有很多抽象方法需要重写。windowClosing（关闭窗口时执行），windowClosed（关闭完窗口后执行），windowOpened（打开窗口后执行）……

有一种适配器模式 WindowAdapter，原理是 WindowAdapter 继承了 WindowListener 后，重写了以上所有抽象方法（只是简单的重写补全了，没有做任何操作。如 windowOpened(){} windowClosed(){} ）。然后我们可以只重写自己想要重写的方法，而不用把所有方法都重写。

```java
frame.addWindowListener(new WindowAdapter(){
    @Override
    public void windowClosing(WindowEvent e){
        System.exit(0);
    }
})
```



