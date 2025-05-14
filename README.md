# 🐍Explicação do Código 'Vadidator.py'

O código `Vadidator.py` é um programa em Java que valida a bandeira de um cartão de crédito com base no número fornecido, além de gerar uma data de validade e um código de segurança (CVV) fictícios. Abaixo está a explicação detalhada de cada parte do código.

---

## 1. Validação da Bandeira do Cartão
O método `validarBandeira` verifica a bandeira do cartão de crédito com base no número fornecido. Ele utiliza expressões regulares (regex) para identificar padrões específicos de cada bandeira.

### Código:

```python
import re
import random
from datetime import datetime, timedelta

def validar_bandeira(numero_cartao):
    bandeiras = [
        ("Visa", r"^4[0-9]{12}(?:[0-9]{3})?$"),
        ("MasterCard", r"^(5[1-5][0-9]{14}|2(2[2-9][0-9]{12}|[3-6][0-9]{13}|7[01][0-9]{12}|720[0-9]{12}))$"),
        ("Elo", r"^(4011|4312|4389|45|50|627780|636297|636368)[0-9]*$"),
        ("American Express", r"^3[47][0-9]{13}$"),
        ("Discover", r"^(6011|65|64[4-9])[0-9]*$"),
        ("Hipercard", r"^6062[0-9]*$"),
        ("Diners Club", r"^3(?:0[0-5]|[68][0-9])[0-9]{11}$"),
        ("EnRoute", r"^2(?:014|149)[0-9]{11}$"),
        ("JCB", r"^(?:2131|1800|35[0-9]{3})[0-9]{11}$"),
        ("Voyager", r"^8699[0-9]{11}$"),
        ("Aura", r"^50[0-9]{14,17}$")
    ]

    for nome, padrao in bandeiras:
        if re.match(padrao, numero_cartao):
            return nome
    return "Bandeira não suportada ou número inválido."
```

###Explicação:
- **Saída**: O nome da bandeira correspondente ou uma mensagem indicando que a bandeira não é suportada.
- **Funcionamento**:
  - O método percorre uma lista de bandeiras e seus padrões regex.
  - Se o número do cartão corresponder a um padrão, a bandeira correspondente é retornada.

---

## 2. Gereação de Dados para a Validade
### Código:
```python
def gerar_data_validade():
    anos_no_futuro = random.randint(1, 5)
    data_futura = datetime.now().replace(day=1) + timedelta(days=365 * anos_no_futuro)
    return data_futura.strftime("%m/%y")
```

### Explicação:
- **Entrada**: Nenhuma.
- **Saída**: Uma data de validade no formato `MM/yy`.
- **Funcionamento**:
  - Escolhe um número aleatório de anos (1 a 5).
  - Soma esse tempo à data atual.
  - Formata o resultado como MM/yy `MM/yy`.

---

## 3. Gereação do CVV
A função gerar_cvv cria um código de segurança fictício(CVV), variando conforme a bandeira.

### Código:
```python
def gerar_cvv(bandeira):
    if bandeira == "American Express":
        return f"{random.randint(0, 9999):04d}"  # CVV com 4 dígitos
    else:
        return f"{random.randint(0, 999):03d}"   # CVV com 3 dígitos
```
- **Entrada**: O nome da bandeira do cartão.
- **Saída**: CVV como string ("123" ou "0123").
- **Funcionamento**:
  - Se a bandeira for `"American Express"`, o CVV terá 4 dígitos.
  - Para outras bandeiras, o CVV terá 3 dígitos.
  - O método Usa random.randint para gerar o número.

---

## 4. Função Principal (main)
Esta função reúne todas as partes anteriores e exibe os resultados.

### Código:
```python
def main():
    numero_cartao = "370623013947503"  # Substitua por outro número para testar
    bandeira = validar_bandeira(numero_cartao)
    print(f"Bandeira do cartão: {bandeira}")

    if bandeira != "Bandeira não suportada ou número inválido.":
        validade = gerar_data_validade()
        cvv = gerar_cvv(bandeira)
        print(f"Data de validade: {validade}")
        print(f"CVV: {cvv}")

if __name__ == "__main__":
    main()
```
- **Entrada**: Define um número de cartão para teste.
- **Saída**: Valida a bandeira com validar_bandeira..
- **Funcionamento**:
  - Gera uma data de validade com gerar_data_validade..
  - Gera um CVV com gerar_cvv.
---


### Resumo:
Este programa:
1. Detecta a bandeira do cartão com regex.
2. Gera uma data de validade aleatória no futuro.
3. Gera um CVV fictício baseado na bandeira.
4. Exibe todos os resultados.

---

## Sugestões de Melhoria
1. **Validação do número do cartão**: Adicionar uma verificação mais robusta para validar o número do cartão (ex.: algoritmo de Luhn).
2. **Mensagens de erro**: Informar ao usuário de forma mais clara se o número do cartão for inválido.
3. **Separação de responsabilidades**: Dividir o código em métodos menores para melhorar a legibilidade e manutenção.

---
# 🔒 Validação com o Algoritmo de Luhn

O algoritmo de Luhn é usado para validar se um número de cartão de crédito é matematicamente válido. Ele não garante que o cartão exista, mas verifica se a sequência é plausível.
---

## 5. Validação com o Algoritmo de Luhn

### Código:

```python
def validar_luhn(numero):
    total = 0
    reverso = numero[::-1]

    for i, digito in enumerate(reverso):
        n = int(digito)
        if i % 2 == 1:
            n *= 2
            if n > 9:
                n -= 9
        total += n

    return total % 10 == 0"
```

### Explicação:
- **Entrada**: String com o número do cartão.
- **Saída**: True se for válido, False se for inválido.
- **Funcionamento**:
  - Inverte o número para aplicar as regras na ordem certa.
  - Dobra os dígitos nas posições pares (começando do fim).
  - Se o resultado da multiplicação for maior que 9, subtrai 9.
  - Soma todos os valores.
  - Se o total for divisível por 10, o número é válido.

---

## 2. Gereação de Dados para a Validade
### Código:
```python
def gerar_data_validade():
    anos_no_futuro = random.randint(1, 5)
    data_futura = datetime.now().replace(day=1) + timedelta(days=365 * anos_no_futuro)
    return data_futura.strftime("%m/%y")
```

# 🧠 Atualização da função principal com Luhn

### Código:
```python
import re
import random
from datetime import datetime, timedelta

def validar_bandeira(numero_cartao):
    bandeiras = [
        ("Visa", r"^4[0-9]{12}(?:[0-9]{3})?$"),
        ("MasterCard", r"^(5[1-5][0-9]{14}|2(2[2-9][0-9]{12}|[3-6][0-9]{13}|7[01][0-9]{12}|720[0-9]{12}))$"),
        ("Elo", r"^(4011|4312|4389|45|50|627780|636297|636368)[0-9]*$"),
        ("American Express", r"^3[47][0-9]{13}$"),
        ("Discover", r"^(6011|65|64[4-9])[0-9]*$"),
        ("Hipercard", r"^6062[0-9]*$"),
        ("Diners Club", r"^3(?:0[0-5]|[68][0-9])[0-9]{11}$"),
        ("EnRoute", r"^2(?:014|149)[0-9]{11}$"),
        ("JCB", r"^(?:2131|1800|35[0-9]{3})[0-9]{11}$"),
        ("Voyager", r"^8699[0-9]{11}$"),
        ("Aura", r"^50[0-9]{14,17}$")
    ]

    for nome, padrao in bandeiras:
        if re.match(padrao, numero_cartao):
            return nome
    return "Bandeira não suportada ou número inválido."

def validar_luhn(numero):
    total = 0
    reverso = numero[::-1]
    for i, digito in enumerate(reverso):
        n = int(digito)
        if i % 2 == 1:
            n *= 2
            if n > 9:
                n -= 9
        total += n
    return total % 10 == 0

def gerar_data_validade():
    anos_no_futuro = random.randint(1, 5)
    data_futura = datetime.now().replace(day=1) + timedelta(days=365 * anos_no_futuro)
    return data_futura.strftime("%m/%y")

def gerar_cvv(bandeira):
    if bandeira == "American Express":
        return f"{random.randint(0, 9999):04d}"
    else:
        return f"{random.randint(0, 999):03d}"

def main():
    numero_cartao = "370623013947503"  # Exemplo válido American Express

    if not validar_luhn(numero_cartao):
        print("Número de cartão inválido (falha no algoritmo de Luhn).")
        return

    bandeira = validar_bandeira(numero_cartao)
    print(f"Bandeira do cartão: {bandeira}")

    if bandeira != "Bandeira não suportada ou número inválido.":
        validade = gerar_data_validade()
        cvv = gerar_cvv(bandeira)
        print(f"Data de validade: {validade}")
        print(f"CVV: {cvv}")

if __name__ == "__main__":
    main()

```
### ✔ Agora o Programa:

1. Valida a bandeira do cartão com regex.
2. Valida a estrutura do número com o algoritmo de Luhn.
3. Gera data de validade entre 1 e 5 anos no futuro.
4. Gera CVV de 3 ou 4 dígitos, conforme a bandeira.
5. Exibe tudo no console.

---
