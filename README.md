# CALCULADORA-PYTHON-COM-SOMA-ENTRE-DATAS-
Calculadora desktop em Python com interface moderna, histórico de operações e cálculo entre datas usando calendário integrado. Projeto para portfólio.
🧮 Calculadora Profissional em Python
📌 Descrição

Este projeto consiste em uma calculadora desktop profissional desenvolvida em Python, com interface gráfica moderna inspirada na calculadora do Windows.
O objetivo do projeto é demonstrar habilidades em:

Desenvolvimento de aplicações desktop

Programação orientada a objetos

Manipulação de eventos de teclado

Interface gráfica com Tkinter

Integração com calendário (Date Picker)

Geração de executável (.exe)

Projeto ideal para portfólio 💼🚀

🎯 Funcionalidades

✔ Operações matemáticas básicas

Soma

Subtração

Multiplicação

Divisão

Porcentagem

✔ Suporte completo ao teclado

Teclado numérico

Enter para calcular

Backspace para apagar

Delete para limpar

✔ Histórico de cálculos

Registro automático das operações

Botão para limpar histórico

✔ Cálculo entre datas

Seleção via calendário (tkcalendar)

Diferença em dias entre duas datas

Resultado exibido no visor

Registro no histórico

✔ Interface profissional

Tema escuro

Layout organizado

Botões estilizados

UX inspirada no Windows

✔ Versão executável

Geração de arquivo .exe

Não requer Python instalado

🖥️ Tecnologias Utilizadas

Python 3.14

Tkinter – Interface gráfica

tkcalendar – Seletor de datas

PyInstaller – Geração do executável

📷 Demonstração

(Aqui você pode adicionar prints do programa rodando)

Exemplo:

/assets/print1.png
/assets/print2.png

⚙️ Como executar o projeto
🔹 Rodar pelo Python
python calculadora.py

🔹 Gerar o executável (.exe)
python -m pip install pyinstaller
python -m pyinstaller --onefile --noconsole calculadora.py


O executável será gerado em:

dist/calculadora.exe

📂 Estrutura do projeto
calculadora/
├── calculadora.py
├── build/
├── dist/
│   └── calculadora.exe
└── README.md

🧠 Aprendizados

Com este projeto foi possível aplicar conceitos importantes como:

Programação orientada a objetos (POO)

Tratamento de exceções

Manipulação de datas

Design de interface (UI/UX)

Empacotamento de aplicações

🚀 Melhorias futuras

Modo claro

Versão científica

Exportar histórico para arquivo

Splash screen

Ícone personalizado

Atualizador automático

👨‍💻 Autor

Leonardo Estevão Alves
📌 Desenvolvedor em formação
📌 Projeto para portfólio

⭐ Contribuições

Sinta-se à vontade para:

Fazer fork

Abrir issues

Enviar pull requests

🔥 Descrição curta (pra repositório)

Calculadora desktop profissional desenvolvida em Python com interface gráfica moderna, histórico de operações, suporte a teclado e cálculo entre datas usando calendário integrado.import tkinter as tk
from datetime import datetime
from tkcalendar import DateEntry

class Calculadora:
    def __init__(self, root):
        self.root = root
        self.root.title("Calculadora Profissional")
        self.root.geometry("620x580")
        self.root.resizable(False, False)
        self.root.configure(bg="#1f1f1f")

        self.expressao = ""

        self.interface()
        self.bind_teclado()

    # ================= INTERFACE =================

    def interface(self):
        topo = tk.Frame(self.root, bg="#1f1f1f")
        topo.pack(fill="x", padx=20, pady=15)

        self.visor_var = tk.StringVar(value="0")
        self.visor = tk.Entry(
            topo,
            textvariable=self.visor_var,
            font=("Segoe UI", 36),
            bg="#1f1f1f",
            fg="white",
            bd=0,
            justify="right"
        )
        self.visor.pack(fill="x", ipady=15)

        corpo = tk.Frame(self.root, bg="#1f1f1f")
        corpo.pack(fill="both", expand=True, padx=15)

        self.teclado = tk.Frame(corpo, bg="#1f1f1f")
        self.teclado.pack(side="left", fill="both", expand=True)

        self.historico_frame = tk.Frame(corpo, bg="#262626", width=210)
        self.historico_frame.pack(side="right", fill="y")

        tk.Label(
            self.historico_frame,
            text="Histórico",
            bg="#262626",
            fg="white",
            font=("Segoe UI", 12, "bold")
        ).pack(pady=5)

        self.lista = tk.Listbox(
            self.historico_frame,
            bg="#262626",
            fg="white",
            bd=0,
            highlightthickness=0,
            font=("Segoe UI", 10)
        )
        self.lista.pack(fill="both", expand=True, padx=10)

        tk.Button(
            self.historico_frame,
            text="Limpar Histórico",
            bg="#d63031",
            fg="white",
            bd=0,
            font=("Segoe UI", 10),
            command=self.limpar_historico
        ).pack(fill="x", padx=10, pady=8)

        # ===== CALENDÁRIO =====

        tk.Label(
            self.historico_frame,
            text="Diferença entre datas",
            bg="#262626",
            fg="white",
            font=("Segoe UI", 10, "bold")
        ).pack(pady=5)

        self.data1 = DateEntry(
            self.historico_frame,
            date_pattern="dd/mm/yyyy",
            background="darkblue",
            foreground="white"
        )
        self.data1.pack(fill="x", padx=10, pady=3)

        self.data2 = DateEntry(
            self.historico_frame,
            date_pattern="dd/mm/yyyy",
            background="darkblue",
            foreground="white"
        )
        self.data2.pack(fill="x", padx=10, pady=3)

        tk.Button(
            self.historico_frame,
            text="Calcular dias",
            bg="#00b894",
            fg="white",
            bd=0,
            font=("Segoe UI", 10),
            command=self.calcular_datas
        ).pack(fill="x", padx=10, pady=6)

        self.criar_botoes()

    # ================= BOTÕES =================

    def criar_botoes(self):
        botoes = [
            ("C", self.limpar), ("⌫", self.apagar), ("%", lambda: self.clicar("/100")), ("÷", lambda: self.clicar("/")),
            ("7", lambda: self.clicar("7")), ("8", lambda: self.clicar("8")), ("9", lambda: self.clicar("9")), ("×", lambda: self.clicar("*")),
            ("4", lambda: self.clicar("4")), ("5", lambda: self.clicar("5")), ("6", lambda: self.clicar("6")), ("-", lambda: self.clicar("-")),
            ("1", lambda: self.clicar("1")), ("2", lambda: self.clicar("2")), ("3", lambda: self.clicar("3")), ("+", lambda: self.clicar("+")),
            ("0", lambda: self.clicar("0")), (".", lambda: self.clicar(".")), ("=", self.calcular)
        ]

        for i in range(5):
            self.teclado.rowconfigure(i, weight=1)
        for j in range(4):
            self.teclado.columnconfigure(j, weight=1)

        pos = 0
        for l in range(5):
            for c in range(4):
                if pos >= len(botoes): break
                texto, cmd = botoes[pos]
                pos += 1

                cor = "#2d2d2d"
                if texto in "+-×÷":
                    cor = "#ff9f43"
                elif texto == "=":
                    cor = "#00b894"
                elif texto == "C":
                    cor = "#d63031"
                elif texto == "⌫":
                    cor = "#fbc531"

                b = tk.Button(
                    self.teclado,
                    text=texto,
                    font=("Segoe UI", 14),
                    bg=cor,
                    fg="white",
                    bd=0,
                    command=cmd
                )
                b.grid(row=l, column=c, padx=5, pady=5, sticky="nsew")

    # ================= LÓGICA =================

    def atualizar(self):
        self.visor_var.set(self.expressao if self.expressao else "0")

    def clicar(self, v):
        self.expressao += v
        self.atualizar()

    def limpar(self):
        self.expressao = ""
        self.atualizar()

    def apagar(self):
        self.expressao = self.expressao[:-1]
        self.atualizar()

    def calcular(self):
        try:
            res = eval(self.expressao)
            registro = f"{self.expressao} = {res}"
            self.lista.insert(0, registro)

            self.expressao = str(res)
            self.atualizar()
        except:
            self.visor_var.set("Erro")
            self.expressao = ""

    # ================= HISTÓRICO =================

    def limpar_historico(self):
        self.lista.delete(0, tk.END)

    # ================= DATAS =================

    def calcular_datas(self):
        d1 = self.data1.get_date()
        d2 = self.data2.get_date()

        diff = abs((d2 - d1).days)

        msg = f"{d1.strftime('%d/%m/%Y')} → {d2.strftime('%d/%m/%Y')} = {diff} dias"
        self.lista.insert(0, msg)
        self.visor_var.set(diff)

    # ================= TECLADO =================

    def bind_teclado(self):
        self.root.bind("<Key>", self.teclado_evento)

    def teclado_evento(self, e):
        k = e.keysym

        if k.isdigit():
            self.clicar(k)
        elif k in ["plus","KP_Add"]:
            self.clicar("+")
        elif k in ["minus","KP_Subtract"]:
            self.clicar("-")
        elif k in ["asterisk","KP_Multiply"]:
            self.clicar("*")
        elif k in ["slash","KP_Divide"]:
            self.clicar("/")
        elif k in ["Return","KP_Enter"]:
            self.calcular()
        elif k == "BackSpace":
            self.apagar()
        elif k == "Delete":
            self.limpar()
        elif k in ["period","KP_Decimal"]:
            self.clicar(".")

# ================= RUN =================

if __name__ == "__main__":
    root = tk.Tk()
    app = Calculadora(root)
    root.mainloop()


