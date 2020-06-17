# switch-case相比if-else

 &emsp;&emsp;switch...case与if...else的根本区别在于，switch...case会生成一个跳转表来指示实际的case分支的地址，而这个跳转表的索引号与switch变量的值是相等的。从而，switch...case不用像if...else那样遍历条件分支直到命中条件，而只需访问对应索引号的表项从而到达定位分支的目的。

&emsp;&emsp;具体地说，switch...case会生成一份大小（表项数）为最大case常量＋1的跳表，程序首先判断switch变量是否大于最大case 常量，若大于，则跳到default分支处理；否则取得索引号为switch变量大小的跳表项的地址（即跳表的起始地址＋表项大小＊索引号），程序接着跳到此地址执行，到此完成了分支的跳转。


&emsp;&emsp;由此看来，switch有点以空间换时间的意思，而事实上也的确如此。

1. 当分支较多时，时用switch的效率是很高的。因为switch是随机访问的，就是确定了选择值之后直接跳转到那个特定的分支，但是if。。else是遍历所有可能值，直到找到符合条件的分支。如此看来，switch的效率确实比if-else要高的多。
2. 但是switch...case要占用较多的代码空间，因为它要生成跳表，特别是当case常量分布范围很大但实际有效值又比较少的情况，switch...case的空间利用率将变得很低。
3. switch...case只能处理case为常量的情况，对非常量的情况是无能为力的。例如 if (a > 1 && a < 100)，是无法使用switch...case来处理的。所以，switch只能是在常量选择分支时比ifelse效率高，但是ifelse能应用于更多的场合，ifelse比较灵活。