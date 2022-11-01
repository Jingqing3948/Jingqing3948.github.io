![image-20220518101108824](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220518101108824.png)

鼠标在棋盘上移动时，会显示当前位置放置棋子，会落到的位置，即红框部分。

点击白棋、黑棋切换棋子，点击删除后点击棋子删除。不能判断输赢。

```java
import javax.imageio.ImageIO;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseMotionAdapter;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;

public class Gobang {
    private Frame frame=new Frame();
    //声明四个 BufferedImage 对象，记录四张图片
    private BufferedImage table;
    private BufferedImage blackChess;
    private BufferedImage whiteChess;
    private BufferedImage selected;


    //宽高必须和图片一样
    private final int TABLE_WIDTH=535;
    private final int TABLE_HEIGHT=536;
    //横向纵向可以下多少子？
    final int BOARD_SIZE=15;
    //每个棋子占用棋盘的比例
    final int RATE=TABLE_WIDTH/BOARD_SIZE;
    //声明变量，记录棋子对于 x y 方向上的偏移量
    final int X_OFFSET=5;
    final int Y_OFFSET=6;
    //二维数组记录棋子，如果值为0：没棋子；1：白棋；2：黑棋；
    int[][] board=new int[BOARD_SIZE][BOARD_SIZE];
    //声明红色选择框的坐标，就是二维数组中的索引
    int selected_X=-1;
    int selected_Y=-1;
    //自定义类继承 Canvas
    private class ChessBoard extends Canvas{
        @Override
        public void paint(Graphics g) {
            //绘图
            //绘制棋盘
            g.drawImage(table,0,0,null);
            //绘制选择框
            if(selected_X>=0&&selected_Y>=0&&selected_X<15&&selected_Y<15)g.drawImage(selected,selected_X*RATE+X_OFFSET,selected_Y*RATE+Y_OFFSET,null);
            //绘制棋子
            for(int i=0;i<BOARD_SIZE;i++)
            {
                for(int j=0;j<BOARD_SIZE;j++){
                    if(board[i][j]==0)continue;
                    else if(board[i][j]==1)g.drawImage(whiteChess,i*RATE+X_OFFSET,j*RATE+Y_OFFSET,null);
                    else if(board[i][j]==2)g.drawImage(blackChess,i*RATE+X_OFFSET,j*RATE+Y_OFFSET,null);
                }
            }

        }
    }
    ChessBoard chessBoard=new ChessBoard();
    //声明变量记录当前下棋的颜色，初始黑棋
    int board_type=2;
    //声明底部用到的组件
    Panel p=new Panel();
    Button whiteBtn=new Button("白棋");
    Button blackBtn=new Button("黑棋");
    Button deleteBtn=new Button("删除");

    public void refreshBtnColor(Color whiteBtnColor, Color blackBtnColor, Color deleteBtnColor){
        //设置按钮颜色的方法：
        whiteBtn.setBackground(whiteBtnColor);
        blackBtn.setBackground(blackBtnColor);
        deleteBtn.setBackground(deleteBtnColor);
    }

    public void init(){
        chessBoard.setPreferredSize(new Dimension(TABLE_WIDTH,TABLE_HEIGHT));
        frame.add(chessBoard);
        whiteBtn.addActionListener(e ->{
            //刷新要下的棋子标志为1
            board_type=1;
            //刷新按钮颜色，点击后按钮颜色会改变
            refreshBtnColor(Color.GREEN,Color.GRAY,Color.GRAY);
        });
        blackBtn.addActionListener(e ->{
            //刷新要下的棋子标志为1
            board_type=2;
            //刷新按钮颜色，点击后按钮颜色会改变
            refreshBtnColor(Color.GRAY,Color.GREEN,Color.GRAY);
        });
        deleteBtn.addActionListener(e ->{
            //刷新要下的棋子标志为1
            board_type=0;
            //刷新按钮颜色，点击后按钮颜色会改变
            refreshBtnColor(Color.GRAY,Color.GRAY,Color.GREEN);
        });
        p.add(whiteBtn);
        p.add(blackBtn);
        p.add(deleteBtn);
        frame.add(p,BorderLayout.SOUTH);
        chessBoard.addMouseMotionListener(new MouseMotionAdapter(){
            @Override
            public void mouseMoved(MouseEvent e) {
                //鼠标移动时会调用
                selected_X=(e.getX()-X_OFFSET)/RATE;
                selected_Y=(e.getY()-Y_OFFSET)/RATE;
                chessBoard.repaint();
            }
        });
        chessBoard.addMouseListener(new MouseAdapter() {
            @Override
            public void mouseClicked(MouseEvent e) {
                int xPos=(e.getX()-X_OFFSET)/RATE;
                int yPos=(e.getY()-Y_OFFSET)/RATE;
                if(xPos<BOARD_SIZE&&yPos<BOARD_SIZE)board[xPos][yPos]=board_type;
                chessBoard.repaint();
            }

            @Override
            public void mouseExited(MouseEvent e) {
                selected_X=-1;
                selected_Y=-1;
            }
        });
        try {
            table=ImageIO.read(new File("D:\\86138\\Documents\\ithema\\java\\代码\\awt编程代码\\awt\\img\\board.jpg"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            whiteChess=ImageIO.read(new File("D:\\86138\\Documents\\ithema\\java\\代码\\awt编程代码\\awt\\img\\white.gif"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            blackChess=ImageIO.read(new File("D:\\86138\\Documents\\ithema\\java\\代码\\awt编程代码\\awt\\img\\black.gif"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            selected=ImageIO.read(new File("D:\\86138\\Documents\\ithema\\java\\代码\\awt编程代码\\awt\\img\\selected.gif"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        frame.pack();
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new Gobang().init();
    }
}
```

![image preview](C:\Users\86138\Desktop\sa3yy-b9n46.gif)

但是运行时因为不停地重绘，画面一直在闪。因为这种绘制方法没有用到缓冲区，每次都是直接绘制在组件上的。

只要把 Frame 改成 JFrame，Canvas 改成 JPanel 即可。下节课开始详细介绍 swing.