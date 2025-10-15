# Back-End do Aplicativo Sabor Local

## 1. Visão Geral e Arquitetura

O back-end do Sabor Local foi projetado para servir como o **cérebro do aplicativo**. Sua principal responsabilidade é gerenciar e persistir de forma segura todos os dados dos usuários, oferecendo uma API RESTful confiável para que o front-end (desenvolvido em FlutterFlow) possa consumir e manipular essas informações.

* **Plataforma:** Xano
* **Banco de Dados:** PostgreSQL(gerenciado pelo Xano)
* **Autenticação:** Baseada em Token (JWT)

---

## 2. Estrutura do Banco de Dados

A base de dados foi modelada para garantir a integridade e o relacionamento entre os dados do usuário.

* **`user`**: Tabela principal que armazena os dados de cadastro do cliente (nome, telefone, email, senha). Contém campos de controle como:
    * `is_active` (booleano): Indica se o usuário completou a verificação por e-mail.
    * `profile_complete` (booleano): Indica se o usuário já cadastrou pelo menos um endereço.

* **`verificacao_cadastro`**: Tabela temporária que armazena o código de verificação enviado ao usuário. Este registro é associado ao `user_id` e apagado após a validação bem-sucedida.

* **`dados_adicionais`**: Armazena os múltiplos endereços de um usuário(nome atribuído ao local, CEP, rua, bairro, número, complemento(opcional), sempre referenciando o `user_id` correspondente.

### Diagrama de Relacionamento

Essa é uma observação fantástica e uma ideia excelente. Você está pensando exatamente como um desenvolvedor profissional. Apresentar a documentação via Swagger (OpenAPI) é o padrão da indústria e eleva o nível do seu portfólio de "um projeto que funciona" para "um projeto com documentação profissional e interativa".

Vamos refinar nosso plano. Você está certo, a parte das tabelas precisa de mais destaque, e o link do Swagger será a estrela da seção da API.

Aqui está a nova estrutura, mais completa e profissional.

Passo 1: Refinar o README.md no GitHub (A Documentação Central)
Seu repositório no GitHub será o "hub" técnico. Vamos enriquecer a seção do banco de dados com suas descrições e substituir a lista manual de endpoints pelo link do Swagger.

Copie e cole este conteúdo atualizado no seu arquivo README.md:

Markdown

# Back-End do Aplicativo Sabor Local

## 1. Visão Geral e Arquitetura

O back-end do Sabor Local foi projetado para servir como o **cérebro do aplicativo**. Sua principal responsabilidade é gerenciar e persistir de forma segura todos os dados dos usuários, oferecendo uma API RESTful confiável para que o front-end (desenvolvido em FlutterFlow) possa consumir e manipular essas informações.

* **Plataforma:** Xano (No-Code/Low-Code Back-end)
* **Banco de Dados:** PostgreSQL (gerenciado pelo Xano)
* **Autenticação:** Baseada em Token (JWT)

---

## 2. Estrutura do Banco de Dados

A base de dados foi modelada para garantir a integridade e o relacionamento entre os dados do usuário, seguindo os requisitos do projeto.

* **`user`**: Tabela principal que armazena os dados de cadastro do cliente (nome, número, email, senha). Nela também estão presentes:
    * `is_active` (booleano): Quando `true`, indica que o usuário já se autenticou no app via e-mail.
    * `profile_complete` (booleano): Quando `true`, indica que o usuário já cadastrou pelo menos um endereço em sua conta.

* **`verificacao_cadastro`**: Tabela temporária que armazena o `random_number` (código de verificação) enviado ao usuário via SendGrid. Este registro é associado ao `user_id` e apagado após a autenticação bem-sucedida, quando o status `is_active` do usuário se torna `true`.

* **`dados_adicionais`**: Armazena os múltiplos endereços de um usuário (nome do local, CEP, rua, etc.), sempre referenciando o `user_id` correspondente. O cadastro bem-sucedido de um endereço atualiza o status `profile_complete` do usuário para `true`.

### Diagrama de Relacionamento

[ Tabela: user ]
|
|---( 1 para 1 )---> [ Tabela: verificacao_cadastro ]
|                     (liga pelo user_id)
|
`---( 1 para Muitos )--> [ Tabela: dados_adicionais ]
(liga pelo user_id)

## 3. Fluxo de API (Jornada do Usuário)

Os endpoints foram projetados para seguir uma sequência lógica que acompanha a jornada do usuário dentro do aplicativo:

1.  **Cadastro Inicial (`POST /verificacao_cadastro`):** O usuário preenche seus dados. A API cria um pré-cadastro (`is_active = false`) e envia um e-mail de verificação.
2.  **Ativação da Conta (`POST /finalizar_cadastro`):** O front-end envia o código inserido pelo usuário para validação. Após o código ser validado, este endpoint é chamado para ativar a conta do usuário (`is_active = true`) e limpar os dados de verificação.
3.  **Login (`POST /auth/login`):** Com a conta ativa, o usuário pode fazer login para obter um `authToken`, que é necessário para todas as ações futuras.
4.  **Ações Autenticadas (Ex: `POST /dados_adicionais`):** Uma vez logado, o usuário pode realizar ações protegidas, como cadastrar seus endereços. Em breve, o usuário conseguirá visualizar seus endereços salvos.

---

## 4. Documentação da API (Swagger/OpenAPI)

Para uma exploração detalhada, interativa e técnica de cada endpoint, a documentação completa da API foi gerada e está disponível através do Swagger.

### ➡️ **[Acessar a Documentação Interativa da API](https://x8ki-letl-twmt.n7.xano.io/api:f2k3pTaR?token=WlQzSsho6aJizNjoldZQxmcOAV8)**
