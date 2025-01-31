# Personalizando o Banco de Dados com Índices e Procedures

## 💻 Descrição do Projeto

- **Parte 1** – Criação de índices para consultas para o cenário de company com as perguntas (queries sql) para recuperação de informações. Sendo assim, dentro do script será criado os índices com base na consulta SQL.  

> Será levado em consideração para criação dos índices ~> `Quais os dados mais acessados ?` e `Quais os dados mais relevantes no contexto?`.

Lembre-se da função do índice... ele impacta diretamente na velocidade da busca pelas informações no SGBD. Crie apenas aqueles que são importantes.

  - **Perguntas**:
    - Qual o departamento com maior número de pessoas? 
    - Quais são os departamentos por cidade? 
    - Relação de empregrados por departamento.

 A criação do índice pode ser criada via ALTER TABLE ou CREATE Statement, como segue o exemplo: 
  ```sql
    ALTER TABLE customer ADD UNIQUE index_email(email); 
  ```
  ```sql
    CREATE INDEX index_ativo_hash ON exemplo(ativo) USING HASH; 
  ```
- **Parte 2** - Utilização de procedures para manipulação de dados em Banco de Dados 
  - Criar uma procedure que possua as instruções de inserção, remoção e atualização de dados no banco de dados. As instruções devem estar dentro de estruturas condicionais (como CASE ou IF).  
  - Além das variáveis de recebimento das informações, a procedure deverá possuir uma variável de controle. Essa variável de controle irá determinar a ação a ser executada. Ex: opção 1 – select, 2 – update, 3 – delete. 
 
- **Entregável**: 
  - Crie as queries para responder essas perguntas 
  - Crie o índice para cada tabela envolvida (de acordo com a necessidade) 
  - Tipo de indice utilizado e motivo da escolha (via comentário no script ou readme) 
  - Script SQL com a procedure criada é chamada para manipular os dados de universidade e e-commerce. Podem ser criados dois arquivos distintos, assim como a utilização do mesmo script para criação das procedures. Fique atento para selecionar o banco de dados antes da criação da procedure.  

## ✅ Conceitos Aprendidos

## 💻 Entidades e Atributos

## 🤝 Relacionamentos

## 📊 Diagrama

## 🦶 Próximos Passos

## 👀 Observações

## 🏁 CONCLUSÃO
