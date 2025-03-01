# 🛠️ Triggers no Banco de Dados

## 📖 O que é uma Trigger?

Uma trigger no banco de dados é um mecanismo que executa automaticamente uma ação definida quando eventos específicos ocorrem em uma tabela. Esses eventos podem ser inserção (INSERT), atualização (UPDATE) ou exclusão (DELETE).

Triggers são úteis para automatizar regras de negócio, auditar alterações e garantir a integridade dos dados diretamente no banco de dados.

## 🎯 Características Principais de uma Trigger
- **Automação**: Executa ações automaticamente em resposta a eventos.
- **Monitoramento de Dados**: Pode ser usada para registrar alterações nos dados ou auditar operações.
- **Escopo Amplo**: Pode ser aplicada a eventos de diferentes tipos (antes ou depois das operações).
- **Regras de Negócio**: Útil para implementar validações e consistências que não podem ser feitas apenas com constraints.
- **Execução no Banco**: Ocorre diretamente no banco de dados, sem necessidade de intervenção do lado da aplicação.

## 📝 Exemplo: Criar uma Trigger para Auditoria de Alterações
### Criando a Tabela de Auditoria
  ```sql
    CREATE TABLE LogAlteracoes (
      id INT AUTO_INCREMENT PRIMARY KEY,
      tabela VARCHAR(50),
      operacao VARCHAR(10),
      usuario VARCHAR(50),
      data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
      dados_antigos JSON,
      dados_novos JSON
    );
```

### Criando a Trigger
  ```sql
    DELIMITER $$ -- Alterar temporariamente o delimitador, permite o uso de ';' dentro da Trigger.
      CREATE TRIGGER trigger_auditoria -- Cria o Trigger
      AFTER INSERT OR UPDATE OR DELETE ON tabela_alvo -- Gatilho após inserção.
      FOR EACH ROW
      BEGIN -- Inicia o bloco de instruções.
        DECLARE dados_antigos JSON;
        DECLARE dados_novos JSON;

        IF (NEW.id IS NOT NULL) THEN  -- Registro operação seja INSERT.
          SET dados_novos = JSON_OBJECT('id', NEW.id, 'nome', NEW.nome); -- Ajuste conforme os campos da tabela.
        END IF;

        IF (OLD.id IS NOT NULL) THEN  -- Caso operação seja UPDATE.
          SET dados_antigos = JSON_OBJECT('id', OLD.id, 'nome', OLD.nome); -- Ajuste conforme os campos da tabela.
        END IF;

        -- A operação (INSERT, UPDATE ou DELETE) é determinada com base nos valores de OLD.id e NEW.id.
        IF (IFNULL(OLD.id, NEW.id) IS NOT NULL) THEN 
          INSERT INTO LogAlteracoes (tabela, operacao, usuario, dados_antigos, dados_novos)
          VALUES ('tabela_alvo', 
                  CASE
                      WHEN OLD.id IS NULL THEN 'INSERT' -- Se OLD.id for nulo, é um INSERT (novo registro).
                      WHEN NEW.id IS NULL THEN 'DELETE' -- Se NEW.id for nulo, é um DELETE (registro removido).
                      ELSE 'UPDATE'                     -- Se ambos os id's não forem nulos, é um UPDATE (alteração).
                  END,
                  USER(), 
                  dados_antigos, 
                  dados_novos);
        END IF;
      END $$ -- Finaliza o bloco de instruções com o novo Delimiter que foi definido '$$'.
    DELIMITER ; -- Retorna para o delimitador padrão ';'.
  ```

### Explicação
- **Tabela de Auditoria**: Armazena informações sobre alterações, como o tipo de operação e os dados afetados.
- **Função Trigger**: Define a lógica executada para registrar as alterações em formato JSON.
- **Trigger**: Vincula a função aos eventos de INSERT, UPDATE ou DELETE na tabela-alvo.

## 🚀 Comandos para Gerenciar Triggers
### Criar uma Trigger
  ```sql
    CREATE TRIGGER NomeDaTrigger
    AFTER [INSERT | UPDATE | DELETE]
    ON NomeDaTabela
    FOR EACH ROW
    EXECUTE FUNCTION NomeDaFuncao();
  ```

### Remover uma Trigger
  ```sql
    DROP TRIGGER NomeDaTrigger ON NomeDaTabela;
  ```

### Consultar Triggers Existentes
  ```sql
    SELECT * FROM information_schema.triggers;
  ```

## 🔥 Vantagens do Uso de Triggers
1. **Automatização**: Elimina a necessidade de aplicar regras manualmente.
2. **Centralização da Lógica**: Regras de negócios ficam no banco, não na aplicação.
3. **Auditoria**: Permite monitorar alterações nos dados de forma automática.
4. **Consistência**: Garante integridade dos dados em diferentes operações.

## 🚫 Limitações das Triggers
- **Desempenho**: Pode impactar negativamente o desempenho em tabelas com alto volume de operações.
- **Complexidade**: Manutenção de triggers pode ser difícil em sistemas grandes.
- **Dependência do Banco**: Lógica nas triggers não é portátil entre diferentes SGBDs.

## 📌 Quando Usar Triggers?
- Para realizar auditorias de alterações em tabelas.
- Para validar ou transformar dados antes de serem salvos.
- Para sincronizar tabelas ou registros entre diferentes partes do banco de dados.
- Para executar cálculos ou ações automaticamente após eventos.

## 🔄 Alternativas a Triggers
- **Stored Procedures** Para encapsular a lógica em chamadas explícitas.
- **Regras na Aplicação**: Para implementar validações diretamente no código.
- **Jobs Agendados**: Para tarefas que podem ser realizadas periodicamente, sem depender de eventos em tempo real.
