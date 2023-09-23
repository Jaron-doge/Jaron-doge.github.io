---
title: 第3章 Unity物理引擎
date: 2021-4-18 24:00:00
tags: 
categories: Unity
---
## Unity物理引擎

### 3.1 刚体

<!-- more -->

**|刚体|**：添加了刚体组件的游戏物体，就有了重力，会做自由落体运动可以像现实中的物体一样运动。

#### 3.1.1 刚体组件属性

- Mass: 设置物理的质量，单位是KG。
- Drag: 空气阻力，0表示无阻力，值很大时物体会停止运动。
- Angular Drag: 受到扭曲力时的空气阻力。
- Use Gravity： 是否使用重力。

#### 3.1.2 刚体移动方法

- Rigidbody.MovePosition(Vector3);

  **参数Vector3表示要移动到的新位置**，使用“当前位置“ + 移动增量的方式。

#### 3.1.3 特点

- 会与场景中的物体发生碰撞
- 会受重力影响，在到达场景边缘外时，会自由下落。

### 3.2 碰撞体

**|碰撞体|**：使用刚体移动的物体，与场景中的其他物体相碰撞，其实碰撞的是目标物体的“碰撞体”组件，也就是 Collider。

#### 3.2.1 应用场景

![1.png](https://i.loli.net/2021/04/19/4UhF25tgOfQiXkm.png)

### 3.3 碰撞事件检测与处理

#### 3.3.1 碰撞事件检测方法

**与start、update方法同级！！！**

- OnCollisionEnter(Collision);
- OnCollisionExit(Collision);
- OnCollisionStay(Collision);

Collision参数：碰撞类，用于传递碰撞信息。

#### 3.3.2 碰撞检测条件

- 两个物体接触并发生碰撞
- 运动的物体必须带有碰撞体Collider和刚体Rigidbody
- 另一个物体(可以静止也可以运动)必须至少带有碰撞体Collider

### 3.4 触发事件检测与处理

**|触发器|**： 将碰撞体组件属性面板上的“ Is Trigger” 选项选中，当前的游戏物体的碰撞体就变成了触发器。不与目标物体发生直接的碰撞（接触），而是只要进入目标物体的“触发范围”就能执行某些特定操作。

#### 3.4.1 触发事件检测方法

**与start、update方法同级**

- OnTriggerEnter( Collider);
- OnTriggerExit( Collider);
- OnTriggerStay( Collider);

Collider参数：碰撞体类，用于传递触发信息。

#### 3.4.2 触发检测条件

- 运动的物体必须带有碰撞体Collider和刚体Rigidbody
- 另一个物体(可以静止也可以运动)必须至少带有碰撞体Collider
- 其中一个物体上勾选了 Is Trigger 选项

### 3.5 物理射线检测

#### 3.5.1 物理射线检测方法

```c#
private Ray ray;
ray = Camera.main.ScreenPointToRay(Input.mousePosition);
```

- Camera.main ：代表 tag 设置为"MainCamera"的摄像机的 Camera组件的引用。
- ScreenPointToRay (Vector3 )：摄像机组件对象下的一个方法，将屏幕点转化为射线 ，返回一个Ray 类型的射线。
- Input.mousePosition：鼠标所在的位置，Vector3类型。

#### 3.5.2 检测射线与其他物体的碰撞

```C#
private RaycastHit hit;
if(Physics.Raycast(ray, out hit)) {
    // 将碰撞到的物体销毁
    GameObject.Destroy(hit.collider.gameObject);
}
```

- RaycastHit ：一个结构体，用于存储射线的碰撞信息。

- Physics.Raycast (Ray, out RaycastHit)：射线检测，第一个参数为要检测的射线，如果射线与其他物体相撞了，相撞的信息存储在第二参数里。

#### 3.5.3 物理射线使用步骤

第一步：创建一根射线。

第二步：检查这根射线与其他物体的碰撞，得到碰撞信息。

第三步：通过碰撞信息对碰撞到的物体进行处理。