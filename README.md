# Orquestrador de Microserviços

Este projeto implementa um orquestrador de microserviços com um frontend simples para interagir com eles.

## Descrição

O orquestrador é responsável por receber requisições do frontend, identificar a operação desejada e encaminhá-la para o microserviço correspondente. Ele também lida com as respostas dos microserviços e as retorna ao cliente.

Atualmente, o foco principal é fornecer uma interface para um serviço de calculadora, onde diferentes operações matemáticas são tratadas por microserviços distintos.

## Estrutura do Projeto

```
microservico/
├── api/
│   └── index.js        # Entrypoint para Vercel (se usando serverless functions)
├── config/
│   └── serviceUrls.js  # Configuração das URLs dos microserviços
├── src/
│   ├── app.js          # Lógica principal do Express (rotas, orquestração)
├── frontend.html       # Interface do usuário para interagir com os serviços
├── package.json        # Dependências e scripts do projeto
├── vercel.json         # Configuração de deploy para a Vercel
└── README.md           # Este arquivo
```

## Microserviços Disponíveis

A lista de microserviços e suas respectivas URLs de endpoint está configurada no arquivo `config/serviceUrls.js`.

Exemplo de microserviço já integrado:
*   **Serviço de Soma**: Responsável por realizar a operação de adição.

## Como Executar Localmente

1.  **Clone o repositório:**
    ```bash
    git clone <URL_DO_REPOSITORIO_PRINCIPAL>
    cd microservico
    ```

2.  **Instale as dependências:**
    ```bash
    npm install
    ```

3.  **Inicie o servidor de orquestração:**
    ```bash
    npm start
    ```
    Isso geralmente iniciará o servidor na porta definida em `src/server.js` (por padrão, 3000 ou a porta definida pela Vercel CLI localmente).

4.  **Acesse o frontend:**
    Abra seu navegador e acesse `http://localhost:PORTA/` (substitua `PORTA` pela porta em que o servidor está rodando).

**Nota:** Certifique-se de que os microserviços individuais (como o `add_service`) estejam em execução e acessíveis nas URLs configuradas em `config/serviceUrls.js` se você não estiver usando as versões publicadas (ex: no Vercel).

## Deploy

### Vercel

Este projeto está configurado para deploy na Vercel. O arquivo `vercel.json` define como o build e o roteamento devem ser tratados.

Para fazer o deploy:
1.  Certifique-se de ter a [Vercel CLI](https://vercel.com/docs/cli) instalada e configurada.
2.  Execute o comando de deploy no diretório raiz do projeto:
    ```bash
    vercel
    ```
    Ou, para um deploy de produção:
    ```bash
    vercel --prod
    ```

O microserviço de soma (`add_service`) foi deployado separadamente e sua URL de produção está configurada no orquestrador.

## Endpoints da API do Orquestrador

*   `GET /`: Serve o arquivo `frontend.html`.
*   `POST /calculate`: Endpoint principal para o orquestrador.
    *   **Corpo da Requisição (JSON):**
        ```json
        {
          "operation": "nomeDaOperacao", // Ex: "add", "subtract", etc.
          "params": { /* parâmetros específicos da operação */ } // Ex: { "a": 5, "b": 3 } para soma
        }
        ```
    *   **Respostas:**
        *   `200 OK`: Sucesso, com o resultado da operação no corpo da resposta.
        *   `400 Bad Request`: Erro nos parâmetros da requisição ou operação desconhecida.
        *   `500 Internal Server Error`: Erro interno no orquestrador.
        *   `503 Service Unavailable`: Microserviço específico indisponível ou não respondeu.
        *   Outros códigos de status podem ser repassados diretamente do microserviço.

Desenvolvido por Erick Nicolau - 2024/2025
