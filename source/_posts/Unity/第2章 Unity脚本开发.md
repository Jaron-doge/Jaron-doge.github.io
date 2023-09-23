---
title: 第2章 Unity脚本开发
date: 2021-4-8 24:00:00
tags: 
categories: Unity
---
## Unity脚本开发

### 2.1 脚本的创建与运行

<!-- more -->

```c#
	public int age;
	public string name;
// Start is called before the first frame update
    void Start()
    {
        Debug.Log("Start");
    }

    // Update is called once per frame
    void Update()
    {
        Debug.Log("Update");
    }
```

#### 2.1.1 脚本注意事项

- 脚本文件名称与类名必须相同
- 初始化信息不能写到脚本构造函数中，而应定义在Unity的事件函数Start中
- 游戏对象中的脚本可以在属性窗口修改属性，public字段直接可以当属性用
- 脚本只能依附于游戏对象或者其他脚本调用才会运行，一个脚本可以赋值给多个游戏对象
- 脚本运行过程中，脚本的参数修改不被保存

### 2.2 脚本的生命周期

脚本的生命周期，就是所挂载到的游戏物体的生命周期

#### 2.2.1 常用生命周期事件

![1.png](https://i.loli.net/2021/04/19/uSVtFyqsZgd3A9T.png)

### 2.3 向量

#### 2.3.1 二维向量(Vector2)

**静态属性**

- down(0, -1)、up、left(1, 0)、right、one(1, 1)、zero

**属性**

- magnitude——长度
- normalized——单位化

**公共方法**

- Normalize——对向量进行单位化

**静态方法**

- Distance——两个向量间的距离

- Angle——两个向量间的角度

#### 2.3.2 三维向量(Vector3)

**静态属性**

- back、forward(0, 0, 1)

**属性**

- magnitude
- normalized

**静态方法**

- Cross(叉乘)	
- Dot(点乘)

- Lerp(线性插值运算)

- MoveTowards(匀速运动)

- Slerp(球形插值运算) 角度+长度

### 2.4 输入按键控制

#### 2.4.1 获取按键方法

- Input.GetKey(); 				// 持续检测
- Input.GetKeyDown();       // 一次
- Input.GetKeyUp();            // 一次

返回 ：键码，保存了物理键盘按键“索引码”。

#### 2.4.2 获取鼠标事件

- Input.GetMouseButton ();			// 持续监测

- Input.GetMouseButtonDown ();  // 一次

- Input.GetMouseButtonUp ();       // 一次

  0	→	左键
  1	→	右键
  2	→	中 键

#### 2.4.3 获取按钮输入

自定义的虚拟按钮

Edit	→	Project Setting	→	Input Manager

- Input.GetButton (“Jump”);

- Input.GetButtonDown (“Jump”);

- Input.GetButtonUp (“Jump”);

#### 2.4.4 获取虚拟轴的值

Edit	→	Project Setting	→	Input Manager

- Input.GetAxis ();			// 返回-1到1之间的浮点数，有渐变效果
- Input.GetAxisRaw ();     // 没有渐变效果，只能返回-1，0，1三个值 

#### 2.4.5 鼠标按键行为

- OnMouseDrag()：当鼠标拖动

- OnMouseDown()：当鼠标按下

- OnMouseUp()：当鼠标抬起

- OnMouseEnter()：当鼠标进入

- OnMouseExit()：当鼠标退出

- OnMouseOver()：当鼠标经过

```c#
void OnMouseEnter() {
	gameObject.GetComponent<MeshRenderer>().material.color 				= Color.red;
}
```

**获取鼠标行为和获取鼠标按键的区别**

- 获取鼠标按键针对鼠标左中右键，而获取鼠标行为不针对
- 获取鼠标按键是一个方法，返回bool类型，而获取鼠标行为是一个时间函数，跟Start和Update同级别

### 2.5 时间控制

![2.png](https://i.loli.net/2021/04/19/BupDtxYKqvAX9I5.png)

#### 2.5.1 应用示例

**Time.deltaTime**

- 计时器：实现每隔多少秒做什么事情

- 控制物体匀速移动或旋转

  每秒移动多少米

  每秒旋转多少度

```c#
// 每秒移动speed米
transform.Translate(Vector3.right* Time.deltaTime * speed);
```

**Time.timeScale**

- 时间流逝的快慢

  1：时间 正常
  2：表示时间流逝加快，是正常的 2 倍
  0：时间停止，游戏暂停

```c#
if(Input.GetKeyDown(KeyCode.Q)) {
	Time.timeScale = 10;
}

if(Input.GetKeyDown(KeyCode.P)) {
	Time.timeScale = 0;
}
```

### 2.6 游戏物体的实例化和销毁

#### 2.6.1 预制体

**|预制体|**：Prefab，预先准备好的物体 可以被重复使用，是 Unity 中很 重要的资源 类型

**创建**

手动创建：从 Hierachy 面板用鼠标拖动游戏物体到 Asserts 资源文件
夹中

脚本动态创建：
Instantiate(Prefabs, transform.position transform.rotation )

**基本操作**

- Select 查找 Hierachy 面板 中预制体实例对象在 Asserts 资源中的位置
- Revert ：丢弃对当前预制体实例对象所做的修改，重置到跟预制体原型一致的状态

- Apply ：将对当前预制体实例对象所做的修改应用到其他预制体实例对象上

**特点**

- 所有的 Prefab 实例都是 Prefab 原型的 克隆，通过脚本实例化生
  成的对象会有 Clone 的标记

- 只要 Prefab 原型发生改变，所有的 Prefab 实例都会发生变化

#### 2.6.2 创建游戏物体

- GameObject go = new GameObject (“Go");

- GameObject.CreatePrimitive(PrimitiveType.Cube);

- 通过 Instantiate 方法实例化创建游戏对象

  - 克隆预制体(prefab)
  - 克隆普通的游戏对象

  Instantiate(Prefabs, transform.position , transform.rotation);

#### 2.6.3 游戏物体的销毁

- GameObject.Destroy(Object);

- GameObject.Destroy(Object, float);    // 延迟销毁

销毁游戏物体时将销毁该物体所有的组件和子物体。

### 2.7 游戏物体的访问和控制

#### 2.7.1 查找游戏物体的方法

1. GameObject.Find("Cube");     // 根据游戏物体的名称全局查找)
2. transform.Find(“G1/G11”);    // 在当前对象的子物体中查找，参数必须指明具体的查找路径
3. GameObject.Find("Cube").transform.GetChild(0));     // 通过父物体查找子物体
4. 通过 tag 标签查找：
  - GameObject.FindWithTag("Player");     // 查找一个物体
  - GameObject.FindGameObjectWithTag(“Player");     // 查找一个物体
  - GameObject.FindGameObjectsWithTag(“Player");     // 查找一批物体

**若游戏物体未激活时(禁用)，方法 1 和 4 将查找不到，方法 2 和 3 可以查找到**

#### 2.7.2 禁用和启用游戏物体

- gameObject.SetActive(true/false);

**访问游戏物体的激活状态**

- gameObject.activeSelf				// 取决于自身是否被禁用
- gameObject.activeInHierarchy  // 物体自身及所有祖先物体是否被禁用

### 2.8 组件的访问和控制

#### 2.8.1 获取组件

- 获取当前游戏对象上的组件

  GetComponent\<BoxCollider>();

- 获取其他游戏对象的组件(先定义public gameObject)

  other.GetComponent\<Transform>();

- 获取所有子物体的某种组件

  GetComponentsInChildren\<Collider>();

- 获取所有父物体的某种组件

  GetComponentsInParent\<Collider >();

#### 2.8.2 组件的添加和删除

- 启用当前游戏物体上的某个组件

  GetComponent\<BoxCollider >().enabled = true

- 启用其他游戏物体上的某个组件(先定义public gameObject)

  other.GetComponent\<BoxCollider >().enabled = true

### 2.9 游戏物体的移动和旋转

#### 2.9.1 物体移动方法

- transform.Translate();

  ```c#
  transform.Translate(Vector3.right * Time.deltaTime * 2, Space.World);
  ```

- Vector3.Lerp(), 先快后慢;

  ```c#
  transform.position = Vector3.Lerp(transform.position, target, Time.deltaTime);
  ```

- Vector3.MoveToward(), 匀速;

  ```c#
  transform.position = Vector3.MoveTowards(transform.position, target, Time.deltaTime);
  ```

**控制物体移动**

```c#
float h = Input.GetAxis("Horizontal");
float v = Input.GetAxis("Vertical");
transform.Translate(Vector3.right * h * Time.deltaTime);
transform.Translate(Vector3.forward * v * Time.deltaTime);
```

#### 2.9.2 物体旋转方法

- 第一种方法：使用transform.Rotate方法

  transform.Rotate(), 参数是欧拉角

- 第二种方法：使用四元数的乘法，实现当按下R键的时候让物体在当前角度的基础上沿Y轴旋转5度

  ```c#
  Quaternion q1 =  transform.rotation;
  Quaternion q2 = Quaternion.Euler(0, 5 ,0);
  if(Input.GetKeyDown(KeyCode.R)) {
      transform.rotation =  q1 * q2;
  } 
  ```

### 2.10 Invoke方法

- 调用函数，多少秒后执行某个函数

  Invoke(string, float);

- 重复调用函数，多少秒后执行某个函数，并且每隔多少秒都会执行一次

  InvokeRepeating(string, float, float);

- 取消脚本中所有的Invoke调用

  CancelInvoke();

- 取消某个函数的Invoke调用

  CancelInvoke("函数名");

- 判断某个函数是否在等候调用

  IsInvoking(string);

### 2.11 协程

**|协程|**：协同程序，即主程序在运行的同时开启另外一段处理逻辑。调用协程方法可以不用等这个方法执行完就继续向下执行。

#### 2.11.1 协程的定义

```c#
IEnumerator Task() {
	yield return new WaitForSeconds(2f); // 等待2s后继续执行下面语句
    Debug.Log("Test");
}
```

返回值为IEnumerator，使用yield语句可以暂停协程的执行，yield return的返回值决定了什么时候恢复协程的执行。

```C#
yield return null;// 暂停，下一帧继续
yield return new WaitForSeconds(2f);// 暂停2s	
yield return StartCoroutine("Other");// 暂停当前协程，开启协程Ohter
yield new WaitForFixedUpdate();// 暂停，下一次FixedUpdate继续
```

#### 2.11.2 协程的开启和关闭

**两种开启与关闭的方法**

- 参数为协程方法名

  StartCoroutine (string methodName);

  StopCoroutine (string methodName);

- 参数为IEnumerator类型

  StartCoroutine(IEnumerator routine);

  StopCoroutine(IEnumerator routine);

- 停止当前脚本所有协程

  StopAllCoroutines();

```c#
void Start() {
    StartCoroutine ("TestCoroutine");
    StartCoroutine (TestCoroutine());
}
```

**PS：**开启协程和停止协程的方式必须保持一致