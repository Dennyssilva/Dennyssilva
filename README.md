from cryptography.fernet import Fernet

# Função para gerar uma chave e salvar em um arquivo
def gerar_chave():
    chave = Fernet.generate_key()
    with open("chave.key", "wb") as chave_file:
        chave_file.write(chave)

# Função para carregar a chave
def carregar_chave():
    return open("chave.key", "rb").read()

# Função para criptografar dados
def criptografar_dados(dados, chave):
    f = Fernet(chave)
    dados_criptografados = f.encrypt(dados.encode())
    return dados_criptografados

# Função para cadastrar votos
def cadastrar_votos():
    votos = {}
    while True:
        candidato = input("Digite o nome do candidato (ou 'fim' para terminar): ")
        if candidato.lower() == 'fim':
            break
        if candidato in votos:
            votos[candidato] += 1
        else:
            votos[candidato] = 1
    return votos

# Função principal
def main():
    gerar_chave()
    chave = carregar_chave()
    votos = cadastrar_votos()
    votos_str = str(votos)
    votos_criptografados = criptografar_dados(votos_str, chave)
    
    with open("votos_criptografados.txt", "wb") as arquivo:
        arquivo.write(votos_criptografados)

    print("Votos cadastrados e arquivo criptografado criado com sucesso.")

if __name__ == "__main__":
    main()

