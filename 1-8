from fractions import Fraction
import random
from optparse import OptionParser
import profile

class acc_Expression():
    '''算术表达式的生成'''
    def __init__(self, max_num):
        self.init_operators()
        self.init_nums(max_num)
        self.init_expression()

    def init_num(self, max_num):
        '''随机生成数'''
        denominator = random.randint(1, max_num)
        numerator = random.randint(0, max_num)
        return Fraction(numerator, denominator)

    def insert_bracket(self):
        '''插入括号'''

        if len(self.operators) > 1:
            x = random.randint(0, len(self.operators))
            while x < len(self.operators):
                y = random.randint(x, len(self.operators))
                low = False
                for add in self.operators[x:y+1]:
                     if add in ['+', '-']:
                         low = True
                         break
                try:
                    if self.operators[y+1] in ['×', '÷'] and low:
                        self.operators.insert(x, '(')
                        self.operators.insert(y+2,')')
                except IndexError:
                    pass
                x = y+2
            

    def init_operators(self):
        '''随机生成一个运算符并随机插入括号'''
        self.operators = []
        operator = ['+', '-', '×', '÷', 'null']
        for x in range(3):
            if x == 1:
                self.operators.append(random.choice(operator[:-2]))
            else:
                y = random.choice(operator)
                if y != 'null':
                    self.operators.append(y)
        self.insert_bracket()
    
    def init_nums(self, max_num):
        self.nums = []
        self.nums.append(self.init_num(max_num))
        for x in range(len(self.operators)):
            y = self.init_num(max_num)
            if self.operators[x] == '÷':
                while y.numerator == 0:
                    y = self.init_num(max_num)
            self.nums.append(y)
    
    def str_num(self, num):
        '''字符串化一个分数'''
        inter = int(num.numerator / num.denominator)
        numerator = int(num.numerator % num.denominator)
        str_num = ''
        if numerator != 0:
            str_num += str(numerator) + '/' + str(num.denominator)
        if not str_num:
            '''如果为空'''
            str_num += str(inter)
        else:
            if inter == 0:
                return str_num
            else:
                str_num = str(inter) + '`' + str_num
        return str_num
    
    def init_expression(self):
        '''生成一个算术表达式的字符串形式'''
        self.str = ''
        i = 0
        self.exp = []
        again = False
        for x in self.operators:
            if again:
                self.str += x + ' '
            elif x == '(':
                self.str += x + ' '
            elif x == ')':
                self.str += self.str_num(self.nums[i]) + ' '
                i += 1
                self.str += x + ' '
                again = True
            else:
                self.str += self.str_num(self.nums[i]) + ' '
                self.str += x + ' '
                i += 1
        self.str += self.str_num(self.nums[-1]) + ' ='



def getResult(expression):
    stackValue = []
    for item in expression:
        if item in ["+", "-", "×", "÷"]:
            n2 = stackValue.pop()
            n1 = stackValue.pop()
            result = cal(n1, n2, item)
            if result < 0:
                return -1
            stackValue.append(result)
        else:
            stackValue.append(item)
    return stackValue[0]

def cal(n1, n2, op):
    if op == "+":
        return n1 + n2
    if op == "-":
        return n1 - n2
    if op == "×":
        return n1 * n2
    if op == "÷":
        return n1 / n2

class calculate:
    def __init__(self):
        self.list_operators = ["+", "-", "×", "÷", "(", ")", "="]
        self.pri_operators = {"+": 0, "-": 0, "×": 1, "÷": 1}

    def to_suffix_expression(self, expression):
        '''生成逆波兰表达式'''
        stack_operator = myStack()
        suffix_expression = []
        list_temp = []
        expression = expression + "="
        for element in expression:
            if element not in self.list_operators:
                list_temp.append(element)
            elif element == "=":
                if len(list_temp) != 0:
                    str_temp = ""
                    for i in range(0, len(list_temp)):
                        str_temp = str_temp+list_temp.pop(0)
                    suffix_expression.append(str_temp)
            else:
                if len(list_temp) != 0:
                    str_temp = ""
                    for i in range(0, len(list_temp)):
                        str_temp = str_temp+list_temp.pop(0)
                    suffix_expression.append(str_temp)
                if stack_operator.isEmpty() or element == "(":
                    stack_operator.push(element)
                elif element != "(" and element != ")":
                    if stack_operator.peek() != "(" and self.pri_operators[element] > self.pri_operators[
                        stack_operator.peek()]:
                        stack_operator.push(element)
                    else:
                        while True:
                            if stack_operator.peek() == "(":
                                stack_operator.push(element)
                                break
                            elif self.pri_operators[element] < self.pri_operators[stack_operator.peek()]:
                                while not stack_operator.isEmpty() and True:
                                    str_operator = stack_operator.peek()
                                    if str_operator == "(" or self.pri_operators[str_operator] < self.pri_operators[
                                        element]:
                                        break
                                    else:
                                        stack_operator.pop()
                                        suffix_expression.append(str_operator)
                            else:
                                suffix_expression.append(stack_operator.pop())
                            if stack_operator.isEmpty():
                                stack_operator.push(element)
                                break
                elif element == ")":
                    while True:
                        if stack_operator.peek() != "(":
                            suffix_expression.append(stack_operator.pop())
                        else:
                            stack_operator.pop()
                            break
                else:
                    stack_operator.push(element)
        if not stack_operator.isEmpty():
            while not stack_operator.isEmpty():
                suffix_expression.append(stack_operator.pop())
        return suffix_expression

    def str_to_fraction(self, suf):
        '''字符串转换为fraction类'''
        for x in range(len(suf)):
            if suf[x] not in self.list_operators:
                y = suf[x].strip()
                if y.find('`') == -1:
                    if y.find('/') == -1:
                        numerator =  int(y)
                        denominator = 1
                    else:
                        a = y.split('/')
                        numerator = int(a[0])
                        denominator = int(a[1])
                else:
                    a = y.split('`')
                    inter = int(a[0])
                    b = a[1].split('/')
                    denominator = int(b[1])
                    numerator = int(b[0]) + inter * denominator
                new_num = Fraction(numerator,denominator)
                suf[x] = new_num
        return suf

class myStack:
    """模拟栈"""

    def __init__(self):
        self.items = []

    def isEmpty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        if not self.isEmpty():
            return self.items[len(self.items) - 1]

    def size(self):
        return len(self.items)

def main():
  
    
    parser = OptionParser()
    parser.add_option("-n", action="store", type="int", dest="problem",default="10", help='题目个数')
    parser.add_option("-r", action="store", type='int', dest="max_num", default="10", help="题目操作数取值范围",)
    i = 1
    (options, args) = parser.parse_args()
    correct = []
    wrong = [] 
    n = open('Exercises.txt', 'w')
    m = open('Answer.txt', 'w')
    while i < options.problem + 1:
        ari = acc_Expression(options.max_num)
        inf = calculate()
        try:
            real_res =getResult(inf.str_to_fraction(inf.to_suffix_expression(ari.str)))
        except ValueError:
            continue
        if real_res >= 0:
            real_res = ari.str_num(real_res)
            print(str(i)+'. ' + ari.str, end = '')
            n.write(str(i)+'. ' + ari.str + '\n')
            m.write(str(i)+'. ' + real_res + '\n')
            res = input()
            i += 1
    n.close()
    m.close()
    print('题目正确率：' + str(100 * len(correct)/options.problem)+'%')


            

if __name__ == '__main__':
    main()
