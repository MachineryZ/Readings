- 灵活使用迭代器

~~~python
length = len(alist)
i = 0
while i < length:
	do_sth_with(alist[i])
	i += 1
	
# Pythonic:
for i in alist:
  do_Sth_with(alist[i])
~~~

- 安全关闭文件描述符使用 with 语句

~~~python
with open(path, 'r') as f:
	do_sth_with(f)
~~~

