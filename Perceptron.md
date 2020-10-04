# Perceptron

### Single-Layer Perceptron

input layer - output layer

ex. AND 게이트

```shell
def AND_gate(x1, x2):
    w1=0.5
    w2=0.5
    b=-0.7
    result = x1*w1 + x2*w2 + b
    if result <= 0:
        return 0
    else:
        return 1
```

ex. OR 게이트

```shell
def OR_gate(x1, x2):
    w1=0.6
    w2=0.6
    b=-0.5
    result = x1*w1 + x2*w2 + b
    if result <= 0:
        return 0
    else:
        return 1
```

ex. NAND 게이트

```shell
def NAND_gate(x1, x2):
    w1=-0.5
    w2=-0.5
    b=0.7
    result = x1*w1 + x2*w2 + b
    if result <= 0:
        return 0
    else:
        return 1
```





### MultiLayer Perceptron

Input layer - Hidden layer - output layer

ex. XOR 게이트

```shell
def XOR_gate(x1, x2):
	result = NAND_gate(x1, x2)
	if result == 1:
		result2 = OR_gate(x1, x2)
		if result2 <= 0:
			return 0
		else:
			return 1
	else:
		return 0
```

hidden layer가 2개 이상인 신경망을 Deep Neural Network라고 함