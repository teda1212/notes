# 结合AI 使用 GUI编程实现一个黑马人事管理系统



第一步：创建一个模块做这个项目。itheima-employee-sys

第二步：分析项目的业务需求。

* 提供一个登录和注册用户的界面。

* 提供一个**人事信息的管理界面**：**展示**全部员工信息，提供一个根据名称**查询**某个员工信息展示，**增、删、改**员工信息。

* 分析项目的角色：  

  ​	--  登录用户： 登录名称,  密码

  ​	--  员工信息： ID, 姓名,性别，年龄，电话，职位，入职时间，薪水，部门信息。

* 分析系统的界面：

  ​	--  登录界面类：创建登录界面。

  ​	--  信息管理界面类：创建管理界面。

第三步：结合AI开始开发： 通义千问  

* AI帮我们生成一个登录界面。
* AI帮我们生成一个信息管理界面。

## 正式开发

### 1、拿到登录界面的程序。

```java
package com.itheima.ui;

import javax.swing.*;
import java.awt.*;
// 自定义登录界面
public class LoginUI extends JFrame{
    private JTextField usernameField; // 用户名输入框
    private JPasswordField passwordField; // 密码输入框
    private JButton loginButton; // 登录按钮
    private  JButton registerButton; // 注册按钮

    public LoginUI(){
        super("登录界面");
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setSize(400, 400);
        this.setLocationRelativeTo(null); // 居中显示

        createAndShowGUI();
    }

    private  void createAndShowGUI() {
        // 创建主窗口
        // 创建面板
        JPanel panel = new JPanel();
        panel.setLayout(null);
        panel.setBackground(new Color(240, 240, 240));

        // 设置字体和颜色
        Font customFont = new Font("楷体", Font.BOLD, 18);
        Color primaryColor = new Color(66, 135, 245);
        Color secondaryColor = new Color(204, 204, 204); // 更浅的颜色用于注册按钮
        
        // 标题
        JLabel titleLabel = new JLabel("黑马程序员人事管理系统");
        titleLabel.setBounds(50, 30, 300, 30);
        titleLabel.setFont(new Font("楷体", Font.BOLD, 24));
        panel.add(titleLabel);

        // 用户名标签
        JLabel usernameLabel = new JLabel("用户名：");
        usernameLabel.setBounds(50, 100, 150, 30);
        usernameLabel.setFont(customFont);
        panel.add(usernameLabel);

        // 用户名输入框
        usernameField = new JTextField();
        usernameField.setBounds(160, 100, 190, 30);
        usernameField.setFont(customFont);
        panel.add(usernameField);

        // 密码标签
        JLabel passwordLabel = new JLabel("密   码：");
        passwordLabel.setBounds(50, 150, 150, 30);
        passwordLabel.setFont(customFont);
        panel.add(passwordLabel);

        // 密码输入框
        passwordField = new JPasswordField();
        passwordField.setBounds(160, 150, 190, 30);
        passwordField.setFont(customFont);
        panel.add(passwordField);

        // 登录按钮
        loginButton = new JButton("登   录");
        loginButton.setBounds(50, 200, 150, 30);
        loginButton.setFont(customFont);
        loginButton.setBackground(primaryColor);
        loginButton.setForeground(Color.WHITE);
        panel.add(loginButton);

        // 注册按钮
        registerButton = new JButton("注   册");
        registerButton.setBounds(200, 200, 150, 30);
        registerButton.setFont(customFont);
        registerButton.setBackground(secondaryColor);
        registerButton.setForeground(Color.BLACK);
        panel.add(registerButton);

        // 添加面板到窗口
        this.add(panel);
        this.setVisible(true);
    }
}
```

定义了一个APP的启动类：

```java
public class App {
    public static void main(String[] args) {
        new LoginUI();
    }
}
```

### 2、拿到管理信息的界面

提供一个人事信息的管理界面：展示全部员工信息，提供一个根据名称查询某个员工信息展示，添加员工信息，删除员工信息，修改员工信息。

AI生成的原版代码：

```java
public class EmployeeManagerUI {
    private JFrame frame;
    private JTable table;
    private DefaultTableModel model;
    private JTextField textFieldSearch;

    public static void main(String[] args) {
        EmployeeManagerUI window = new EmployeeManagerUI();
        window.frame.setVisible(true);
    }

    public EmployeeManagerUI() {
        initialize();
    }

    private void initialize() {
        frame = new JFrame();
        frame.setBounds(100, 100, 800, 600);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.getContentPane().setLayout(new BorderLayout());

        // 输入框和搜索按钮
        JPanel topPanel = new JPanel();
        topPanel.setLayout(new FlowLayout(FlowLayout.CENTER));
        textFieldSearch = new JTextField(20);
        JButton btnSearch = new JButton("搜索");
        JButton btnAdd = new JButton("添加");
        topPanel.add(textFieldSearch);
        topPanel.add(btnSearch);
        topPanel.add(btnAdd);

        // 创建表格模型
        model = new DefaultTableModel(
                new Object[][]{},
                new String[]{"ID", "姓名", "职位", "部门"}
        ) {
            @Override
            public boolean isCellEditable(int row, int column) {
                return false; // 设置所有单元格为不可编辑
            }
        };

        table = new JTable(model);
        JScrollPane scrollPane = new JScrollPane(table);
        table.setRowHeight(30);

        // 添加数据到表格
        for (int i = 0; i < 20; i++) {
            model.addRow(new Object[]{i + 1, "员工" + (i + 1), "职位" + (i + 1), "部门" + (i + 1)});
        }

        // 右键菜单
        JPopupMenu popupMenu = new JPopupMenu();
        JMenuItem editItem = new JMenuItem("编辑");
        JMenuItem deleteItem = new JMenuItem("删除");
        popupMenu.add(editItem);
        popupMenu.add(deleteItem);
        table.addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                if (e.getButton() == MouseEvent.BUTTON3) { // 明确检查是否为鼠标右键
                    int row = table.rowAtPoint(e.getPoint());
                    if (row >= 0) {
                        table.setRowSelectionInterval(row, row);
                        popupMenu.show(table, e.getX(), e.getY());
                    }
                }
            }
        });

        // 绑定事件到菜单项
        editItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedRow = table.getSelectedRow();
                if (selectedRow >= 0) {
                    int id = (Integer) model.getValueAt(selectedRow, 0); // 假设ID在第一列
                    JOptionPane.showMessageDialog(frame, "删除 ID: " + id);
                    // 这里可以添加编辑逻辑，比如打开一个新的对话框让用户修改数据
                }
            }
        });

        deleteItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedRow = table.getSelectedRow();
                if (selectedRow >= 0) {
                    int id = (Integer) model.getValueAt(selectedRow, 0); // 假设ID在第一列
                    JOptionPane.showMessageDialog(frame, "删除 ID: " + id);
                    // 这里可以添加删除逻辑，比如从模型中移除该行
                }
            }
        });

        // 搜索按钮监听器
        btnSearch.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String searchValue = textFieldSearch.getText();
                // 这里可以添加搜索逻辑
            }
        });

        // 添加按钮监听器
        btnAdd.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // 这里可以添加添加新员工的逻辑
            }
        });

        frame.getContentPane().add(topPanel, BorderLayout.NORTH);
        frame.getContentPane().add(scrollPane, BorderLayout.CENTER);
    }
}
```

**改造成自定义的信息管理界面。**

```java
package com.itheima.ui;

import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.util.Date;

public class EmployeeManagerUI extends JFrame{
    private JFrame frame; // 窗口
    private JTable table;  // 表格
    private DefaultTableModel model; // 表格模型: 封装表格数据的对象
    private JTextField nameTextFieldSearch; // 搜索输入框

    public EmployeeManagerUI() {
        frame = this;
        initialize();
        this.setVisible(true); // 显示窗口
    }

    private void initialize() {
        this.setBounds(100, 100, 800, 600);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.getContentPane().setLayout(new BorderLayout());

        // 输入框和搜索按钮
        JPanel topPanel = new JPanel();
        topPanel.setLayout(new FlowLayout(FlowLayout.CENTER));
        nameTextFieldSearch = new JTextField(20);
        JButton btnSearch = new JButton("搜索");
        JButton btnAdd = new JButton("添加");
        topPanel.add(nameTextFieldSearch);
        topPanel.add(btnSearch);
        topPanel.add(btnAdd);

        // 创建表格模型
        model = new DefaultTableModel(
                new Object[][]{},
                new String[]{"ID", "姓名", "性别", "年龄", "电话" ,"职位", "入职日期", "薪水", "部门"}
        ){
            @Override
            public boolean isCellEditable(int row, int column) {
                return false; // 设置所有单元格为不可编辑
            }
        };

        table = new JTable(model);
        JScrollPane scrollPane = new JScrollPane(table);
        table.setRowHeight(30);

        // 添加数据到表格
        for (int i = 0; i < 20; i++) {
            model.addRow(new Object[]{i + 1, "员工" + (i + 1),  "男",  21, "18667656520", "java工程师", new Date().toLocaleString(), 30000, "部门" + (i + 1)});
        }

        // 右键菜单
        JPopupMenu popupMenu = new JPopupMenu();
        JMenuItem editItem = new JMenuItem("编辑");
        JMenuItem deleteItem = new JMenuItem("删除");
        popupMenu.add(editItem);
        popupMenu.add(deleteItem);
        table.addMouseListener(new MouseAdapter() {
            @Override
            public void mousePressed(MouseEvent e) {
                if (e.getButton() == MouseEvent.BUTTON3) { // 明确检查是否为鼠标右键
                    int row = table.rowAtPoint(e.getPoint());
                    if (row >= 0) {
                        table.setRowSelectionInterval(row, row);
                        popupMenu.show(table, e.getX(), e.getY());
                    }
                }
            }
        });

        // 绑定事件到菜单项
        editItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedRow = table.getSelectedRow();
                if (selectedRow >= 0) {
                    int id = (Integer) model.getValueAt(selectedRow, 0); // 假设ID在第一列
                    JOptionPane.showMessageDialog(frame, "删除 ID: " + id);
                    // 这里可以添加编辑逻辑，比如打开一个新的对话框让用户修改数据
                }
            }
        });

        deleteItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedRow = table.getSelectedRow();
                if (selectedRow >= 0) {
                    int id = (Integer) model.getValueAt(selectedRow, 0); // 假设ID在第一列
                    JOptionPane.showMessageDialog(frame, "删除 ID: " + id);
                    // 这里可以添加删除逻辑，比如从模型中移除该行
                }
            }
        });

        // 搜索按钮监听器
        btnSearch.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String searchValue = nameTextFieldSearch.getText();
                // 这里可以添加搜索逻辑
            }
        });

        // 添加按钮监听器
        btnAdd.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                // 这里可以添加添加新员工的逻辑
            }
        });

        frame.getContentPane().add(topPanel, BorderLayout.NORTH);
        frame.getContentPane().add(scrollPane, BorderLayout.CENTER);
    }
}
```

### 3、准备系统的角色类

* 用户类，员工信息类。

```java
package com.itheima.bean;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

// 用户信息包含用户名，密码，登录名。
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private String username; // 用户名 
    private String password; // 密码
    private String loginName; // 登录名
}
```

```java
package com.itheima.bean;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

// 员工信息："ID", "姓名", "性别", "年龄", "电话" ,"职位", "入职日期", "薪水", "部门"
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Employee {
    private int id;
    private String name;
    private String sex;
    private int age;
    private String phone;
    private String position;
    private String entryDate;
    private double salary;
    private String department;
}
```

### 4、开发登录功能

系统中需要存在用户信息，大家可以先开发注册，注册一些用户信息（这个交给大家自己做）

可以在系统直接准备一些用户对象，作为正确的用户信息。

准备一个集合容器，存储系统中全部的用户信息。这个集合只需要一个。

```java
// 定义一个静态的集合，存储系统中全部的用户对象信息。
private static ArrayList<User> allUsers = new ArrayList<>();

// 初始化几个测试的用户对象信息，作为登录用。
static {
    allUsers.add(new User("超级管理员", "123456", "admin"));
    allUsers.add(new User("赵敏", "wuji520", "minmin"));
    allUsers.add(new User("周杰伦", "jaychou", "jay"));
}
```

**开发步骤：**

1、给登录按钮绑定一个点击事件监听器

2、一旦点击登录按钮，拿到输入框输入的用户名和密码

3、去集合中看是否存在这个用户对象，如果认证通过，跳转到信息管理界面。登录失败，弹出提示。

```java
package com.itheima.ui;

import com.itheima.bean.User;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

// 自定义登录界面
public class LoginUI extends JFrame implements ActionListener {
    private JTextField loginNameField; // 用户名输入框
    private JPasswordField passwordField; // 密码输入框
    private JButton loginButton; // 登录按钮
    private  JButton registerButton; // 注册按钮

    // 定义一个静态的集合，存储系统中全部用户的对象
    private static ArrayList<User> allUsers = new ArrayList<>();

    static {
        allUsers.add(new User("超级管理员", "123456", "admin"));
        allUsers.add(new User("游朝政", "123456", "newbee"));
        allUsers.add(new User("周杰伦", "123456", "jay"));
    }

    public LoginUI(){
        super("登录界面");
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setSize(400, 400);
        this.setLocationRelativeTo(null); // 居中显示

        createAndShowGUI();
    }

    private  void createAndShowGUI() {
        // 创建主窗口
        // 创建面板
        JPanel panel = new JPanel();
        panel.setLayout(null);
        panel.setBackground(new Color(240, 240, 240));

        // 设置字体和颜色
        Font customFont = new Font("楷体", Font.BOLD, 18);
        Color primaryColor = new Color(66, 135, 245);
        Color secondaryColor = new Color(204, 204, 204); // 更浅的颜色用于注册按钮

        // 标题
        JLabel titleLabel = new JLabel("黑马程序员人事管理系统");
        titleLabel.setBounds(50, 30, 300, 30);
        titleLabel.setFont(new Font("楷体", Font.BOLD, 24));
        panel.add(titleLabel);

        // 用户名标签
        JLabel usernameLabel = new JLabel("用户名：");
        usernameLabel.setBounds(50, 100, 150, 30);
        usernameLabel.setFont(customFont);
        panel.add(usernameLabel);

        // 用户名输入框
        loginNameField = new JTextField();
        loginNameField.setBounds(160, 100, 190, 30);
        loginNameField.setFont(customFont);
        panel.add(loginNameField);

        // 密码标签
        JLabel passwordLabel = new JLabel("密   码：");
        passwordLabel.setBounds(50, 150, 150, 30);
        passwordLabel.setFont(customFont);
        panel.add(passwordLabel);

        // 密码输入框
        passwordField = new JPasswordField();
        passwordField.setBounds(160, 150, 190, 30);
        passwordField.setFont(customFont);
        passwordField.setEchoChar('*');
        panel.add(passwordField);

        // 登录按钮
        loginButton = new JButton("登   录");
        loginButton.setBounds(50, 200, 150, 30);
        loginButton.setFont(customFont);
        loginButton.setBackground(primaryColor);
        loginButton.setForeground(Color.WHITE);
        panel.add(loginButton);
        loginButton.addActionListener(this);


        // 注册按钮
        registerButton = new JButton("注   册");
        registerButton.setBounds(200, 200, 150, 30);
        registerButton.setFont(customFont);
        registerButton.setBackground(secondaryColor);
        registerButton.setForeground(Color.BLACK);
        panel.add(registerButton);
        registerButton.addActionListener(this);

        // 添加面板到窗口
        this.add(panel);
        this.setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        // 这里可能是登录按钮的点击事件，也可能是注册按钮的点击事件
        // 判断点击的是哪个按钮
        JButton btn = (JButton) e.getSource();// getSource返回Object，强转为JButton
        if (btn == loginButton) {
            // 独立功能独立成方法
            login();
        }else {
            System.out.println("注册按钮被点击了");
        }
    }

    private void login() {
        // 1、获取用户输入的用户名和密码
        String loginName = loginNameField.getText();
        String password = new String(passwordField.getPassword());
        // 2、遍历集合，判断用户输入的用户名和密码是否存在于集合中
        // 3、根据登录名称去查询用户对象返回，查询到用户对象，说明登录名称正确了
        User user = getUserByLoginName(loginName);
        if (user != null) {
            if (user.getPassword().equals(password)) {
                // 4、登录成功，跳转到员工管理界面
                new EmployeeManagerUI(user.getUsername());
                this.dispose();
            }else {
                // 5、登录失败，弹出提示框
                JOptionPane.showMessageDialog(this, "密码错误，请重新输入");
            }
        }else {
            JOptionPane.showMessageDialog(this, "用户名不存在，请重新输入");
        }
    }

    // 根据登录名称查询用户对象，查询到用户对象，说明登录名称正确了，使用for循环根据索引遍历
    private User getUserByLoginName(String loginName) {
        for (User user : allUsers) {
            if (user.getLoginName().equals(loginName)) {
                return user;
            }
        }
        return null;// 没有查询到用户对象，说明登录名称不正确。
    }
}
```

### 5、完成信息管理界面的功能开发（核心功能）

1、登录成功后，把登录成功的用户名传给信息管理界面。

2、添加员工信息的功能。

​      --  准备一个静态的集合，用于存储系统中全部的员工信息。

​      --  给添加按钮绑定点击事件。

```java
// 添加按钮监听器
btnAdd.addActionListener(e -> {
    // 弹出一个添加员工信息的界面出来。
    new AddEmployeeUI();
});
```

​      --  点击添加按钮后，弹出一个添加员工信息的输入框。

```java
package com.itheima.ui;

import javax.swing.*;
import java.awt.*;
import java.text.SimpleDateFormat;

/**
 * AddEmployeeUI 类用于创建添加员工信息的用户界面
 * 继承自 JFrame，包含多个文本字段用于输入员工的详细信息，以及添加和取消操作的按钮
 */
public class AddEmployeeUI extends JFrame{
    // 定义多个 JTextField 用于输入员工的各种信息
    private JTextField txtId, txtName, txtSex, txtAge, txtPhone, txtPosition, txtSalary, txtDepartment;
    // 使用 JFormattedTextField 限制入职日期的输入格式
    private JFormattedTextField txtHireDate;
    // 定义添加和取消按钮
    private JButton btnSave, btnCancel;

    /**
     * 构造方法用于初始化用户界面
     */
    public AddEmployeeUI() {
        // 初始化这个界面，提供很多很多输入框信息
        // ID，姓名，性别，年龄，手机号，职位，入职时间，工资，部门
        // 提供一个添加按钮和一个取消按钮
        super("添加员工信息");
        setLayout(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.fill = GridBagConstraints.HORIZONTAL;
        gbc.insets = new Insets(5, 5, 5, 5);

        // 设置标签字体
        Font labelFont = new Font("楷体", Font.PLAIN, 14);

        // 以下代码块用于创建和添加员工信息输入字段
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.anchor = GridBagConstraints.EAST;
        JLabel idLabel = new JLabel("ID:");
        idLabel.setFont(labelFont);
        add(idLabel, gbc);

        gbc.gridx = 1;
        gbc.anchor = GridBagConstraints.WEST;
        txtId = new JTextField(10);
        add(txtId, gbc);

        // 姓名输入字段
        gbc.gridx = 0;
        gbc.gridy = 1;
        JLabel nameLabel = new JLabel("姓名:");
        nameLabel.setFont(labelFont);
        add(nameLabel, gbc);

        gbc.gridx = 1;
        txtName = new JTextField(10);
        add(txtName, gbc);

        // 性别输入字段
        gbc.gridx = 0;
        gbc.gridy = 2;
        JLabel sexLabel = new JLabel("性别:");
        sexLabel.setFont(labelFont);
        add(sexLabel, gbc);

        gbc.gridx = 1;
        txtSex = new JTextField(10);
        add(txtSex, gbc);

        // 年龄输入字段
        gbc.gridx = 0;
        gbc.gridy = 3;
        JLabel ageLabel = new JLabel("年龄:");
        ageLabel.setFont(labelFont);
        add(ageLabel, gbc);

        gbc.gridx = 1;
        txtAge = new JTextField(10);
        add(txtAge, gbc);

        // 电话输入字段
        gbc.gridx = 0;
        gbc.gridy = 4;
        JLabel phoneLabel = new JLabel("电话:");
        phoneLabel.setFont(labelFont);
        add(phoneLabel, gbc);

        gbc.gridx = 1;
        txtPhone = new JTextField(10);
        add(txtPhone, gbc);

        // 职位输入字段
        gbc.gridx = 0;
        gbc.gridy = 5;
        JLabel positionLabel = new JLabel("职位:");
        positionLabel.setFont(labelFont);
        add(positionLabel, gbc);

        gbc.gridx = 1;
        txtPosition = new JTextField(10);
        add(txtPosition, gbc);

        // 入职日期输入字段，使用 SimpleDateFormat 设置日期格式
        gbc.gridx = 0;
        gbc.gridy = 6;
        JLabel hireDateLabel = new JLabel("入职日期:");
        hireDateLabel.setFont(labelFont);
        add(hireDateLabel, gbc);

        gbc.gridx = 1;
        txtHireDate = new JFormattedTextField(new SimpleDateFormat("yyyy-MM-dd"));
        add(txtHireDate, gbc);

        // 薪水输入字段
        gbc.gridx = 0;
        gbc.gridy = 7;
        JLabel salaryLabel = new JLabel("薪水:");
        salaryLabel.setFont(labelFont);
        add(salaryLabel, gbc);

        gbc.gridx = 1;
        txtSalary = new JTextField(10);
        add(txtSalary, gbc);

        // 部门输入字段
        gbc.gridx = 0;
        gbc.gridy = 8;
        JLabel departmentLabel = new JLabel("部门:");
        departmentLabel.setFont(labelFont);
        add(departmentLabel, gbc);

        gbc.gridx = 1;
        txtDepartment = new JTextField(10);
        add(txtDepartment, gbc);

        // 添加和取消按钮
        gbc.gridx = 0;
        gbc.gridy = 9;
        gbc.gridwidth = 2;
        gbc.anchor = GridBagConstraints.CENTER;
        gbc.fill = GridBagConstraints.NONE;
        btnSave = new JButton("添加");
        btnCancel = new JButton("取消");
        btnSave.setPreferredSize(new Dimension(100, 30));
        btnCancel.setPreferredSize(new Dimension(100, 30));
        JPanel buttonPanel = new JPanel();
        buttonPanel.add(btnSave);
        buttonPanel.add(btnCancel);
        add(buttonPanel, gbc);

        // 设置窗口属性
        pack();// 自适应窗口大小
        setSize(300, 400);
        setLocationRelativeTo(null);
        setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        setVisible(true);
    }
}
```

​      --  把员工信息封装成一个员工对象，**存入到系统集合中去**，然后刷新表格，展示最新添加的数据。

```java
// com.itheima.ui.AddEmployeeUI 的构造器改为有参
public AddEmployeeUI(EmployeeManagerUI employeeManagerUI){
}
//接受com.itheima.ui.EmployeeManagerUI的this对象
// 添加按钮监听器
btnAdd.addActionListener(e -> {
    // 添加员工信息
    new AddEmployeeUI(this);
});

// com.itheima.ui.AddEmployeeUI 的构造器中增加添加员工信息的功能
// 添加员工信息，绑定一个点击事件监听器
btnSave.addActionListener(e -> {
    // 获取输入框的数据，封装成员工对象，添加到信息界面的集合并在表格中展示
    Employee employee = new Employee(
        Integer.parseInt(txtId.getText()),
        txtName.getText(),
        txtSex.getText(),
        Integer.parseInt(txtAge.getText()),
        txtPhone.getText(),
        txtPosition.getText(),
        txtHireDate.getText(),
        Double.parseDouble(txtSalary.getText()),
        txtDepartment.getText()
    );
    // 把这个员工对象送到信息界面的集合中，并刷新表格
    employeeManagerUI.addEmployee(employee);
    // 弹出一个提示框，提示添加成功
    JOptionPane.showMessageDialog(this, "添加成功");
    // 关闭当前窗口
    this.dispose();
});
```



### 6、删除某行数据的功能

第一步：拿到当前右键要删除的这一行数据的id值。 1  2

第二步：用id去集合中删除这个员工对象，把数据抹掉。

第三步：从界面上抹掉这一行数据。刷新。

```java
// com.itheima.ui.EmployeeManagerUI
// 删除item绑定监听器
deleteItem.addActionListener(e -> {
    int selectedRow = table.getSelectedRow();
    if (selectedRow >= 0) {
        int id = (Integer) model.getValueAt(selectedRow, 0); // 假设ID在第一列
        JOptionPane.showMessageDialog(frame, "删除 ID: " + id);
        // 这里可以添加删除逻辑，比如从模型中移除该行
        // model.removeRow(selectedRow);//方法1
        deleteEmployee1(id);//方法2
        // 方法3
        // deleteEmployee1(id);
        // model.removeRow(selectedRow)
    }
});

// 在表格中删除
private void deleteEmployee1(int id) {
    // 从表格模型中删除员工信息
    for (int i = 0; i < model.getRowCount(); i++) {
        int rowId = (Integer) model.getValueAt(i, 0);
        if (rowId == id) {
            model.removeRow(i);
            break;
        }
    }
}

// 在集合中删除
private void deleteEmployee2(int id) {
    // 从集合中删除员工信息
    for (int i = 0; i < employee.size(); i++) {
        Employee employee = employees.get(i);
        if (employee.getId() == id) {
            employees.remove(i);
            break;
        }
    }
}
```

