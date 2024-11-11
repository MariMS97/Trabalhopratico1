# Trabalho prático 1 - Banco de Dados

Livraria Via Literária 
Por: Mariana Magalhães Silva

## 1-Resumo do Negócio->

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

## 2- Modelo Conceitual no padrão MER (Modelo Entidade-Relacionamento)->

### Entidades e Atributos:
#### * Usuário:
+ id_usuario: Int;
+ Nome: Varchar (100);
+ Datanasc: Date “DD-MM-YYYY”;
+ Email: Varchar (50)(Chave Primária)(e-mail único para identificar o usuário);
+ Senha: Varchar (12).
  
#### * Pagamento:
+ id_pagamento: Int;
+ id_usuario: Int;
+ CPF: Int “11 dígitos” (atributo único, chave primária);
+ CEP: Int “8 dígitos”;
+ Tipo de Pagamento: Varchar (50).
  
#### * Livros:
+ id_livro: Int;
+ Gênero: Varchar (100);
+ Título: Varchar (100); 
+ Lançamento: Date “DD-MM-YYYY”;
+ Preço_livro: Char “R$ 000,00”;
+ Formato do Arquivo: Char “PDF, e-PUB, MOBI, KPF, AZW, IBA”.

### Relacionamentos:
#### 1. Usuário-Pagamento: Relacionamento Realiza
- Um Usuário pode ter várias formas de pagamento (relacionamento 1);
- Exemplo de Cardinalidade: Um Usuário Realiza um ou mais Pagamentos, e um Pagamento pertence a um Usuário.
#### * Usuário-Livro: Relacionamento Compra
- Um Usuário pode comprar vários Livros, e um Livro pode ser comprado por vários Usuários (relacionamento N);
- Exemplo de Cardinalidade: Um Usuário Compra um ou mais Livros, e um Livro pode ser comprado por vários Usuários.

##  3. Modelo Lógico ->

As tabelas principais são:
+ Usuário (Nome, Datanasc, Email, Senha);
+ Pagamento (CPF, CEP, Tipo_pagamento);
+ Livro (cod_livro, Genero, Titulo, Lançamento, Preco_livro, Formato_arquivo).

Chaves Primarias e Estrangeiras:
+ id_usuario é chave primária em Usuarios.
+ id_livro é chave primária em Livros.
+ id_pagamento é chave primária em Pagamentos.
+ id_usuario em Pagamentos é uma chave estrangeira que referência Usuarios.

## 4. Normalização

### 1FN:

#### A 1ª Forma Normal exige:

+ Eliminação de grupos repetidos.
+ Garantia de que cada campo contenha apenas um valor (colunas atômicas).
+ Um identificador único para cada linha da tabela (chave primária).
  
#### Análise das Tabelas em 1FN

+ Tabela Usuário: já está em 1FN, pois cada coluna é atômica e não há grupos repetidos.
+ Tabela Pagamento: está quase em 1FN, mas alguns ajustes são necessários:
+ O CPF deve ser tratado como um valor atômico e único.
+ Tabela Livros: o campo FormatoArquivo não é atômico, pois contém múltiplos valores possíveis.
  
#### Ajustes para atender a 1FN
+ Criar uma tabela associativa LivroFormato para relacionar o Livro com FormatoArquivo, eliminando valores não atômicos.

###  Estrutura após 1FN

#### Tabela Usuario:

| Campo       | Tipo              |
|-------------|-------------------|          
|Id_usuario	  | Int               |
|Nome	        | Varchar(100)      |
|Datanasc	    | Date(DD-MM-YYYY)  |
|Email	      | Varchar(50)       |
|Senha	      | Varchar(255)      |

#### Tabela Pagamento:

| Campo       | Tipo              |
|-------------|-------------------|          
|id_pagamento | Int               |
|id_usuario   | Int               |
|CPF	        | Varchar(11)       |
|CEP	        | Varchar(8)        |
|Tipopagamento| Varchar(15)       |
                                                        
#### Tabela Livro: 

| Campo       | Tipo              |
|-------------|-------------------|          
|Id_livro	    | Int               |
|id_genero	  | Int               |
|Titulo	      | Varchar(100)      |
|Lancamento	  | Date              |
|Preco	      | Decimal(6, 2)     |
|Formato      | Varchar(20)       |
                                                      
#### Tabela GeneroLivro: 

| Campo       | Tipo              |
|-------------|-------------------|          
|Id_genero	  | Int               |
|Descricao	  | Varchar(100)      |  

### 2FN:

A 2FN exige que todas as tabelas estejam em 1FN e que todos os atributos não chave sejam totalmente dependentes da chave primária. As tabelas já possuem chaves primárias simples, os dados agora atendem à 2FN.

### 3FN:

#### A 3FN exige que:
+ A tabela esteja em 2FN.
+ Não haja dependências transitivas (um campo não chave não deve depender de outro campo não chave).
  
#### Ajustes para 3FN:
Separei o gêneros de livros (GeneroLivro) para evitar dependências transitivas.
Após a 3FN, o design está otimizado e livre de dependências transitivas.

### Modelo lógico depois da normalização:

#### Usuario:

+ Chaves Primárias: *id_usuario* identifica exclusivamente cada usuário.
+ Campos Únicos: *Email* garante que nenhum usuário tenha um email duplicado.
+ Observação: A coluna *Senha* foi aumentada para 255 caracteres para maior segurança, acomodando hash de senhas.

#### Pagamento:

+ Chaves Primárias: *id_pagamento* identifica unicamente cada pagamento.
+ Chave Estrangeira: *id_usuario* referencia Usuario, criando uma relação 1 (um usuário pode ter múltiplos pagamentos).
+ Campos Únicos: *CPF* garante que cada *CPF* seja único.

#### Livro:

+ Chaves Primárias: *id_livro* identifica unicamente cada livro.
+ Chave Estrangeira: *id_genero* referencia *GeneroLivro*, associando o livro a um gênero específico.

#### GeneroLivro:

+ Chaves Primárias: *id_genero* identifica cada gênero exclusivamente.
