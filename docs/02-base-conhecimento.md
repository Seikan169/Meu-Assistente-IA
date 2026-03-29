# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_usuario.csv` | CSV | Armazena o nível de experiência do usuário e temas já consultados para evitar repetições. |
| `glossario_cripto.json` | JSON | Base didática para traduzir "criptonês" em linguagem simples para iniciantes. |
| `projetos_confiaveis.json` | JSON | Curadoria de ativos com Market Cap relevante (Ex: BTC, ETH) para evitar análise de moedas suspeitas. |
| `guia_seguranca.pdf` | CSV | Manual de boas práticas sobre custódia (Wallets) e checklist de identificação de golpes.|

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

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

[Sua descrição aqui]

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
