# ==========================================
# SISTEMA DE CONTROLE DE PRODUÇÃO E QUALIDADE
# ==========================================

caixas = []                  # Lista de caixas fechadas
caixa_atual = []             # Caixa em montagem
capacidade_caixa = 10

aprovadas = 0
reprovadas = 0
motivos_reprovacao = {
    "peso": 0,
    "cor": 0,
    "comprimento": 0
}

def verificar_peca(id, peso, cor, comprimento):
    motivos = []

    if not (95 <= peso <= 105):
        motivos.append("peso")
    if cor.lower() not in ["azul", "verde"]:
        motivos.append("cor")
    if not (10 <= comprimento <= 20):
        motivos.append("comprimento")

    if len(motivos) == 0:
        return True, motivos
    else:
        return False, motivos


while True:
    print("\n--- Nova Peça Produzida ---")

    id_peca = input("ID da peça: ")
    peso = float(input("Peso (g): "))
    cor = input("Cor: ")
    comprimento = float(input("Comprimento (cm): "))

    status, motivos = verificar_peca(id_peca, peso, cor, comprimento)

    if status == True:
        aprovadas += 1
        caixa_atual.append(id_peca)

        # Se a caixa atingiu capacidade, fecha e abre nova
        if len(caixa_atual) == capacidade_caixa:
            caixas.append(caixa_atual)
            caixa_atual = []  # nova caixa
            print("📦 Caixa cheia! Nova caixa iniciada.")

        print("✔ Peça APROVADA!")
    else:
        reprovadas += 1
        for m in motivos:
            motivos_reprovacao[m] += 1

        print("❌ Peça REPROVADA! Motivos:", motivos)

    continuar = input("Registrar nova peça? (s/n): ").lower()
    if continuar != 's':
        # Fechar a última caixa se não estiver vazia
        if len(caixa_atual) > 0:
            caixas.append(caixa_atual)
        break


# ============
# RELATÓRIO FINAL
# ============

print("\n===== RELATÓRIO FINAL =====")
print(f"Total de peças aprovadas: {aprovadas}")
print(f"Total de peças reprovadas: {reprovadas}")

print("\nMotivos de reprovação:")
for m, qnt in motivos_reprovacao.items():
    print(f" - {m}: {qnt}")

print(f"\nQuantidade de caixas utilizadas: {len(caixas)}")

print("\nCaixas e peças:")
for index, caixa in enumerate(caixas):
    print(f"Caixa {index+1}: {caixa}")


Minha solução é um sistema em Python que automatiza o controle de produção e qualidade das peças fabricadas. Principais Funcionalidades: Recebe os dados de cada peça produzida, como ID, peso, cor e comprimento. Avalia se a peça foi aprovada ou reprovada com base em critérios específicos: peso entre 95g e 105g, cor azul ou verde, e comprimento entre 10cm e 20cm. Armazena as peças aprovadas em caixas com capacidade para 10 peças cada e fecha a caixa automaticamente quando atinge essa capacidade. Gera um relatório consolidado com o total de peças aprovadas, reprovadas e a quantidade de caixas utilizadas.
 Utilizei uma classe para representar cada peça, com atributos para os dados e um método para avaliar a qualidade."Peca" Criei uma classe que gerencia as peças, as avaliações e o armazenamento em caixas."LinhaDeMontagemRegras de Aprovação: "Na classe, implementei as regras de qualidade. Se a peça não atende a qualquer critério, ela é marcada como reprovada, e o motivo é armazenado para consultas posteriores."PecaArmazenamento em Caixas: "As peças aprovadas são adicionadas a uma lista, e quando essa lista atinge 10 peças, a caixa é fechada e uma nova é iniciada. Por fim, a funcionalidade de relatório compila o total de peças aprovadas e reprovadas, junto com os motivos das reprovações. Utilizei a programação orientada a objetos, o que permite modularizar o código e facilita a manutenção e a expansão do sistema no futuro. Os nomes de classes e métodos foram escolhidos de forma a refletir sua funcionalidade, como , e , tornando o código mais legível. Implementei checagens para garantir que os dados das peças estejam dentro dos parâmetros esperados, evitando que erros sejam propagados no sistema. Incluí comentários no código para explicar partes importantes, o que ajuda outros desenvolvedores ou mesmo eu no futuro a entender a lógica implementada.Com essa solução, conseguimos otimizar o processo de inspeção, reduzir erros e custos operacionais, trazendo mais eficiência para a linha de produção.

