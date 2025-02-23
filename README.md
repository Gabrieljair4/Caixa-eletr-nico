# Caixa-eletr-nico
Simulador de caixa eletrônico.
import time
from datetime import datetime

# Simulando um banco de dados de usuários
usuarios = {
    "12345": {
        "senha": "1234",
        "saldo": 1000.0,
        "extrato": []
    },
    "67890": {
        "senha": "5678",
        "saldo": 500.0,
        "extrato": []
    }
}

# Função para autenticação do usuário
def autenticar():
    tentativas = 3
    while tentativas > 0:
        conta = input("Digite o número da conta: ")
        senha = input("Digite a senha: ")

        if conta in usuarios and usuarios[conta]["senha"] == senha:
            print("\n✅ Login bem-sucedido!\n")
            return conta
        else:
            tentativas -= 1
            print(f"❌ Credenciais inválidas. Tentativas restantes: {tentativas}\n")

    print("🚫 Número de tentativas excedido. Encerrando...")
    exit()

# Função para exibir o saldo
def exibir_saldo(conta):
    saldo = usuarios[conta]["saldo"]
    print(f"💰 Saldo atual: R${saldo:.2f}\n")

# Função para realizar saque
def sacar(conta):
    try:
        valor = float(input("Digite o valor para saque: R$"))
        if valor <= 0:
            print("⚠️ Valor inválido. Tente novamente.\n")
        elif valor > usuarios[conta]["saldo"]:
            print("🚫 Saldo insuficiente.\n")
        else:
            usuarios[conta]["saldo"] -= valor
            usuarios[conta]["extrato"].append((datetime.now(), f"Saque: -R${valor:.2f}"))
            print(f"💸 Saque de R${valor:.2f} realizado com sucesso!\n")
            exibir_saldo(conta)
    except ValueError:
        print("⚠️ Entrada inválida. Tente novamente.\n")

# Função para realizar depósito
def depositar(conta):
    try:
        valor = float(input("Digite o valor para depósito: R$"))
        if valor <= 0:
            print("⚠️ Valor inválido. Tente novamente.\n")
        else:
            usuarios[conta]["saldo"] += valor
            usuarios[conta]["extrato"].append((datetime.now(), f"Depósito: +R${valor:.2f}"))
            print(f"💳 Depósito de R${valor:.2f} realizado com sucesso!\n")
            exibir_saldo(conta)
    except ValueError:
        print("⚠️ Entrada inválida. Tente novamente.\n")

# Função para exibir extrato
def exibir_extrato(conta):
    print("\n📄 Extrato de Transações:")
    if not usuarios[conta]["extrato"]:
        print("Nenhuma transação realizada.\n")
    else:
        for data, transacao in usuarios[conta]["extrato"]:
            print(f"{data.strftime('%d/%m/%Y %H:%M:%S')} - {transacao}")
        print("")

# Função principal do ATM
def atm():
    print("🏧 Bem-vindo ao Caixa Eletrônico Python 🏧\n")
    conta_logada = autenticar()

    while True:
        print("Escolha uma opção:")
        print("1️⃣ - Ver Saldo")
        print("2️⃣ - Saque")
        print("3️⃣ - Depósito")
        print("4️⃣ - Extrato")
        print("5️⃣ - Sair")

        opcao = input("Digite a opção desejada: ")

        if opcao == "1":
            exibir_saldo(conta_logada)
        elif opcao == "2":
            sacar(conta_logada)
        elif opcao == "3":
            depositar(conta_logada)
        elif opcao == "4":
            exibir_extrato(conta_logada)
        elif opcao == "5":
            print("\n👋 Obrigado por usar nosso ATM. Até mais!")
            time.sleep(1)
            break
        else:
            print("⚠️ Opção inválida. Tente novamente.\n")

# Executando o ATM
if __name__ == "__main__":
    atm()
