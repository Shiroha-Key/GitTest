# 编译当前cpp文件,产生一个.exe可执行文件

```markdown
cl /EHsc <cpp文件> 
```

# 在使用While循环读取多组数据时，要注意输入的值的类型

```markdown
int val = 0;
while(cin >> val){...}
```

若此时出现这一组数据：

​	1 3 5 6.7 8 10

**则读入过程会在6.7时停止，因为cin检测到一个浮点数赋值给整数类型的val，判定为假，被while检测到，于是停止循环**

==正确的解决方法应为把val换成double类型或float类型==







