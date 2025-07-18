import re

# 연산자 우선순위
precedence = {'+': 1, '-': 1, '*': 2, '/': 2}

# 중위표기 → 후위표기 변환
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

# 후위표기 계산
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

# 수식 리스트
expressions = [
    "5 + 3",
    "7 * 8",
    "10 - 4",
    "20 / 4",
    "10 + 3 * 2",
    "20 - 15 / 5",
    "5 * 2 + 10 / 2",
    "(5 + 3) * 2",
    "100 / (10 + 10)",
    "3 * (4 + (5 - 1))",
    "(15 + 5) * (10 - 4) / 2",
    "10 - 4 - 3",
    "100 / 10 / 2",
    "12 + 30 * 2"
]

# 결과 출력
for idx, expr in enumerate(expressions, 1):
    postfix = infix_to_postfix(expr)
    result = evaluate_postfix(postfix)
    print(f"[Case {idx}] 중위표기: {expr}")
    print(f"         후위표기: {' '.join(postfix)}")
    print(f"         연산결과: {result}\n")
