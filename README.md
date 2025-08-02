# 🤖 Agente de IA para Engenharia

📘 **README – CresciBot**  
Assistente Técnico para Engenharia (n8n + Supabase + GPT-4o-mini)

---

## 📌 Visão Geral

Este workflow n8n implementa um **assistente inteligente para engenheiros**, integrado com **Supabase** e **OpenAI (GPT-4o-mini)**, capaz de:

- ✅ Receber perguntas de engenheiros via webhook  
- ✅ Identificar o usuário pelo UUID  
- ✅ Verificar se a pergunta já foi feita anteriormente (cache)  
- ✅ Gerar resposta objetiva + explicação técnica (caso nova pergunta)  
- ✅ Salvar a pergunta/resposta no Supabase  
- ✅ Retornar a resposta formatada de forma útil e prática  

---

## ⚙️ Componentes Principais

### 🔗 Webhook

- **Rota:** `POST /crescibot`  
- **Recebe o corpo JSON com os campos:**

```json
{
  "uuid": "id_do_usuario",
  "question": "pergunta feita pelo usuário"
}
🧑‍💻 [perfil_usuario]
Consulta o Supabase na tabela usuarios

Filtro: id = uuid recebido

Obtém o perfil do usuário que fez a pergunta

💾 [consultar cache de respostas]
Consulta a tabela respostas_ia

Filtro: usuario_id = id do usuário e pergunta = question

Verifica se a resposta já existe (cache)

🔀 [If]
Se já existe resposta → retorna do cache imediatamente

Se não existe → continua o fluxo para geração da resposta

🧠 [AI Agent]
Usa o modelo GPT-4o-mini

Com o seguinte system message customizado:

text
Copiar
Editar
Você é um assistente técnico especializado em engenharia...
Sempre responda primeiro com um resumo direto e prático,
depois com uma explicação técnica opcional.
✔️ Garante que as respostas sejam úteis para engenheiros ocupados, em dois blocos:

✅ Resumo direto

📘 Explicação técnica (opcional)

📝 [salvando resposta e pergunta]
Salva no Supabase (respostas_ia) os seguintes campos:

text
Copiar
Editar
usuario_id  
pergunta  
resposta  
modelo = GPT-4o-mini  
🔁 [Respond to Webhook]
Retorna a resposta ao usuário em JSON:

json
Copiar
Editar
{
  "resposta": "✅ Resumo direto... 📘 Explicação técnica..."
}
📂 Tabelas envolvidas no Supabase
usuarios
Deve conter o campo: id (UUID)

respostas_ia
Campos obrigatórios:

Campo	Tipo	Descrição
usuario_id	UUID	ID do usuário que fez a pergunta
pergunta	text	Texto da pergunta feita
resposta	text	Texto completo da resposta gerada
modelo	text	Nome do modelo usado (ex: GPT-4o-mini)

🚀 Exemplo de Uso (via API externa)
Requisição:
http
Copiar
Editar
POST /crescibot
Content-Type: application/json
json
Copiar
Editar
{
  "uuid": "31737869-e764-4a54-90b3-5d56ab35069d",
  "question": "Em sistemas trifásicos estrela equilibrados, há corrente no neutro?"
}
Resposta:
json
Copiar
Editar
{
  "resposta": "✅ Resposta direta: Não, se a carga for perfeitamente equilibrada, não há corrente no neutro. Mas na prática, desbalanceamentos geram corrente.\n\n📘 Explicação técnica: ..."
}
✅ Vantagens do Workflow
⚡ Resposta rápida e prática para engenheiros

💬 Respostas técnicas confiáveis com OpenAI

💾 Cache para evitar reprocessamento

🧠 Formatação clara com separação entre resumo e teoria

🔌 Pronto para integração com chatbot, web, mobile ou WhatsApp

🔧 Melhorias Futuras (opcional)
Adicionar controle de limite de uso por usuário

Criar dashboard para estatísticas de perguntas mais frequentes

Implementar rota GET /respostas/:uuid para listar histórico individual
