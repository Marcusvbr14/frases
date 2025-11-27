# frases
Frases Rápidas

import pandas as pd
import json
import os

FILE_PATH = "Argumentos e frases (3).xlsx"
OUTPUT_FOLDER = "exportados"

def carregar_planilha(path):
    excel = pd.ExcelFile(path)
    dados = {}

    for sheet in excel.sheet_names:
        df = excel.parse(sheet)
        df = df.dropna(how="all")  # remove linhas totalmente vazias
        df.columns = [str(c) for c in df.columns]  # garante nomes simples
        dados[sheet] = df

    return dados

def exportar_csv(dados, pasta):
    os.makedirs(pasta, exist_ok=True)
    for nome_aba, df in dados.items():
        nome_limpo = nome_aba.strip().replace(" ", "_")
        df.to_csv(f"{pasta}/{nome_limpo}.csv", index=False)

def exportar_json(dados, pasta):
    os.makedirs(pasta, exist_ok=True)
    dados_json = {k: v.to_dict(orient="records") for k, v in dados.items()}

    with open(f"{pasta}/todas_as_abas.json", "w", encoding="utf-8") as f:
        json.dump(dados_json, f, ensure_ascii=False, indent=4)

def main():
    print("Lendo planilha...")
    dados = carregar_planilha(FILE_PATH)
    print(f"{len(dados)} abas carregadas.")

    print("Exportando CSVs...")
    exportar_csv(dados, OUTPUT_FOLDER)

    print("Gerando JSON geral...")
    exportar_json(dados, OUTPUT_FOLDER)

    print("Processo concluído!")

if __name__ == "__main__":
    main()

