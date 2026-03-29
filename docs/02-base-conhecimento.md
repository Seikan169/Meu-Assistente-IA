# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_usuario.csv` | CSV | Armazena o nível de experiência do usuário e temas já consultados para evitar repetições. |
| `glossario_cripto.json` | JSON | Base didática para traduzir "criptonês" em linguagem simples para iniciantes. |
| `projetos_confiaveis.json` | JSON | Curadoria de ativos com Market Cap relevante (Ex: BTC, ETH) para evitar análise de moedas suspeitas. |
| `guia_seguranca.pdf` | CSV | Manual de boas práticas sobre custódia (Wallets) e checklist de identificação de golpes.|



---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os dados foram expandidos para incluir uma camada de sentimento de mercado e indicadores técnicos simplificados (como Médias Móveis e RSI). Além disso, o glossário foi adaptado para traduzir o 'criptonês' para uma linguagem que um iniciante em Engenharia de Software ou um investidor comum consiga entender sem atrito.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

O SeikanCript utiliza uma arquitetura RAG (Retrieval-Augmented Generation) para acessar um banco de dados vetorial com manuais de educação financeira. Para dados de mercado, o agente utiliza Function Calling conectado à API da CoinGecko, garantindo que as análises técnicas sejam baseadas em preços reais e não em dados de treinamento obsoletos.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são integrados de forma híbrida:

1• As regras de comportamento e o glossário básico de termos são fixados no System Prompt para garantir a personalidade educativa.

2• O perfil do investidor e o histórico de transações são injetados dinamicamente no Contexto da Janela de Chat a cada nova interação.

3• Caso o usuário pergunte sobre um ativo específico, o agente dispara uma Function Call para buscar os dados em tempo real (API), que retornam e são anexados temporariamente ao prompt para a geração da resposta final.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
[CONTEXTO DO SISTEMA]
Nome do Agente: SeikanCript
Perfil do Usuário: João Silva (Investidor Iniciante)
Objetivo: Acumular 0.1 BTC até Dez/2026
Saldo em Cripto: US$ 500.00 (em BTC)

[ÚLTIMAS ATIVIDADES REGISTRADAS]
- 01/03: Compra de US$ 100.00 em BTC.
- 10/03: Dúvida tirada sobre "O que é RSI".
- 15/03: Alerta de volatilidade enviado (queda de 5% no BTC).

[DADOS DA API EM TEMPO REAL]
- Preço Atual BTC: US$ 68.450,00
- Variação 24h: +1.2%
- Índice Fear & Greed: 65 (Ganância)

[PERGUNTA DO USUÁRIO]
"Seikan, o BTC subiu um pouco, vale a pena comprar mais os meus US$ 50 mensais hoje?"
```
