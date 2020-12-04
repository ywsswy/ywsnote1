# 设置字体颜色
QLabel *label = new QLabel(tr("Hello Qt!"));
QPalette pe;
pe.setColor(QPalette::WindowText,Qt::white);
label->setPalette(pe)

# 鼠标事件/全局
https://jingyan.baidu.com/article/656db91843f4b4a381249cd2.html

# QPen画笔类学习
https://blog.csdn.net/qq_16668303/article/details/96995288
QObject
    > QPaintDevice
        > QWidget
            > QMainWindow
            > QFrame
                > QLabel

# eg:
import PyQt5.QtWidgets
app = PyQt5.QtWidgets.QApplication(sys.argv)
win = PyQt5.QtWidgets.QWidget()
# do
win.show()
sys.exit(app.exec_())



# 关系PyQt5.
QtWidgets.
    QWidget()
        setWindowTitle(s)
        setWindowOpacity(f)
        resize(w,h)
        setWindowFlags(QtCore.Qt.WindowStaysOnTopHint)
        mapFromGlobal(QPoint(x,y)) # 从屏幕坐标系转成窗口坐标系
        mapToGlobal(QPoint(x,y)) 
    QLabel(win)
        setText(s)
        setGeometry(x,y,w,h)
        setAutoFillBackground(bool)
        setPalette() # 设置调色板
        setFont(PyQt5.QtGui.QFont("Roman times", 25))
QtGui.
    QPalette()
        setColor(QPalette.ColorRole, color_name) #https://www.cnblogs.com/gaiqingfeng/p/13274916.html
    QPalette.
        Window #背景色的宏(QPalette.ColorRole)
    QColor(r,g,b)
    QPainter
        setPen(pen)
        drawLine(winps.x(), winps.y(), winpe.x(), winpe.y()) # 窗口坐标系，非屏幕坐标系
    QPen # QPen(PyQt5.QtGui.QColor(127, 127, 127), 2, PyQt5.QtCore.Qt.SolidLine)
        setDashPattern([1,4,3,4]) #画一个点，空四个点，画三个点，空四个点
QtCore.
    Qt.
        WindowStaysOnTopHint #窗口置顶的宏
        FramelessWindowHint #没有边框
        SolidLine #实线宏
        CustomDashLine # 自定义虚线宏
    QTimer() #我是用threading.Timer玩的
