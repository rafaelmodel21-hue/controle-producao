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

