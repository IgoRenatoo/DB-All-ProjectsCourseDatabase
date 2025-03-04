# 🛠️ Assertions no Banco de Dados

## 📖 O que é uma Assertion?

Uma assertion no banco de dados é uma restrição que define uma condição que deve ser sempre verdadeira para os dados presentes em uma ou mais tabelas. Ela funciona como uma garantia de integridade dos dados, assegurando que certas regras de negócio sejam mantidas.

Diferente de constraints como PRIMARY KEY ou FOREIGN KEY, que são aplicadas a colunas específicas, uma assertion pode abranger múltiplas tabelas e condições complexas.

## 🎯 Características Principais de uma Assertion
- **Validação de Dados:** Garante que os dados sempre atendam a uma condição específica.
- **Escopo Amplo:** Pode ser aplicada a várias tabelas ou relacionamentos.
- **Execução Automática:** É verificada automaticamente pelo banco de dados durante operações de inserção, atualização ou exclusão.
- **Uso Típico:** Implementação de regras de negócio complexas que não podem ser garantidas por constraints simples.
- **Persistência:** Permanece ativa até ser explicitamente removida.

## 📝 Exemplo: Garantir que o Saldo de uma Conta Nunca Seja Negativo
### Criando uma Assertion para Validar o Saldo
  ```sql
    CREATE ASSERTION SaldoNaoNegativo
    CHECK (
      NOT EXISTS (
          SELECT 1
          FROM Contas
          WHERE saldo < 0
      )
    );
  ```

### Explicação:
- **Objetivo**: Garantir que nenhuma conta tenha saldo negativo.
- **Condição**: A assertion verifica se existe algum registro na tabela Contas com saldo menor que zero.
- **Funcionamento**: Se a condição for violada, a operação que causou a violação será rejeitada.

## 🚀 Comandos para Gerenciar Assertions
### Criar uma Assertion
  ```sql
    CREATE ASSERTION NomeDaAssertion
    CHECK (condição);
  ```
### Remover uma Assertion
  ```sql
    DROP ASSERTION NomeDaAssertion;
  ```
### Verificar Assertions Existentes
  ```sql
    -- Depende do SGBD, mas geralmente pode ser consultado em tabelas de metadados.
    SELECT * FROM information_schema.assertions;
  ```
  
## 🔥 Vantagens do Uso de Assertions
1. **Integridade dos Dados**: Garante que regras de negócio complexas sejam sempre respeitadas.
2. **Centralização da Lógica**: Mantém as regras de validação diretamente no banco de dados, evitando duplicação de código em aplicações.
3. **Prevenção de Erros**: Bloqueia operações que possam comprometer a consistência dos dados.
4. **Facilidade de Manutenção**: Alterações nas regras de negócio podem ser feitas diretamente no banco, sem necessidade de modificar aplicações.

## 🚫 Limitações das Assertions
- **Suporte Limitado**: Nem todos os sistemas de banco de dados suportam assertions (por exemplo, MySQL não possui suporte nativo).
- **Desempenho**: Assertions complexas podem impactar o desempenho, especialmente em operações de grande volume.
- **Complexidade**: A criação e manutenção de assertions podem ser mais difíceis do que o uso de constraints simples.

## 📌 Quando Usar Assertions?
- Para garantir que regras de negócio complexas sejam aplicadas em todo o banco de dados.
- Quando constraints simples (como CHECK ou UNIQUE) não são suficientes para validar a integridade dos dados.
- Para evitar inconsistências em operações que envolvem múltiplas tabelas.
- Em cenários onde a validação deve ser feita diretamente no banco, sem depender de aplicações externas.

## 🔄 Alternativas a Assertions
Em bancos de dados que não suportam assertions, é possível usar:

- **Triggers**: Para implementar validações personalizadas antes ou após operações.
- **Stored Procedures**: Para encapsular lógica de validação em operações específicas.
- **Constraints Simples**: Para regras mais básicas, como CHECK, UNIQUE, ou FOREIGN KEY.
