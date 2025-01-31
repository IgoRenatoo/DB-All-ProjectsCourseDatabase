# 🛠️ Exemplos de Comandos DDL - Data Definition Language

## 🖥️ Comandos para Tabelas - CREATE | DROP | ALTER | RENAME | VIEW
  - **CREATE**: Criar uma tabela chamada `Clientes` com campos `id`, `Nome` e `CPF`:
    ```sql
    CREATE TABLE Clientes (
        id INT PRIMARY KEY,
        Nome VARCHAR(100),
        CPF VARCHAR(11)
    );
    ```
  - **DROP**: Remove uma tabela do banco de dados de nome `Clientes`.  
    ```sql
      DROP TABLE Clientes;
    ```
  - **ALTER**: Adicionar uma coluna chamada `Telefone` à tabela `Clientes`
    ```sql
      ALTER TABLE Clientes ADD Telefone VARCHAR(15);
    ```
  - **RENAME**: Renomear a tabela `Clientes` para `NovoClientes`.
    ```sql
      RENAME TABLE Clientes TO NovoClientes;
    ```
  - **VIEW**: Criar uma view chamada `ResumoClientes` para exibir os nomes e telefones dos clientes:
    ```sql
      CREATE VIEW ResumoClientes AS -- Ela não manipula dados, apenas apresenta uma forma visual.
      SELECT Nome, Telefone
      FROM Clientes;
    ```

## 🚀 Atomic DDL

  Imagine que temos duas contas bancárias: uma conta de origem (Cliente A) e uma conta de destino (Cliente B).
  
  O objetivo é transferir R$ 500 da conta A para a conta B. Essa operação envolve dois passos principais:

    - Debitar R$ 500 da conta de origem.
    - Creditar R$ 500 na conta de destino.

  Se qualquer uma dessas etapas falhar (por exemplo, falta de saldo ou falha no sistema), a transação deve ser completamente revertida.

  ```sql
    -- Início da transação
  BEGIN;

  -- PASSO 1: Verificar se a conta de origem tem saldo suficiente
  DO $$
  BEGIN
      IF (SELECT Saldo FROM Contas WHERE id = 1) < 500 THEN
          -- Se o saldo for insuficiente, registra um erro e faz rollback explícito
          RAISE NOTICE 'Saldo insuficiente na Conta A. Transação será revertida.';
          ROLLBACK; -- Reverte explicitamente
          RETURN;   -- Finaliza o bloco de execução
      END IF;
  END $$;

  -- PASSO 2: Debitar R$ 500 da conta de origem (Cliente A)
  UPDATE Contas
  SET Saldo = Saldo - 500
  WHERE id = 1;

  -- PASSO 3: Creditar R$ 500 na conta de destino (Cliente B)
  UPDATE Contas
  SET Saldo = Saldo + 500
  WHERE id = 2;

  -- PASSO 4: Confirmar a transação
  COMMIT;
  ```
