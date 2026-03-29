# Código da Aplicação

Esta pasta contém o código do seu agente financeiro.

## Estrutura Sugerida

```
import json
import os
from colorama import init
import git
import streamlit as st 
from google import genai
from google.genai import types

# 1. Configuração do Cliente
client = genai.Client(api_key="Sua_Chave_API_Aqui")

# 2. Função para carregar a Base de Conhecimento Local
def carregar_dados(caminho_arquivo):
    try:
        with open(caminho_arquivo, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        return None

# 3. Preparação do Contexto e Instruções
def preparar_contexto():
    perfil = carregar_dados('data/perfil_investidor.json')
    contexto_usuario = ""
    if perfil:
        contexto_usuario = f"Usuário: {perfil.get('nome')}, Perfil: {perfil.get('perfil_risco')}, Experiência: {perfil.get('experiencia_cripto')}"

    system_instruction = f"""
    Seu nome é SeikanCript. Você é um mentor educativo de criptoativos.
    CONTEXTO DO USUÁRIO ATUAL: {contexto_usuario}

    REGRAS RÍGIDAS:
    1. NUNCA invente preços. Use sempre a ferramenta de busca (Google Search) para verificar cotações atuais.
    2. NÃO recomende compra/venda. Use "Os dados atuais indicam..." ou "Historicamente...".
    3. EDUQUE: Explique termos técnicos com analogias simples.
    4. DISCLAIMER: Termine sempre com "Criptoativos são voláteis. Invista apenas o que pode perder."
    """
    return system_instruction, perfil

# --- LÓGICA DA INTERFACE STREAMLIT ---

st.set_page_config(page_title="SeikanCript", page_icon="🛡️")
st.title("🛡️ SeikanCript, Seu Mentor Cripto")

# Inicializamos o histórico de chat se não existir
if "messages" not in st.session_state:
    st.session_state.messages = []

# Carregamos o contexto inicial
instr_sistema, perfil = preparar_contexto()

# Boas-vindas personalizadas (aparece apenas no início)
if perfil and not st.session_state.messages:
    st.write(f"Olá, **{perfil.get('nome')}**! Como posso ajudar na sua jornada cripto hoje?")

# Mostra as mensagens anteriores na tela
for message in st.session_state.messages:
    with st.chat_message(message["role"]):
        st.markdown(message["content"])

# Campo de entrada (Chat Input)
if pergunta := st.chat_input("Sua dúvida sobre criptoativos..."):
    # 1. Adiciona a pergunta do usuário à tela e ao histórico
    st.chat_message("user").write(pergunta)
    st.session_state.messages.append({"role": "user", "content": pergunta})
    
    # 2. Processa a resposta
    with st.spinner("Consultando o mercado..."):
        try:
            response = client.models.generate_content(
                model="gemini-2.5-flash",
                contents=pergunta,
                config=types.GenerateContentConfig(
                    system_instruction=instr_sistema,
                    tools=[types.Tool(google_search=types.GoogleSearch())]
                )
            )
            resposta_ia = response.text
            
            # 3. Adiciona a resposta da IA à tela e ao histórico
            with st.chat_message("assistant"):
                st.markdown(resposta_ia)
            st.session_state.messages.append({"role": "assistant", "content": resposta_ia})
            
        except Exception as e:
            if "429" in str(e):
                st.error("Limite de cota atingido. Aguarde 1 minuto.")
            else:
                st.error(f"Erro técnico: {e}")
               
```
