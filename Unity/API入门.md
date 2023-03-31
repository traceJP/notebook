## 一、c#-unity特性

### 1、序列化字段

- 在Unity编辑器中显示私有变量

```c#
[SerializeField]

private int a;
```

### 2、隐藏公共变量

- 在编辑器中隐藏共有变量

```c#
[HideInInspector]

public int a;
```

### 3、设定变量在编辑器中设置的范围

```c#
[Range(0, 100)]

public int a;
```



 



