import sys
import pymysql
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5.QtGui import *


class MainWindow(QMainWindow, QWidget):
    windowList = []

    def __init__(self):
        super().__init__()
        self.init()

    def init(self):

        self.setWindowTitle('宿舍管理系统')
        self.setGeometry(300, 200, 1000, 500)

        # 创建菜单栏
        self.createMenus()

        # 创建空表格，后面显示数据库内容要用
        self.table = QTableWidget(self)
        self.table.setGeometry(550, 40, 430, 450)

        # 学生管理功能
        Label1 = QLabel("学号", self)
        Label1.setGeometry(50, 50, 60, 40)
        self.Edit1 = QLineEdit("", self)
        self.Edit1.setGeometry(110, 50, 80, 40)

        Label2 = QLabel("姓名", self)
        Label2.setGeometry(210, 50, 60, 40)
        self.Edit2 = QLineEdit("", self)
        self.Edit2.setGeometry(270, 50, 80, 40)

        Label3 = QLabel("宿舍号", self)
        Label3.setGeometry(370, 50, 60, 40)
        self.Edit3 = QLineEdit("", self)
        self.Edit3.setGeometry(430, 50, 80, 40)

        Button1 = QPushButton("录入", self)
        Button1.setGeometry(50, 110, 80, 40)
        Button1.clicked.connect(self.luru1)

        Button2 = QPushButton("修改", self)
        Button2.setGeometry(150, 110, 80, 40)
        Button2.clicked.connect(self.up1)

        Button3 = QPushButton("查询", self)
        Button3.setGeometry(250, 110, 80, 40)
        Button3.clicked.connect(self.se1)

        Button4 = QPushButton("浏览", self)
        Button4.setGeometry(350, 110, 80, 40)
        Button4.clicked.connect(self.liu1)

        # 宿舍管理功能
        Label4 = QLabel("宿舍号", self)
        Label4.setGeometry(50, 170, 60, 40)
        self.Edit4 = QLineEdit("", self)
        self.Edit4.setGeometry(110, 170, 80, 40)

        Label5 = QLabel("人数", self)
        Label5.setGeometry(210, 170, 60, 40)
        self.Edit5 = QLineEdit("", self)
        self.Edit5.setGeometry(270, 170, 80, 40)

        Button5 = QPushButton("录入", self)
        Button5.setGeometry(50, 230, 80, 40)
        Button5.clicked.connect(self.luru2)

        Button6 = QPushButton("修改", self)
        Button6.setGeometry(150, 230, 80, 40)
        Button6.clicked.connect(self.up2)

        Button7 = QPushButton("查询", self)
        Button7.setGeometry(250, 230, 80, 40)
        Button7.clicked.connect(self.se2)

        Button8 = QPushButton("浏览", self)
        Button8.setGeometry(350, 230, 80, 40)
        Button8.clicked.connect(self.liu2)

        # 维修管理功能
        Label7 = QLabel("学号", self)
        Label7.setGeometry(50, 290, 50, 40)
        self.Edit7 = QLineEdit("", self)
        self.Edit7.setGeometry(100, 290, 60, 40)

        Label8 = QLabel("宿舍号", self)
        Label8.setGeometry(50, 350, 50, 40)
        self.Edit8 = QLineEdit("", self)
        self.Edit8.setGeometry(100, 350, 60, 40)

        Label9 = QLabel("具体问题", self)
        Label9.setGeometry(170, 290, 80, 40)
        self.Edit9 = QLineEdit("", self)
        self.Edit9.setGeometry(260, 290, 100, 40)

        Label10 = QLabel("反馈信息", self)
        Label10.setGeometry(170, 350, 80, 40)
        self.Edit10 = QLineEdit("", self)
        self.Edit10.setGeometry(260, 350, 100, 40)

        Button9 = QPushButton("加入", self)
        Button9.setGeometry(370, 290, 50, 40)
        Button9.clicked.connect(self.luru3)

        Button10 = QPushButton("加入", self)
        Button10.setGeometry(370, 350, 50, 40)
        Button10.clicked.connect(self.luru4)

        Button11 = QPushButton("浏览", self)
        Button11.setGeometry(430, 290, 50, 40)
        Button11.clicked.connect(self.liu3)

        Button12 = QPushButton("统计", self)
        Button12.setGeometry(430, 350, 50, 40)
        Button12.clicked.connect(self.count1)

    def luru1(self):
        # 获取三个文本框内的内容
        e1 = int(self.Edit1.text())
        e2 = self.Edit2.text()
        e3 = int(self.Edit3.text())
        print(e3)

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()

        cur.execute("select * from hostel WHERE hostelid = " + str(e3) + ";")
        data1 = cur.fetchall()
        print(data1)
        if len(data1) != 0:
            if data1[0][1] < 6:
                cur.execute("INSERT INTO student(studentid, studentname, hostelid) "
                            "VALUES(" + str(e1) + ", '" + e2 + "', " + str(e3) + ");")
                db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
                cur.execute("select * from student;")
                data = cur.fetchall()
                print(data)
                cur.execute("SET FOREIGN_KEY_CHECKS = 0")
                cur.execute("UPDATE hostel SET hostelid = " + str(e3) + ", headcount = "
                            + str(data1[0][1] + 1) + " WHERE hostelid = " + str(e3) + ";")
                cur.execute("SET FOREIGN_KEY_CHECKS = 1")
                db.commit()
                cur.execute('select * from hostel')
                data = cur.fetchall()
                print(data)
            else:
                QMessageBox.information(self, "警告", "此宿舍已满")
                print("此宿舍已满")
        else:
            QMessageBox.information(self, "警告", "不存在此宿舍")
            print("不存在此宿舍")

        cur.close()
        db.close()

    def se1(self):
        # 获取三个文本框内的内容
        e1 = int(self.Edit1.text())

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from student WHERE studentid = " + str(e1) + ";")
        data1 = cur.fetchall()
        print(data1)
        info = "学号：" + str(data1[0][0]) + "  姓名：" + str(data1[0][1]) + "  宿舍号:" + str(data1[0][2])
        QMessageBox.information(self, "查询结果", info)
        cur.close()
        db.close()

    def up1(self):
        # 获取三个文本框内的内容
        e1 = int(self.Edit1.text())
        e2 = self.Edit2.text()
        e3 = int(self.Edit3.text())

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("SET FOREIGN_KEY_CHECKS = 0")
        cur.execute("UPDATE student SET studentid = " + str(e1) + ", studentname = '" + e2 +
                    "', hostelid = " + str(e3) + " WHERE studentid = " + str(e1) + ";")
        cur.execute("SET FOREIGN_KEY_CHECKS = 1")
        db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
        cur.execute('select * from student')
        data = cur.fetchall()
        print(data)
        cur.close()
        db.close()

    def liu1(self):
        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from student ;")
        data = cur.fetchall()
        print(data)
        titles = ['学号', '姓名', '宿舍号']
        self.table.setRowCount(len(data))  # 设置行数--不包括标题列
        self.table.setColumnCount(len(data[0]))  # 设置列数
        self.table.setHorizontalHeaderLabels(titles)  # 标题列---水平标题
        self.table.setEditTriggers(QTableWidget.AnyKeyPressed)  # 单元格用户能否、如何编辑
        self.table.setSelectionBehavior(QTableWidget.SelectItems)  # 设置选中行
        self.table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)  # 设置列宽的适应方式
        QTableWidget.resizeRowsToContents(self.table)  # 设置行高与内容匹配

        for i in range(len(data)):
            for j in range(len(data[0])):
                temp_data = data[i][j]  # 临时记录，不能直接插入表格
                sql1 = QTableWidgetItem(str(temp_data))  # 转换后可插入表格
                self.table.setItem(i, j, sql1)  # 将内容填入表格
        cur.close()
        db.close()

    def luru2(self):
        # 获取三个文本框内的内容
        e5 = int(self.Edit4.text())
        e6 = int(self.Edit5.text())

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()

        if e6 < 6:
            cur.execute("SET FOREIGN_KEY_CHECKS = 0")
            cur.execute("INSERT INTO hostel(hostelid, headcount) "
                        "VALUES(" + str(e5) + ", " + str(e6) + ");")
            cur.execute("SET FOREIGN_KEY_CHECKS = 1")
            db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
            cur.execute('select * from hostel')
            data = cur.fetchall()
            print(data)
        else:
            print("超过最大数量")
            QMessageBox.information(self, "警告", "超过最大数量")

        cur.close()
        db.close()

    def se2(self):
        # 获取三个文本框内的内容
        e1 = int(self.Edit4.text())

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from hostel WHERE hostelid = " + str(e1) + ";")
        data1 = cur.fetchall()
        print(data1)
        info = "宿舍号：" + str(data1[0][0]) + "  人数：" + str(data1[0][1])
        QMessageBox.information(self, "查询结果", info)
        cur.close()
        db.close()

    def up2(self):
        # 获取三个文本框内的内容
        e1 = int(self.Edit4.text())
        e3 = int(self.Edit5.text())

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("SET FOREIGN_KEY_CHECKS = 0")
        cur.execute("UPDATE hostel SET hostelid = " + str(e1) +
                    ", headcount = " + str(e3) + " WHERE hostelid = " + str(e1) + ";")
        cur.execute("SET FOREIGN_KEY_CHECKS = 1")
        db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
        cur.execute('select * from hostel')
        data = cur.fetchall()
        print(data)
        cur.close()
        db.close()

    def liu2(self):
        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from hostel ;")
        data = cur.fetchall()
        print(data)
        titles = ['宿舍号', '人数']
        self.table.setRowCount(len(data))  # 设置行数--不包括标题列
        self.table.setColumnCount(len(data[0]))  # 设置列数
        self.table.setHorizontalHeaderLabels(titles)  # 标题列---水平标题
        self.table.setEditTriggers(QTableWidget.AnyKeyPressed)  # 单元格用户能否、如何编辑
        self.table.setSelectionBehavior(QTableWidget.SelectItems)  # 设置选中行
        self.table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)  # 设置列宽的适应方式
        QTableWidget.resizeRowsToContents(self.table)  # 设置行高与内容匹配

        for i in range(len(data)):
            for j in range(len(data[0])):
                temp_data = data[i][j]  # 临时记录，不能直接插入表格
                sql1 = QTableWidgetItem(str(temp_data))  # 转换后可插入表格
                self.table.setItem(i, j, sql1)  # 将内容填入表格
        cur.close()
        db.close()

    def luru3(self):
        # 获取三个文本框内的内容
        e5 = int(self.Edit7.text())
        e6 = int(self.Edit8.text())
        e7 = self.Edit9.text()

        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()

        cur.execute("SET FOREIGN_KEY_CHECKS = 0")
        cur.execute("INSERT INTO service(studentid, hostelid, repairinfo) "
                    "VALUES(" + str(e5) + ", " + str(e6) + ",'" + e7+ "');")
        cur.execute("SET FOREIGN_KEY_CHECKS = 1")
        db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
        cur.execute('select * from service')
        data = cur.fetchall()
        print(data)
        cur.close()
        db.close()

    def luru4(self):
        # 获取三个文本框内的内容
        e5 = int(self.Edit7.text())
        e6 = int(self.Edit8.text())
        e7 = self.Edit10.text()
        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("SET FOREIGN_KEY_CHECKS = 0")
        cur.execute("UPDATE service SET feedback = '"+ e7 + "' WHERE hostelid = "+ str(e6)
                    +" and studentid = "+ str(e5) +";")
        cur.execute("SET FOREIGN_KEY_CHECKS = 1")
        db.commit()  # 只要是修改了表内容的操作，后面一定要提交，否则不起作用
        cur.execute('select * from service')
        data = cur.fetchall()
        print(data)
        cur.close()
        db.close()

    def liu3(self):
        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from service ;")
        data = cur.fetchall()
        print(data)
        titles = ['学号', '宿舍号', '具体问题', '反馈信息']
        self.table.setRowCount(len(data))  # 设置行数--不包括标题列
        self.table.setColumnCount(len(data[0]))  # 设置列数
        self.table.setHorizontalHeaderLabels(titles)  # 标题列---水平标题
        self.table.setEditTriggers(QTableWidget.AnyKeyPressed)  # 单元格用户能否、如何编辑
        self.table.setSelectionBehavior(QTableWidget.SelectItems)  # 设置选中行
        self.table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)  # 设置列宽的适应方式
        QTableWidget.resizeRowsToContents(self.table)  # 设置行高与内容匹配

        for i in range(len(data)):
            for j in range(len(data[0])):
                temp_data = data[i][j]  # 临时记录，不能直接插入表格
                sql1 = QTableWidgetItem(str(temp_data))  # 转换后可插入表格
                self.table.setItem(i, j, sql1)  # 将内容填入表格
        cur.close()
        db.close()

    def count1(self):
        db = pymysql.connect("localhost", "text", "text", "hostel")
        cur = db.cursor()
        cur.execute("select * from service ;")
        data = cur.fetchall()
        print(data)
        count = 0
        for i in range(len(data)):
            if data[i][3] == '是':
                count += 1
        QMessageBox.information(self, "统计", "一共有" + str(len(data)) + "个报修情况.\n"+"已经解决了" + str(count) + "个")


    # 菜单栏
    def createMenus(self):
        # 创建动作 注销
        self.printAction1 = QAction(self.tr("注销"), self)
        self.printAction1.triggered.connect(self.on_printAction1_triggered)

        # 创建动作 退出
        self.printAction2 = QAction(self.tr("退出"), self)
        self.printAction2.triggered.connect(self.on_printAction2_triggered)

        # 创建菜单，添加动作
        self.printMenu = self.menuBar().addMenu(self.tr("注销和退出"))
        self.printMenu.addAction(self.printAction1)
        self.printMenu.addAction(self.printAction2)

    # 注销
    def on_printAction1_triggered(self):
        self.close()
        dialog = logindialog()
        if dialog.exec_() == QDialog.Accepted:
            the_window = MainWindow()
            self.windowList.append(the_window)  # 这句一定要写，不然无法重新登录
            the_window.show()

    # 退出
    def on_printAction2_triggered(self):
        self.close()

    # 关闭界面触发事件
    def closeEvent(self, event):
        pass


class logindialog(QDialog):
    def __init__(self, *args, **kwargs):

        super().__init__(*args, **kwargs)

        self.setWindowTitle('登录界面')
        self.resize(300, 200)
        self.setFixedSize(self.width(), self.height())
        self.setWindowFlags(Qt.WindowCloseButtonHint)

        # 设置界面控件
        self.frame = QFrame(self)
        self.verticalLayout = QVBoxLayout(self.frame)

        self.lineEdit_account = QLineEdit()
        self.lineEdit_account.setPlaceholderText("请输入账号")
        self.verticalLayout.addWidget(self.lineEdit_account)

        self.lineEdit_password = QLineEdit()
        self.lineEdit_password.setPlaceholderText("请输入密码")
        self.verticalLayout.addWidget(self.lineEdit_password)

        self.pushButton_enter = QPushButton()
        self.pushButton_enter.setText("确定")
        self.verticalLayout.addWidget(self.pushButton_enter)

        self.pushButton_quit = QPushButton()
        self.pushButton_quit.setText("取消")
        self.verticalLayout.addWidget(self.pushButton_quit)

        # 绑定按钮事件
        self.pushButton_enter.clicked.connect(self.on_pushButton_enter_clicked)
        self.pushButton_quit.clicked.connect(QCoreApplication.instance().quit)

    def on_pushButton_enter_clicked(self):
        # 账号与密码正确性判断
        account = int(self.lineEdit_account.text())
        password = int(self.lineEdit_password.text())
        u = 0
        db = pymysql.connect("localhost", "text", "text", "hostel")
        # 使用 cursor() 方法创建一个游标对象 cursor
        cur = db.cursor()
        cur.execute('select * from admin')
        data = cur.fetchall()
        for i in range(len(data)):
            if account == data[i][0] and password == data[i][1]:
                u = 1
                cur.close()
                db.close()
                # 通过验证，关闭对话框并返回1
                self.accept()
        if u == 0:
            print("账号或密码错误")
            QMessageBox.information(self, "警告", "账号或密码错误")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    dialog = logindialog()
    if dialog.exec_() == QDialog.Accepted:
        the_window = MainWindow()
        the_window.show()
        sys.exit(app.exec_())
