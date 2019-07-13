## 编写 shader 的注意事项

### 慎用分支和循环语句

会降低 GPU 的并行处理效率。

尽量把计算向流水线上端移动，例如把放在片元着色器中的计算放到顶点着色器中，或在 CPU 中进行预计算。

- 分支判断语句中使用的条件变量最好是常数
- 分支中包含的操作指令尽可能少
- 分支的嵌套层数尽可能少

### 不要除于 0

```c
void color()
{
	return vec4(0.0/0.0);
}
```

不同的平台表现不一致，可能崩溃，可能白色或黑色。
