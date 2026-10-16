saldo_inicial = 0

def cadastrar_cliente():
    nome = input("Nome: ")
    idade = int(input("Idade: "))
    conta = input("Número da conta: ")

    with open("clientes.txt", "a") as arquivo:
        arquivo.write(f"{nome};{conta};{idade};{saldo_inicial}\n")

    print("Cliente cadastrado!")


def ver_clientes():
    with open("clientes.txt", "r") as arquivo:
        for cliente in arquivo:
            dados = cliente.strip().split(";")

            print("----------------")
            print(f"Nome: {dados[0]}")
            print(f"Conta: {dados[1]}")
            print(f"Idade: {dados[2]}")
            print(f"Saldo: R${dados[3]}")

def depositar():
    conta = input("Conta: ")
    valor = float(input("Valor do depósito: "))

    with open("clientes.txt", "r") as arquivo:
        clientes = arquivo.readlines()

    encontrado = False

    for i, cliente in enumerate(clientes):
        dados = cliente.strip().split(";")

        if dados[1] == conta:
            saldo = float(dados[3])
            saldo += valor

            clientes[i] = f"{dados[0]};{dados[1]};{dados[2]};{saldo}\n"

            encontrado = True

            with open("historico.txt", "a") as arquivo:
                arquivo.write(f"Depósito - {dados[0]} - R${valor}\n")

            print(f"Depósito realizado!")
            print(f"Novo saldo: R${saldo}")


    if encontrado:
        with open("clientes.txt", "w") as arquivo:
            arquivo.writelines(clientes)
    else:
        print("Conta não encontrada!")


def sacar():
    conta = input("Conta: ")
    valor = float(input("Valor do saque: "))

    with open("clientes.txt", "r") as arquivo:
        clientes = arquivo.readlines()

    encontrado = False

    for i, cliente in enumerate(clientes):
        dados = cliente.strip().split(";")

        if dados[1] == conta:
            saldo = float(dados[3])

            if valor <= saldo:
                saldo -= valor

                clientes[i] = f"{dados[0]};{dados[1]};{dados[2]};{saldo}\n"

                encontrado = True

                with open("historico.txt", "a") as arquivo:
                    arquivo.write(f"Saque - {dados[0]} - R${valor}\n")

                print("Saque realizado!")
                print(f"Novo saldo: R${saldo}")

            else:
                print("Saldo insuficiente!")

    if encontrado:
        with open("clientes.txt", "w") as arquivo:
            arquivo.writelines(clientes)



def consultar_saldo():
    conta = input("Conta: ")

    with open("clientes.txt", "r") as arquivo:
        for cliente in arquivo:
            dados = cliente.strip().split(";")

            if dados[1] == conta:
                print(f"Saldo: R${dados[3]}")
                return

    print("Conta não encontrada!")


def historico():
    with open("historico.txt", "r") as arquivo:
        for linha in arquivo:
            print(linha.strip())



while True:

    print("""
----- BANCO -----

1 - Cadastrar cliente
2 - Ver clientes
3 - Depositar
4 - Sacar
5 - Consultar saldo
6 - Histórico
7 - Sair

================
""")

    opcao = input("Escolha: ")


    if opcao == "1":
        cadastrar_cliente()

    elif opcao == "2":
        ver_clientes()

    elif opcao == "3":
        depositar()

    elif opcao == "4":
        sacar()

    elif opcao == "5":
        consultar_saldo()

    elif opcao == "6":
        historico()

    elif opcao == "7":
        print("Encerrando...")
        break

    else:
        print("Opção inválida!")# Sistema-Bancario-Python-TXT
