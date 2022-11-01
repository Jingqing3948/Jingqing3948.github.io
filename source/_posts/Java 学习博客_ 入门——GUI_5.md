---
title: Java 学习博客_入门—— GUI_5
date: 2022-05-18
tags: study
category: java
---

# 位图

之前只能绘制一些简单的几何图形。Graphics 的 drawImage() 方法可以绘制位图。Image 对象代表位图。

**位图使用步骤：**

1.创建Image的子类对象 BufferedImage(int width,int height,int ImageType) ,创建时需要指定位图的宽高及类型属性；此时相当于在内存中生成了一张图片；

2.调用 BufferedImage 对象的 getGraphics() 方法获取画笔，此时就可以往内存中的这张图片上绘图了，绘图的方法和之前学习的一模一样（先绘制到内存上）；

3.调用组件的 drawImage() 方法，一次性的内存中的图片 BufferedImage 绘制到特定的组件上（内存再绘制到组件上，效率更高些）。

**案例：**

在画布上用鼠标绘制。右键可以改变红绿蓝三种颜色。

```java
import java.awt.*;
import java.awt.event.*;
import java.awt.image.BufferedImage;

public class ImageTry {
    private Frame frame=new Frame("绘制位图");
    private final int AREA_HEIGHT=500;
    private final int AREA_WIDTH=400;
    //右键菜单项
    private PopupMenu colorMenu=new PopupMenu();
    private MenuItem redItem=new MenuItem("红色");
    private MenuItem greenItem=new MenuItem("绿色");
    private MenuItem blueItem=new MenuItem("蓝色");
    //当前画笔颜色
    private Color forceColor=Color.BLACK;
    //上一次画笔位置
    private int preX=-1;
    private int preY=-1;
    BufferedImage image=new BufferedImage(AREA_WIDTH,AREA_HEIGHT,BufferedImage.TYPE_INT_RGB);
    //通过类图获取关联的 Graphics 对象
    private Graphics g= image.getGraphics();
    //画布区域。自定义一个类继承 Canvas
    private class MyCanvas extends Canvas{
        @Override
        public void paint(Graphics g){
            g.drawImage(image,0,0,null);
        }
    }
    MyCanvas drawArea=new MyCanvas();

    public void init(){
        ActionListener listener=new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String command=e.getActionCommand();
                switch (command)
                {
                    case "红色":
                        forceColor= Color.RED;
                        break;
                    case "绿色":
                        forceColor= Color.GREEN;
                        break;
                    case "蓝色":
                        forceColor= Color.BLUE;
                        break;
                    default:
                        break;
                }
            }
        };
        redItem.addActionListener(listener);
        greenItem.addActionListener(listener);
        blueItem.addActionListener(listener);

        colorMenu.add(redItem);
        colorMenu.add(greenItem);
        colorMenu.add(blueItem);
        drawArea.add(colorMenu);
        drawArea.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseReleased(MouseEvent e) {
                if(e.isPopupTrigger()){
                    colorMenu.show(drawArea,e.getX(),e.getY());
                }
                preX=-1;
                preY=-1;
            }
        });

        //设置绘图背景为白色
        g.fillRect(0,0,AREA_WIDTH,AREA_HEIGHT);
        drawArea.setPreferredSize(new Dimension(AREA_WIDTH,AREA_HEIGHT));

        //监听鼠标移动，绘制
        drawArea.addMouseMotionListener(new MouseMotionAdapter() {
            @Override
            public void mouseDragged(MouseEvent e) {
                if(preX>=0&&preY>=0){
                    g.setColor(forceColor);
                    g.drawLine(preX,preY,e.getX(),e.getY());
                }
                preX=e.getX();
                preY=e.getY();
                drawArea.repaint();
            }
        });



        frame.add(drawArea);

        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        frame.pack();
        frame.setVisible(true);
    }
    public static void main(String[] args){
        new ImageTry().init();
    }
}
```

# ImageIO

位图的读写，非常常用。

实际生活中，很多软件都支持打开本地磁盘已经存在的图片，进行编辑，然后重新保存到本地磁盘。

| 方法名称                                                     | 方法功能                 |
| ------------------------------------------------------------ | ------------------------ |
| static BufferedImage read(File input)                        | 读取本地磁盘图片文件     |
| static BufferedImage read(InputStream input)                 | 读取本地磁盘图片文件     |
| static boolean write(RenderedImage im, String formatName, File output) | 往本地磁盘中输出图片文件 |

```java
public class ImageIOTry {

    private Frame frame=new Frame("查看图片");

    private BufferedImage image;

    private class MyCanvas extends Canvas{
        @Override
        public void paint(Graphics g) {
            if(image!=null)
                g.drawImage(image,0,0,image.getWidth(),image.getHeight(),null);
        }
    }

    private MyCanvas drawArea=new MyCanvas();

    public void init() throws Exception{
        MenuBar menubar=new MenuBar();
        Menu menu=new Menu("文件");
        MenuItem mi1=new MenuItem("打开");
        MenuItem mi2=new MenuItem("保存");
        //打开一个文件对话框。gdk 9支持λ表达式,也可以这样写
        mi1.addActionListener(e -> {
            FileDialog fileDialog=new FileDialog(frame,"打开图片");
            fileDialog.setVisible(true);
            String dir= fileDialog.getDirectory();
            System.out.println(dir);
            String file=fileDialog.getFile();
            try {
                image=ImageIO.read(new File(dir,file));//输出图片
                drawArea.repaint();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        });
        mi2.addActionListener(e -> {
            FileDialog fileDialog=new FileDialog(frame,"保存图片",FileDialog.SAVE);
            fileDialog.setVisible(true);
            String dir= fileDialog.getDirectory();
            String file=fileDialog.getFile();
            try {
                ImageIO.write(image,"JPEG",new File(dir,file));
                drawArea.repaint();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        });
        menu.add(mi1);
        menu.add(new MenuItem("-"));
        menu.add(mi2);
        menubar.add(menu);
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        frame.setMenuBar(menubar);
        frame.add(drawArea);
        frame.setBounds(200,200,400,500);
        frame.setVisible(true);
    }

    public static void main(String[] args) throws Exception {
        new ImageIOTry().init();
    }
}
```

