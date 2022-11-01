---
title: Java 学习博客_入门—— GUI_4
date: 2022-05-17
tags: study
category: java
---



# 小游戏——弹球

每隔几毫秒就重绘一下页面，可以实现动画效果。可以通过定时器 Timer 解决。

弹球游戏就是打砖块，下面有一块按←→可以左右移动的球拍。球落到球拍下面就算失败。

思路：

1. 创建组件作为成员变量，其中固定大小位置的值（final）；
2. 创建自己的 MyCanvas 类继承 Canvas 类，在其中重写 paint()

```java
import javax.swing.*;
import java.awt.*;

public class MarblesTry {
    private Frame frame=new Frame("弹球游戏");
    //桌面的大小
    private final int TABLE_WIDTH=300;
    private final int TABLE_HEIGHT=400;
    //球拍的大小
    private final int RACKET_WIDTH=60;
    private final int RACKET_HEIGHT=20;
    //小球的大小
    private final int BALL_SIZE=16;
    //定义变量记录小球的坐标
    private int ballX=150;
    private int ballY=20;
    //定义变量记录小球在 x y 方向上移动的速度
    private int speedX=5;
    private int speedY=10;
    //定义变量记录球拍的坐标
    private int racketX=150;
    private final int racketY=340;//球拍只能左右移动
    //定义变量标识当前游戏是否已结束
    private boolean isOver=false;
    //定时器
    private Timer timer;

    //定义一个类绘制内容
    private class MyCanvas extends Canvas{
        public void paint(Graphics g){
            //TODO 在这里绘制内容
            if(isOver){
                g.setColor(Color.RED);
                g.setFont(new Font("Times",Font.BOLD,30));
                g.drawString("游戏结束！",80,150);
            }
            else{
                //绘制小球和球拍
                g.setColor(Color.RED);
                g.fillOval(ballX,ballY,BALL_SIZE,BALL_SIZE);
                g.setColor(Color.PINK);
                g.fillRect(racketX,racketY,RACKET_WIDTH,RACKET_HEIGHT);
            }
        }
    }

    //创建绘画区域
    private MyCanvas drawArea=new MyCanvas();

    public void init(){
        //组装组件，以及游戏逻辑的控制
        //球拍的事件监听
        KeyListener listener=new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                //获取电脑的按键
                int keycode=e.getKeyCode();
                if(keycode==KeyEvent.VK_LEFT){
                    //不可以直接-=10，因为有边界问题
                    if(racketX>=10)racketX-=10;
                    drawArea.repaint();
                }
                else if(keycode==KeyEvent.VK_RIGHT){
                    if(racketX<TABLE_WIDTH-RACKET_WIDTH)racketX+=10;
                    drawArea.repaint();
                }
            }
        };
        ActionListener task=new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                //触顶，或者球拍碰到球：Y轴方向上调转
                if(ballY<=0||(ballY>=racketY-BALL_SIZE&&ballX>=racketX&&ballX<=racketX+RACKET_WIDTH))speedY=-speedY;
                //左右触及边界：X轴方向上调转
                if(ballX<=0||ballX>TABLE_WIDTH-BALL_SIZE)speedX=-speedX;
                //如果球的下边界低于球拍的上边界，且此时球水平方向上不在球拍的范围内：直接判断游戏失败
                if(ballY>racketY-BALL_SIZE&&(ballX<racketX||ballX>racketX+RACKET_WIDTH)) {
                    timer.stop();
                    isOver = true;
                    drawArea.repaint();
                }
                ballX+=speedX;
                ballY+=speedY;
                drawArea.repaint();
            }
        };
        timer=new Timer(100,task);//第一个参数是间隔时间，第二个参数是监听事件
        timer.start();
        //点击 x 号，窗口关闭
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        drawArea.setPreferredSize(new Dimension(TABLE_WIDTH,TABLE_HEIGHT));
        //监听器绑定事件源
        frame.addKeyListener(listener);
        drawArea.addKeyListener(listener);
        frame.add(drawArea,BorderLayout.NORTH);

        frame.pack();
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new MarblesTry().init();
    }
}
```
