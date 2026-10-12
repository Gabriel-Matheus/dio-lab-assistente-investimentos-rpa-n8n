# 🤖 Assistente de Investimentos RPA (n8n + Gemini AI)

Este projeto é um fluxo de automação (RPA) construído no **n8n** que atua como um consultor financeiro automatizado. Ele cruza dados de clientes com uma base de investimentos, utiliza Inteligência Artificial (Google Gemini) para redigir e-mails altamente personalizados e faz o disparo automático via Gmail.

## ⚙️ Como funciona o Workflow

O fluxo é acionado via Webhook e segue a seguinte lógica de execução:

1. **Recebimento de Dados (`Recebendo Clientes`):** Um Webhook (POST) recebe uma lista de clientes contendo nome, e-mail, saldo e perfil de investidor.
2. **Busca de Investimentos (`Listando Investimentos`):** Faz uma requisição HTTP para ler um arquivo CSV hospedado no GitHub contendo as opções de investimentos da corretora.
3. **Tratamento de Dados (`Editando Investimentos`):** Um nó de código extrai e estrutura os dados do CSV lido do GitHub, criando um array de objetos JSON.
4. **Cruzamento de Dados (`Juntando Clientes` / `Formatando a Junção`):** Cruza a lista de clientes com a lista de investimentos baseando-se no **perfil de investidor** (Conservador, Moderado ou Arrojado). Cada cliente recebe uma lista de recomendações exclusivas.
5. **Iteração (`Iterando Sobre os Clientes`):** Separa a lista final processando um cliente por vez (Split in Batches).
6. **Inteligência Artificial (`Criando Textos Para Enviar por Email`):** Envia os dados do cliente e as recomendações para o modelo **Google Gemini 3.8 Flash**. A IA atua como um consultor e gera um e-mail persuasivo, empático e com marcações HTML, retornando o resultado em um formato JSON restrito.
7. **Formatação (`Formatando Resposta do Email`):** Limpa as marcações markdown do retorno da IA para garantir que o JSON seja lido corretamente pelo n8n.
8. **Disparo de E-mail (`Send a message`):** Utiliza a API do Gmail para enviar a mensagem formatada para o e-mail do respectivo cliente.
9. **Encerramento (`Respond to Webhook`):** Retorna uma resposta de sucesso para finalizar o fluxo.

---

## 🛠️ Tecnologias e Integrações (Nós Utilizados)

- **Webhook:** Ponto de entrada da aplicação.
- **HTTP Request:** Para consumir o arquivo `.csv` externo.
- **Code (JavaScript):** Para manipulação complexa de arrays e limpeza de strings (Regex).
- **Merge:** Para juntar fluxos paralelos.
- **Split In Batches:** Para processar itens individualmente.
- **Google Gemini (PaLM) API:** LLM utilizado para gerar a cópia dos e-mails.
- **Gmail:** Serviço de envio de e-mails.

---

## 🔐 Pré-requisitos e Credenciais

Para que este workflow funcione em sua instância do n8n, você precisará configurar as seguintes credenciais:

1. **Gmail OAuth2:** Para o nó de envio de e-mails (`Send a message`).
2. **Google Gemini (PaLM) API:** Chave de API do Google AI Studio para o nó do LangChain gerar os textos.

---

## 📥 Formato de Entrada (Payload do Webhook)

O Webhook aguarda uma requisição `POST` com um JSON no seguinte formato (Body):

```json
{
  "clientes": [
    {
      "nome": "Ana Silva",
      "email": "ana@email.com",
      "saldo": "R$ 12.500,00",
      "perfil": "Conservador"
    },
    {
      "nome": "Carla Souza",
      "email": "carla@email.com",
      "saldo": "R$ 28.900,00",
      "perfil": "Arrojado"
    }
  ]
}

🚀 Como importar este workflow no seu n8n
Copie o conteúdo completo do arquivo JSON deste workflow.

Abra a interface do seu n8n.

Vá para a aba Workflows e crie um novo (Add Workflow).

Pressione Ctrl + V (ou Cmd + V no Mac) diretamente na tela em branco.

Configure suas credenciais nos nós que exigem autenticação (Gmail e Gemini).

Salve e ative o workflow!
