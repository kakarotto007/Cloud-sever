# 矩阵的元素相乘和矩阵乘法

```python
	#逐个玄素相乘
    print(f"fensor.mul(tensor):\n{tensor.mul(tensor)}\n")
	# 等价写法
	print(f"tensor * tensor :\n{tensor*tensor}")
```

```python
# 矩阵乘法
print(f"tensor.matmul(tensor.T):\n{tensor.matmul(tensor.T)}\n")
# 等价写法
print(f"tensor @ tensor.T:\n {tensor@tensor.T}")
```

