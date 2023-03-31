# 一、脚本生命周期

- 官方生命周期流程图

![clipboard.png](Unity%E8%84%9A%E6%9C%AC.assets/clip_image002.gif)

- 翻译版

![clipboard.png](Unity%E8%84%9A%E6%9C%AC.assets/clip_image004.gif)

## 二、API架构图和相关类

![clipboard.png](Unity%E8%84%9A%E6%9C%AC.assets/clip_image006.gif)

### （1）Component

- Component类提供

    - 查找（在当前物体、父子物体）组件的查找功能。

### （2）Transform

- Transfrom类提供
    - 查找（父、根、子）Transfrom组件的功能
    - 改变游戏对象位置、角度、缩放比例的功能。
    - transform.localScale和transform.lossyScale的区别：
        - localScale：相对于父物体的缩放比例
        - lossyScale：相对于原本的模型（自身）缩放比例（自身缩放比例*父物体缩放比例）


```c#
// 例如两个物体 fu,zi 其中zi在fu的层级之下

fu.localScale = 2, 1, 1

zi.localScale = 3, 1, 1

则zi.lossyScale = 6, 1, 1  // 2 * 3
```

### （3）GameObject

- GameObject类提供：
    - 创建游戏对象的功能
    - 物体禁用和激活状态的修改功能
    - 查找游戏对象的功能（静态方法）
    - 按游戏名查找（不建议使用，在所有场景中查找性能极低）
    - 按tag查找
    - 销毁和禁止静止销毁游戏对象的功能
- 注意：这是Unity脚本中唯一拥有构造函数的类，这意味着所有组件对象的创建都必须依附于GameObject对象的创建。并且无法单独创建组件对象（因为它没有构造函数）
```c#
Light light = new Light();  // 错误，不能直接创建灯光组件

this.gameObject.AddComponent<Light>();  // 正确，委托给gameObject对象创建灯光组件
```

```c#
// 1 创建游戏对象

GameObject lightGO = new GameObject();

// 2 为游戏对象新建并添加灯光组件

Light light = lightGO.AddComponent<Light>();  // 创建完成后会返回组件的引用

// 3 使用灯光对象为灯光组件进行设置等

light.color = Color.red;

...
```

- gameObject.activeInHierarchy和gameObject.activeSelf的区别：
    - activeInHierarchy：在场景中物体的实际激活状态
    - activeSelf：在Inspector面板中的激活状态
    - 例如两个物体 fu,zi 其中zi在fu的层级之下

- 其中父物体Inspector面板激活为false，则在场景中，父子物体都无法显示
    - 即父子物体activeInHierarchy = false
    - 但是子物体在Inspector面板激活将仍然为true
    - 导致它没在场景中显示的原因是父物体的Inspector面板激活为false，而不是自身

### （4）Time

- Time类管理游戏的各种时间，里面只有静态属性，调用该属性即可知道游戏中的时间变量。


#### 1、Time.time

- 游戏启动到现在的时间。


#### 2、Time.deltaTime

- 游戏后一帧执行的时间到前一帧执行的时间间隔。


- 一般在update钩子中执行物理行为，需要乘上Time.deltaTime。

    - 因为update钩子方法执行的次数只与渲染的速度有关，即在不同的机器性能上调用次数不同，所以执行出来的效果不同。如果乘上Time.deltaTime时，则在性能低的机器上值就大，在性能高的机器上值就小（帧的间隔随渲染时间而变化）。因此就可以综合update钩子函数执行的时间差。以达到物理效果一致的结果。


#### 3、Time.fixedDeltaTime

- 与deltaTime类似，fixedDeltaTime是固定的前一帧执行到后一帧执行的时间间隔。在FixedUpdate钩子中执行物理行为乘上fixedDeltaTime也可以达到匀速的效果，使代码更加合理。也可以不乘，因为fixedUpdate本身就是固定的。


#### 4、Time.timeScale

- 控制游戏时间执行的速度。1为正常，0即为游戏停止。


- 注意：TimeScale的控制在Update钩子和FixedUpdate钩子的执行是不同的。
    - Update的执行只与渲染有关，所以不受TimeScale的影响
    - FixedUpdate钩子的执行是固定间隔的，与渲染无关，所以会受到TimeScale影响

## 三、三维数学

### 1、向量（Vector2/3）

#### （1）向量

- 在Unity中，所有物体的Transform都是一个三维向量，向量的头为世界的中心坐标即（0，0，0），向量的尾为当前物体所在的坐标。
```c#
// 可以使用Debug类中画线调试

Debug.DrawLine(Vector3 <头坐标>, Vector3 <尾坐标>, Color.<颜色>);
```
#### （2）模长（长度）

- 向量头到向量尾的长度
```c#
// 1 使用三维向量对象属性获取

Vector3.magnitude;

// 2 使用三维向量类的两点间距方法获取

Vector3.distance(Vector3 <头坐标>, Vector3 <尾坐标>);
```
#### （3）方向

- 获取向量方向也称标准化向量或归一化向量。即获取该向量的单位向量（大小为1的向量）。


- 几何意义：将该向量拉长或缩短，使得模长等于1。
```c#
Vector3对象.normalized;
```
#### （4）向量运算

- 减法运算：
    - 方向：指向被减向量
    - 大小：两点的距离
    - 注意：两个向量相减，等值的向量一定是从共同的起点开始的。

![clipboard.png](Unity%E8%84%9A%E6%9C%AC.assets/clip_image008.gif)

- 加法运算：

![clipboard.png](Unity%E8%84%9A%E6%9C%AC.assets/clip_image010.gif)

- 向量点乘：
    - a * b = |a| * |b| * cos<a,b>

- 向量叉乘：
- 几何意义：结果为两个向量所组成面的垂直向量，模长为两向量模长乘积再乘夹角的正弦值。
```c#
Vector vector = Vector3.Cross(a, b);
```
**（5）向量与标量的乘除**

- 乘法：该向量的各分量与标量相乘
- 除法：该向量的各分量与标量相除
- 几何意义：缩放向量长度（不会改变方向）。

### 2、三角函数

#### （1）角度和弧度

- 角度和弧度的转换


弧度 = 角度 * Mathf.Deg2Rad

角度 = 弧度 * Mathf.Rad2Deg

### 3、欧拉角

- 使用三个角度来保存方位。其中X与Z沿自身坐标系旋转，Y沿世界坐标系旋转。


- 就是Unity编辑器中物体旋转所输入的数值。其中在脚本中操作，Unity将限制xyz轴的数值。

```c#
// 类型为向量，实际上是欧拉角 
Vector3 euler = transform.eulerAngles;
```

