import re

# 연산자 우선순위 정의
precedence = {'+': 1, '-': 1, '*': 2, '/': 2}

# 중위표기를 후위표기로 변환
def infix_to_postfix(expression):
    output = []
    stack = []
    tokens = re.findall(r'\d+|[()+\-*/]', expression)

    for token in tokens:
        if token.isdigit():
            output.append(token)
        elif token in precedence:
            while stack and stack[-1] != '(' and precedence.get(stack[-1], 0) >= precedence[token]:
                output.append(stack.pop())
            stack.append(token)
        elif token == '(':
            stack.append(token)
        elif token == ')':
            while stack and stack[-1] != '(':
                output.append(stack.pop())
            stack.pop()  # '(' 제거
    while stack:
        output.append(stack.pop())
    return output

# 후위표기식을 계산
def evaluate_postfix(postfix):
    stack = []
    for token in postfix:
        if token.isdigit():
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            if token == '+': stack.append(a + b)
            elif token == '-': stack.append(a - b)
            elif token == '*': stack.append(a * b)
            elif token == '/': stack.append(a / b)
    return stack[0]

# 수식 목록
expressions = [
    "3 + 5 * 2",
    "8 / 2 + 4",
    "(7 - 4) * 3",
    "6 + 2 / 2",
    "9 - 5 + 3",
    "4 * (2 + 3)",
    "1 + 2 * 3 - 1",
    "8 / (2 + 2)",
    "7 - 3 + 2 * 2",
    "9 / 3 + 1 * 4"
]

# 결과 출력
for expr in expressions:
    postfix = infix_to_postfix(expr)
    result = evaluate_postfix(postfix)
    print(f"중위표기: {expr}")
    print(f"후위표기: {' '.join(postfix)}")
    print(f"연산결과: {result}\n")
