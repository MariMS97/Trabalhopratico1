# Trabalho prático 1 - Banco de Dados

Livraria Via Literária 
Por: Mariana Magalhães Silva

## 1-Resumo do Negócio:

Este sistema destina-se a uma livraria online com uma variedade de livros em formato digital. O público-alvo inclui leitores de todos os gêneros literários, com foco tanto em ficção quanto em não-ficção. O sistema permite a criação de usuários, processamento de pagamentos e gerenciamento de inventário de livros. Principais atores incluem administradores, clientes e fornecedores. Serviços oferecidos abrangem a venda de e-books em formatos como PDF, ePub, MOBI, entre outros.

### Fluxos de Processos Cotidianos:
1. Cadastro de novos usuários.
2. Inserção de novos livros e atualização de inventário.
3. Processamento de pagamento e verificação de dados de pagamento (CPF e CEP).
4. Filtragem de livros por gênero e disponibilidade.
5. Geração de relatórios de vendas e consultas sobre os gêneros mais vendidos.

### Regras e Restrições:
- O CPF deve ter 11 dígitos e ser único.
- O CEP deve conter 8 dígitos.
- E-mails dos usuários devem ser únicos.
- Os preços dos livros são armazenados em uma faixa padrão com prefixo em reais.

## 2- Modelo Conceitual no padrão MER (Modelo Entidade-Relacionamento);

### Entidades e Atributos:
#### * Usuário:
1. Nome: Varchar (100);
2. Datanasc: Date “DD-MM-YYYY”;
3. Email: Varchar (50)(Chave Primária)(e-mail único para identificar o usuário);
4. Senha: Varchar (12).
  
#### * Pagamento:
1. CPF: Int “11 dígitos” (atributo único, chave primária);
2. CEP: Int “8 dígitos”;
3. Tipo de Pagamento: Varchar (50).
  
#### * Livros:
1. Código do livro: Int;
2. Gênero: Varchar (100);
3. Título: Varchar (100); 
4. Lançamento: Date “DD-MM-YYYY”;
5. Preço: Char “R$ 000,00”;
6. Formato do Arquivo: Char “PDF, e-PUB, MOBI, KPF, AZW, IBA”.

### Relacionamentos:
#### * Usuário-Pagamento: Relacionamento Realiza
1. Um Usuário pode ter várias formas de pagamento (relacionamento 1);
2. Exemplo de Cardinalidade: Um Usuário Realiza um ou mais Pagamentos, e um Pagamento pertence a um Usuário.
#### * Usuário-Livro: Relacionamento Compra
1. Um Usuário pode comprar vários Livros, e um Livro pode ser comprado por vários Usuários (relacionamento N);
2. Exemplo de Cardinalidade: Um Usuário Compra um ou mais Livros, e um Livro pode ser comprado por vários Usuários.

