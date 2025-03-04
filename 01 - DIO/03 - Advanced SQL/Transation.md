# 🔄 Transação em Bancos de Dados

## 📖 O que são Transition?
Transações são sequências de operações executadas como uma única unidade lógica de trabalho em um banco de dados. Elas garantem que todas as operações sejam concluídas com sucesso (commit) ou nenhuma alteração seja persistida (rollback) em caso de falha, mantendo a integridade dos dados.

## 🎯 Propriedades ACID das Transition
- **Atomicidade**: Todas as operações da transação são concluídas ou nenhuma é aplicada.
- **Consistência**: Mantém regras de integridade (como constraints e relacionamentos).
- **Isolamento**: Operações de transações concorrentes não interferem entre si.
- **Durabilidade**: Alterações persistem após o commit, mesmo em falhas de sistema.

## 📝 Exemplo: Criação de Transition
1. Transação Simples (COMMIT)
  ```sql
    BEGIN TRANSACTION;
    UPDATE contas SET saldo = saldo - 100 WHERE id = 1;  -- Débito
    UPDATE contas SET saldo = saldo + 100 WHERE id = 2;  -- Crédito
    COMMIT;  -- Confirma as alterações
  ```
2. Rollback em Caso de Erro
  ```sql
    BEGIN TRY
        BEGIN TRANSACTION;
        DELETE FROM pedidos WHERE id = 123;  -- Remove pedido
        DELETE FROM itens_pedido WHERE pedido_id = 123;  -- Remove itens
        COMMIT;
    END TRY
    BEGIN CATCH
        ROLLBACK;  -- Desfaz tudo se houver erro
        THROW;  -- Retorna a mensagem de erro
    END CATCH
  ```
3. Savepoint (Ponto de Salvamento)
  ```sql
    BEGIN TRANSACTION;
    INSERT INTO clientes (nome) VALUES ('Maria');
    SAVEPOINT savepoint1;  -- Marca um ponto de restauração
    UPDATE contas SET saldo = 500 WHERE cliente_id = 1;
    -- Se algo der errado:
    ROLLBACK TO savepoint1;  -- Volta ao savepoint
    COMMIT;
  ```
4. Níveis de Isolamento (Exemplo)
  ```sql
    -- PostgreSQL
    SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- Define o nível
    BEGIN;
    SELECT * FROM contas WHERE id = 1;  -- Leitura consistente
    COMMIT;
  ```

## 🚀 Comandos para Gerenciar Transition
### 👨‍💻 Transações na Prática

- Iniciar uma Transação MySQL
  ```sql
    START TRANSACTION;
  ```
- Iniciar uma Transação PostgreSQL/SQLServer
  ```sql
    BEGIN TRANSACTION;
  ```
- Confirmar Alterações (COMMIT)
  ```sql
    COMMIT;  -- Persiste as mudanças
  ```
- Desfazer Alterações (ROLLBACK)
  ```sql
    ROLLBACK;  -- Cancela todas as operações da transação
  ```
- Definir Savepoints
  ```sql
    SAVEPOINT savepoint_nome;  -- Cria um ponto de restauração parcial
  ```
- Configurar Nível de Isolamento PostgreSQL
  ```sql
    SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  -- Nível mais restritivo
  ```
- Configurar Nível de Isolamento SQLServer
  ```sql
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  -- Evita bloqueios
  ```
- Ver Transações Ativas PostgreSQL
  ```sql
    SELECT * FROM pg_stat_activity WHERE state = 'active';  -- Transações em andamento
  ```
- Ver Transações Ativas SQLServer
  ```sql
    SELECT session_id, transaction_id, status FROM sys.dm_tran_active_transactions;
  ```
- Monitorar Deadlocks SQLServer
  ```sql
    SELECT * FROM sys.dm_tran_locks WHERE request_status = 'WAIT';  -- Identifica conflitos
  ```
- Analisar Logs de Transações
  ```sql
    MySQL: Binlog (registra todas as alterações).
    PostgreSQL: WAL (Write-Ahead Logging).
    SQLServer: Transaction Log (armazena histórico de operações).
  ```

## 🔥 Vantagens das Transition
1. **Integridade dos dados**: Garante que operações complexas sejam completas ou revertidas.
2. **Controle de concorrência**: Gerencia acesso simultâneo para evitar inconsistências.
3. **Recuperação de erros**: Permite rollback automático ou manual em falhas.
4. **Auditoria**: Logs detalhados para rastrear alterações.

## 🚫 Limitações das Transition
- **Overhead de desempenho**: Bloqueios (locks) e logs reduzem a velocidade.
- **Complexidade**: Gerenciar deadlocks e níveis de isolamento exige cuidado.
- **Consumo de recursos**: Transações longas ocupam memória e mantêm locks ativos.

## 📌 Quando Usar Transition
- Operações com múltiplos passos (ex: transferência bancária).
- Sistemas críticos: Financeiros, saúde, comércio eletrônico.
- Ambientes de alta concorrência: Muitos usuários acessando os mesmos dados.
- Operações que exigem reversão controlada (ex: cancelamento de pedidos).

## ⚠️ Quando Evitar Transition
- Processos em lote (batch): Use commits parciais para evitar transações muito longas.
- Leituras simples: Sem modificação de dados.
- Casos onde inconsistências temporárias são aceitáveis (ex: estatísticas não críticas).
