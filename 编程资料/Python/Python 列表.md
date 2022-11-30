在python中，用（[ ]）来表示列表

>bicycles = ['trek','cannondale','redline']

访问列表元素

>print(bicycles[0])
>trek

修改，添加和删除元素

>name = ['a','b','c']
>name[1] = d
>print(name)
>['a','d','c']

>name = ['a','b','c']
>name.append('d') # 在列表后添加
>print(name)
>['a','b',c','d']
>name.insert(0,'e') # 在索引号0插入e
>print(name)
>['e','a','b','c','d']
>del name[0] # 删除索引号0
>name.pop(0) # 删除索引号0 无参数便是末尾
>name.remove('e') # 删除值为e的元素

>name = ['a','b','c','d']
>name.sort() # sort对列表永久性排序
>name.sorted() # sorted对列表暂时性排序
>name.reverse() # reverse对列表反转
>len(name) # len对列表的长度