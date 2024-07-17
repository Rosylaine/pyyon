import tkinter as tk
from tkinter import messagebox
import sqlite3

# Conectar ao banco de dados SQLite
conexao = sqlite3.connect('banco_alimentos.db')
cursor = conexao.cursor()

# Criar tabelas
cursor.execute('''CREATE TABLE IF NOT EXISTS voluntarios (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    nome TEXT NOT NULL,
                    idade INTEGER NOT NULL,
                    area_atuacao TEXT NOT NULL,
                    disponibilidade TEXT NOT NULL)''')

cursor.execute('''CREATE TABLE IF NOT EXISTS alimentos (
                    id INTEGER PRIMARY KEY AUTOINCREMENT,
                    tipo TEXT NOT NULL,
                    quantidade INTEGER NOT NULL,
                    data_recebimento TEXT NOT NULL,
                    doador TEXT NOT NULL)''')

conexao.commit()

# Funções para manipulação dos dados
def adicionar_voluntario():
    nome = entry_nome.get()
    idade = entry_idade.get()
    area = entry_area.get()
    disponibilidade = entry_disponibilidade.get()

    if nome and idade and area and disponibilidade:
        cursor.execute("INSERT INTO voluntarios (nome, idade, area_atuacao, disponibilidade) VALUES (?, ?, ?, ?)",
                       (nome, idade, area, disponibilidade))
        conexao.commit()
        messagebox.showinfo("Sucesso", "Voluntário adicionado com sucesso!")
    else:
        messagebox.showwarning("Erro", "Todos os campos são obrigatórios!")

def adicionar_alimento():
    tipo = entry_tipo.get()
    quantidade = entry_quantidade.get()
    data = entry_data.get()
    doador = entry_doador.get()

    if tipo and quantidade and data and doador:
        cursor.execute("INSERT INTO alimentos (tipo, quantidade, data_recebimento, doador) VALUES (?, ?, ?, ?)",
                       (tipo, quantidade, data, doador))
        conexao.commit()
        messagebox.showinfo("Sucesso", "Alimento adicionado com sucesso!")
    else:
        messagebox.showwarning("Erro", "Todos os campos são obrigatórios!")

# Interface gráfica com Tkinter
app = tk.Tk()
app.title("Sistema de Registro de Voluntários e Banco de Alimentos")

# Seção de Voluntários
frame_voluntarios = tk.Frame(app)
frame_voluntarios.pack(pady=10)

tk.Label(frame_voluntarios, text="Cadastro de Voluntários").grid(row=0, columnspan=2)
tk.Label(frame_voluntarios, text="Nome:").grid(row=1, column=0)
entry_nome = tk.Entry(frame_voluntarios)
entry_nome.grid(row=1, column=1)

tk.Label(frame_voluntarios, text="Idade:").grid(row=2, column=0)
entry_idade = tk.Entry(frame_voluntarios)
entry_idade.grid(row=2, column=1)

tk.Label(frame_voluntarios, text="Área de Atuação:").grid(row=3, column=0)
entry_area = tk.Entry(frame_voluntarios)
entry_area.grid(row=3, column=1)

tk.Label(frame_voluntarios, text="Disponibilidade:").grid(row=4, column=0)
entry_disponibilidade = tk.Entry(frame_voluntarios)
entry_disponibilidade.grid(row=4, column=1)

tk.Button(frame_voluntarios, text="Adicionar Voluntário", command=adicionar_voluntario).grid(row=5, columnspan=2, pady=5)

# Seção de Alimentos
frame_alimentos = tk.Frame(app)
frame_alimentos.pack(pady=10)

tk.Label(frame_alimentos, text="Registro de Alimentos").grid(row=0, columnspan=2)
tk.Label(frame_alimentos, text="Tipo:").grid(row=1, column=0)
entry_tipo = tk.Entry(frame_alimentos)
entry_tipo.grid(row=1, column=1)

tk.Label(frame_alimentos, text="Quantidade:").grid(row=2, column=0)
entry_quantidade = tk.Entry(frame_alimentos)
entry_quantidade.grid(row=2, column=1)

tk.Label(frame_alimentos, text="Data de Recebimento:").grid(row=3, column=0)
entry_data = tk.Entry(frame_alimentos)
entry_data.grid(row=3, column=1)

tk.Label(frame_alimentos, text="Doador:").grid(row=4, column=0)
entry_doador = tk.Entry(frame_alimentos)
entry_doador.grid(row=4, column=1)

tk.Button(frame_alimentos, text="Adicionar Alimento", command=adicionar_alimento).grid(row=5, columnspan=2, pady=5)

# Iniciar o aplicativo
app.mainloop()

# Fechar a conexão com o banco de dados ao encerrar o programa
conexao.close()
