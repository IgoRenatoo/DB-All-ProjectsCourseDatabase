# 🛠️ Backup em Banco de Dados

O backup é essencial para garantir a recuperação de dados em caso de falhas, erros humanos ou desastres. No SQL Server, existem vários tipos de backups:

## Tipos de Backup

### Backup Completo (Full Backup)

- Cria uma cópia completa do banco de dados em um ponto específico no tempo.
- É a base para todos os outros tipos de backup.

Exemplo de comando:

```sql
BACKUP DATABASE [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_Full.bak';
```

### Backup Diferencial (Differential Backup)

- Armazena apenas as alterações feitas desde o último backup completo.
- Menor e mais rápido que um backup completo.

Exemplo:

```sql
BACKUP DATABASE [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_Diff.bak' WITH DIFFERENTIAL;
```

### Backup de Log de Transações (Transaction Log Backup)

- Captura todas as transações registradas no log desde o último backup de log.
- Permite recuperação pontual (Point-in-Time Recovery).

Exemplo:

```sql
BACKUP LOG [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_Log.trn';
```

### Backup de Arquivos/Grupos de Arquivos

- Faz backup de arquivos ou grupos de arquivos específicos (útil para bancos grandes).

## Log de Transações (Transaction Log)

O log de transações registra todas as operações de modificação no banco de dados (INSERT, UPDATE, DELETE, DDL). Ele é crítico para:  
- Recuperação de desastres.  
- Garantir a consistência do banco.  
- Permitir rollback/rollforward de transações.

### Funcionamento do Log

- **LSN (Log Sequence Number)**: Cada entrada no log tem um número sequencial único.  
- **Truncamento do Log**: Espaço do log é liberado após um backup de log (se o banco estiver no modo **Full** ou **Bulk-Logged**).  
- **VLFs (Virtual Log Files)**: O log é dividido em VLFs para gerenciamento interno.

### Modelos de Recuperação

#### Simple Recovery Model
- Logs de transações são truncados automaticamente.  
- Não permite backup de logs ou recuperação pontual.  

#### Full Recovery Model
- Todos os logs são mantidos até serem copiados em um backup de log.  
- Permite recuperação pontual.  

#### Bulk-Logged Recovery Model
- Similar ao Full, mas operações em massa (ex: BULK INSERT) são minimamente registradas.  

## Estratégia de Backup Recomendada

- **Backup Completo Semanal**: Base para recuperação.  
- **Backup Diferencial Diário**: Reduz o tempo de restauração.  
- **Backup de Log de Transações a cada 15-30 minutos**: Permite recuperação precisa.  
- **Backup de Cauda (Tail-Log Backup)**: Útil antes de uma restauração em caso de falha.  

```sql
BACKUP LOG [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_TailLog.trn' WITH NORECOVERY;
```

## Processo de Restauração

1. Restaure o último backup completo:  
```sql
RESTORE DATABASE [NomeDoBanco] FROM DISK = 'C:\Backup\NomeDoBanco_Full.bak' WITH NORECOVERY;
```

2. Restaure o backup diferencial mais recente (se houver):  
```sql
RESTORE DATABASE [NomeDoBanco] FROM DISK = 'C:\Backup\NomeDoBanco_Diff.bak' WITH NORECOVERY;
```

3. Restaure todos os logs de transações na ordem sequencial:  
```sql
RESTORE LOG [NomeDoBanco] FROM DISK = 'C:\Backup\NomeDoBanco_Log.trn' WITH NORECOVERY;
```

4. Finalize com:  
```sql
RESTORE DATABASE [NomeDoBanco] WITH RECOVERY;
```

### Recuperação Pontual (Point-in-Time Recovery)
```sql
RESTORE LOG [NomeDoBanco] FROM DISK = 'C:\Backup\NomeDoBanco_Log.trn'
WITH STOPAT = '2023-10-30 14:00:00', NORECOVERY;
```

## Melhores Práticas

- **Teste Restaurações**: Valide periodicamente os backups.  
- **Armazene Backups Offsite**: Use locais físicos ou cloud (ex: Azure Blob Storage).  
- **Monitore o Tamanho do Log**: Evite crescimento excessivo com backups de log frequentes.  
- **Automatize Backups**: Use o SQL Server Agent ou ferramentas como Ola Hallengren's scripts.  
- **Use CHECKSUM**: Para verificar integridade dos backups.  

```sql
BACKUP DATABASE [NomeDoBanco] TO DISK = '...' WITH CHECKSUM;
```

## Exemplo de Script de Backup Automatizado (SQL Server Agent Job)
```sql
-- Backup Completo Diário
BACKUP DATABASE [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_Full_$(ESCAPE_SQUOTE(DATE)).bak'
WITH COMPRESSION, CHECKSUM;

-- Backup de Log a cada 15 minutos
BACKUP LOG [NomeDoBanco] TO DISK = 'C:\Backup\NomeDoBanco_Log_$(ESCAPE_SQUOTE(TIME)).trn'
WITH COMPRESSION, CHECKSUM;
```

## Ferramentas Úteis

- **SQL Server Management Studio (SSMS)**: Interface gráfica para gerenciar backups.  
- **PowerShell**: Automatize backups com scripts.  
- **Third-Party Tools**: Redgate SQL Backup, ApexSQL Backup.  

Se precisar de detalhes específicos ou exemplos adicionais, avise! 😊
