---
title: Python源码分享：24点计算器
date: 2024-09-15 11:36:21
tags: 代码分享
---
## Python源码分享：24点计算器

今天给大家带来24点计算器的源码。

24点介绍：24点是一种棋牌类益智游戏，要求四个数字运算结果等于二十四，一起来玩玩吧！这个游戏用扑克牌更容易来开展。拿一副牌，抽去大小王后(初练也可以把J/Q/K/大小王也拿去)，剩下1～10这40张牌(以下用1代替A)。任意抽取4张牌(称为牌组)，用加、减、乘、除(可加括号，高级玩家也可用乘方开方与阶乘运算)把牌面上的数算成24。每张牌必须用且只能用一次。如抽出的牌是3、8、8、9，那么算式为(9-8)×8×3=24。

小编给大家的源码是自己写的，可以计算24点，还支持多个数的计算和乘方运算，快来使用吧！

```python
import itertools
from tqdm import tqdm

class ProgressCounter:
    def __init__(self, total):
        self.total = total
        self.pbar = tqdm(total=total)

    def update(self, increment=1):
        self.pbar.update(increment)

def evaluate_expression(expression):
    try:
        result = eval(expression)
        return abs(result - 24) < 1e-6  # 设置结果阈值为1e-6，判断是否接近24
    except ZeroDivisionError:
        return False
    except OverflowError:
        return False

def find_combinations(numbers, include_power=False):
    operations = ['+', '-', '*', '/']
    if include_power:
        operations.append('**')
    combinations = list(itertools.permutations(numbers))
    found_combinations = []

    total_combinations = len(combinations) * pow(len(operations), 3)
    counter = ProgressCounter(total_combinations)
    for combo in combinations:
        for ops in itertools.product(operations, repeat=3):
            expression = '(({} {} {}) {} {}) {} {}'.format(combo[0], ops[0], combo[1], ops[1], combo[2], ops[2], combo[3])
            if evaluate_expression(expression):
                found_combinations.append(expression)
            counter.update(1)

    return found_combinations

# 获取用户输入的四个数字
numbers = []
for i in range(4):
    number_str = input('请输入第{}个数：'.format(i+1))
    numbers.append(float(number_str))

# 询问是否加入次方计算
include_power_str = input('是否加入次方计算？(y/n): ')
include_power = include_power_str.lower() == 'y'

# 查找结果为24的组合
results = find_combinations(numbers, include_power)
if results:
    print('找到以下结果为24的组合：')
    for result in results:
        print(result)
else:
    print('找不到结果为24的组合')
print('计算完成，程序结束。感谢您的使用。')
print('2023 星空暗夜团队')
input('请按任意键继续...')
```

如果转载的话请标注原作者，谢谢！