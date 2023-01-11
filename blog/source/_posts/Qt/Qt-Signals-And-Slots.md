---
title: Qt Connect
date: 2022-07-29 17:26:28
categories:
- develop
tags:
- cpp
- Qt
---

<p>
&ensp;&ensp;<code>Qt</code>信号与槽...
</p>

<!-- more -->

# `Qt` 
- 是非常好的用于开发软件界面的库, 当然我这样说有些狭隘, `Qt`能做的事情远不止如此
- 但本文我只是说一下`Qt`的信号槽机制

## 1. `Qt`超级经典的信号与槽机制- `signal` & `slot`
- 示例
    ```cpp
        class Use_SignalAndSlot:public QObject
        {
            Q_OBJECT
        public:
        // ......
        signals:
            void useSignalsAndSlots();
        slots: // (槽函数可以不是slots下的函数)，可以是public or private or protected 下的函数
            void OnuseSignalsAndSlots();
        // ......
        }
    ```

- `class` 必须是继承自 `QObject`
- 要使用`Qt`的这个机制需要在`Class`的定义处添加`Q_OBJECT`的宏定义x
- 定义信号是必须加上- `signals:` 前缀, 且信号不需要实现只需要定义，`Qt`有自己的解析机制

### 1. 信号槽的不同写法 - 官方介绍

#### `Qt4`
来自官方的文档
- 使用宏包裹
    ```cpp
    QMetaObject::Connection QObject::connect(const QObject *sender, const char *signal, const QObject *receiver, const char *method, Qt::ConnectionType type = Qt::AutoConnection)
    ```
    <p> 
    &ensp;&ensp;Creates a connection of the given type from the signal in the sender object to the method in the receiver object. Returns a handle to the connection that can be used to disconnect it later.<br>
    &ensp;&ensp;You must use the <code> SIGNAL() </code> and <code>SLOT()</code> macros when specifying the signal and the method, for example:
    </p>
    
-  上面函数创建一个链接，并将这个链接作为返回值,这个返回值可以用于调用 `disconnect` 断开链接
- example:
    ```cpp

    QLabel *label = new QLabel;
    QScrollBar *scrollBar = new QScrollBar;
    QObject::connect(scrollBar, SIGNAL(valueChanged(int)),label,  SLOT(setNum(int)));

    ```
- <font color=red id="danger">友情提示：请记住这种写法，必须使用<code>SIGNAL</code>&<code>SLOT</code>将对应的信号和槽函数包裹起来，并且这种方法无法检测对应的信号和槽函数是否存在</font>
- 在创建链接的时候，对应的信号中我们只需要给出参数的类型，不需要写出具体的参数名称:
    <p>
    &ensp;&ensp;This example ensures that the label always displays the current scroll bar value. Note that the signal and slots parameters must not contain any variable names, only the type. E.g. the following would not work and return false:
    </p>

    ```cpp
    // WRONG
    QObject::connect(scrollBar, SIGNAL(valueChanged(int value)),label, SLOT(setNum(int value)));
    ```
- overloads - 1

    ```cpp
    QMetaObject::Connection QObject::connect(const QObject *sender, const QMetaMethod &signal, const QObject *receiver, const QMetaMethod &method, Qt::ConnectionType type = Qt::AutoConnection)
    ```
    <p>
    &ensp;&ensp;Creates a connection of the given type from the signal in the sender object to the method in the receiver object. Returns a handle to the connection that can be used to disconnect it later.<br>
    &ensp;&ensp;The Connection handle will be invalid if it cannot create the connection, for example, the parameters were invalid. You can check if the <code>QMetaObject::Connection</code> is valid by casting it to a bool.<br>
    &ensp;&ensp;This function works in the same way as <code>connect(const QObject *sender, const char *signal, const QObject *receiver, const char *method, Qt::ConnectionType type)</code> but it uses <code>QMetaMethod</code> to specify signal and method.<br>
    This function was introduced in Qt 4.8.
    </p>

- overloads - 2

    ```cpp
    QMetaObject::Connection QObject::connect(const QObject *sender, const char *signal, const char *method, Qt::ConnectionType type = Qt::AutoConnection) const
    ```
    <p>
        &ensp;&ensp;This function overloads <code>connect()</code>.<br>
        &ensp;&ensp;Connects signal from the sender object to this object's method.<br>
        &ensp;&ensp;Equivalent to <code>connect(sender, signal, <font color=red>this</font>, method, type)</code>.<br>
        &ensp;&ensp;Every connection you make emits a signal, so duplicate connections emit two signals. You can break a connection using <code>disconnect()</code>.<br>
        &ensp;&ensp;Note: This function is <font color=#00ff00>thread-safe</font>. <br>
        <font color=red>&ensp;&ensp;友情提示： 默认指定this作为接收者</font>
    </p>


#### `Qt5`之后    
- 新的写法
    ```cpp
    template <typename PointerToMemberFunction> QMetaObject::Connection QObject::connect(const QObject *sender, PointerToMemberFunction signal, const QObject *receiver, PointerToMemberFunction method, Qt::ConnectionType type =  Qt::AutoConnection)
    ```

- <code>example</code>
    ```cpp
    QLabel *label = new QLabel;
    QLineEdit *lineEdit = new QLineEdit;
    QObject::connect(lineEdit, &QLineEdit::textChanged,label,  &QLabel::setText);
    ```
- 请注意信号和槽函数的参数必须是匹配的

- overloads - 1

    ```cpp
    template <typename PointerToMemberFunction, typename Functor> 
    QMetaObject::Connection QObject::connect(const QObject *sender,PointerToMemberFunction signal, Functor functor)
    // 这是个重载的函数

    ```

    - Example

    ``` 
    void someFunction();
    QPushButton *button = new QPushButton;
    QObject::connect(button, &QPushButton::clicked, someFunction);
    ```

    - Lambda expressions can also be used:

    ```cpp
    QByteArray page = ...;
    QTcpSocket *socket = new QTcpSocket;
    socket->connectToHost("qt-project.org", 80);
    QObject::connect(socket, &QTcpSocket::connected, [=] () {
            socket->write("GET " + page + "\r\n");
    });
    ``` 

    <p>
    &ensp;&ensp;The connection will automatically disconnect if the sender is destroyed. However, you should take care that any objects used within the functor are still alive when the signal is emitted.<br>
    &ensp;&ensp;Overloaded functions can be resolved with help of qOverload.<br>
    &ensp;&enspNote: This function is thread-safe.<br>
    </p>

- overloads - 2

    ```cpp

    template <typename PointerToMemberFunction, typename Functor> QMetaObject::Connection 
    QObject::connect(const QObject *sender, PointerToMemberFunction signal, const QObject *context, 
                    Functor functor, Qt::ConnectionType type = Qt::AutoConnection)

    ```

    <p>
    &ensp;&ensp;This function overloads connect().<br>
    &ensp;&ensp;Creates a connection of a given type from signal in sender object to functor to be placed in a specific event loop of context, and returns a handle to the connection.<br>
    &ensp;&ensp;Note: <code>Qt::UniqueConnections</code> do not work for lambdas, non-member functions and functors; they only apply to connecting to member functions.<br>
    &ensp;&ensp;The signal must be a function declared as a signal in the header. The slot function can be any function or functor that can be connected to the signal. A function can be connected to a given signal if the signal has at least as many argument as the slot. A functor can be connected to a signal if they have exactly the same number of arguments. There must exist implicit conversion between the types of the corresponding arguments in the signal and the slot.
    </p>

    - Example:

    ```cpp
    void someFunction();
    QPushButton *button = new QPushButton;
    QObject::connect(button, &QPushButton::clicked, this, someFunction, Qt::QueuedConnection);
    ```
    
    - Lambda expressions can also be used:
    
    ```cpp
    QByteArray page = ...;
    QTcpSocket *socket = new QTcpSocket;
    socket->connectToHost("qt-project.org", 80);
    QObject::connect(socket, &QTcpSocket::connected, this, [=] () {
            socket->write("GET " + page + "\r\n");
        }, Qt::AutoConnection);
     ```
     
    <p> 
    &ensp;&ensp;The connection will automatically disconnect if the sender or the context is destroyed. However, you should take care that any objects used within the functor are still alive when the signal is emitted.<br>
    &ensp;&ensp;Overloaded functions can be resolved with help of qOverload.<br>
    &ensp;&ensp;Note: This function is thread-safe.<br>
    &ensp;&ensp;This function was introduced in Qt 5.2.
    </p>

### 2. 信号槽的不同写法 - 几种一般写法    

##### `connect`的最后一个参数，我们暂时使用他的默认认为

####  `Qt4的写法`
- 宏包裹
    ```cpp
    QPushButton *btn = new QPushButton;
    connect(btn, SIGNAL(clicked()), this, SLOT(close()));
    ```
- [注意事项](#danger)

#### `Qt5`后的写法

- 模板匹配

    ```cpp 
    QPushButton *btn = new QPushButton; 
    connect(btn, &QPushButton::clicked, this, &MainWindow::close);
    ```
    
    <p> Qt5后的官方推荐写法，编译的时候信号或槽不存在是无法编译通过的，槽的可以直接写在<code>public or protected or private</code>下
    </p>

- lambda

    ```cpp
    connect(btn, &QPushButton::clicked, [&]() {
    this->close();
    });

    connect(btn, &QPushButton::clicked, this, [&]() {
    this->close();
    });

    ```  

    - 两种写法本质上是一样的，只是说第一种默认指定接收者为<code>this</code> 
    - 请保证`lambda`函数中使用成员都是活跃的，不然将是很糟糕的行为


### 3. connect 最后一个参数

- `Qt::AutoConnection` 
    - 默认使用的参数
    <div>
    <p>(Default) If the receiver lives in the thread that emits the signal, Qt::DirectConnection is used. Otherwise, Qt::QueuedConnection is used. The connection type is determined when the signal is emitted.
    </p>
    <p>
    如果接收器位于发出信号的线程中，则使用 Qt::DirectConnection。 否则，使用 Qt::QueuedConnection。 连接类型在信号发出时确定
    </p>
    </div>
- `Qt::DirectConnection`

    <div>
    <p>
    The slot is invoked immediately when the signal is emitted. The slot is executed in the signalling thread.
    </p>
    <p>
    发出信号时立即调用插槽。 该槽函数在发送者线程中执行。
    </p>
    </div>

- `Qt::QueuedConnection`

    <div>
    <p>
    The slot is invoked when control returns to the event loop of the receiver's thread. The slot is executed in the receiver's thread.
    </p>
    <p>
    当控制返回到接收者线程的事件循环时调用该槽,会等待当前函数执行结束，重新回到事件循环。 槽函数在接收者的线程中执行.
    </p>
    </div>

- `Qt::BlockingQueuedConnection`
    <div>
    <p>
    Same as Qt::QueuedConnection, except that the signalling thread blocks until the slot returns. This connection must not be used if the receiver lives in the signalling thread, or else the application will deadlock.
    </p>
    <p>
    与 Qt::QueuedConnection 相同，只是信号线程阻塞直到槽返回。 如果接收者与发送者在一个线程中，则不得使用此连接，否则应用程序将死锁
    </p>
    </div>

- `Qt::UniqueConnection`
    <div>
    <p>
    This is a flag that can be combined with any one of the above connection types, using a bitwise OR. When Qt::UniqueConnection is set, QObject::connect() will fail if the connection already exists (i.e. if the same signal is already connected to the same slot for the same pair of objects). This flag was introduced in Qt 4.6.
    </p>
    <p>
    这是一个可以与上述任何一种连接类型结合使用的标志，使用按位或。 当设置了 Qt::UniqueConnection 时，如果连接已经存在（即，如果相同的信号已经连接到同一对对象的同一槽函数），则 QObject::connect() 将失败。 这个标志是在 Qt 4.6 中引入的。
    </p>
    </div>








