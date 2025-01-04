# Aprendendo Sobre Cron para Automação de Execução de Tarefas em Sistemas Unix

O Cron é uma ferramenta poderosa no Linux usada para agendar tarefas para execução periódica. É útil para automatizar backups, execução de scripts e outras tarefas recorrentes.

---

## 1. **Introdução ao Cron**

- **Cron**: Um daemon que executa comandos em horários e intervalos específicos.
- **CronTab**: Arquivo ou utilitário que armazena a tabela de agendamentos para o cron.
- **CronJob**: Um único comando ou script agendado para execução pelo cron.

---

### 2. **Estrutura de um CronJob**

Cada linha em um arquivo `crontab` segue esta estrutura:

```bash
* * * * * comando
- - - - -
| | | | | 
| | | | +---- Dia da semana (0 - 7, onde 0 e 7 representam domingo)
| | | +------ Mês (1 - 12)
| | +-------- Dia do mês (1 - 31)
| +---------- Hora (0 - 23)
+------------ Minuto (0 - 59)
```

- **`*`**: Significa "qualquer valor".
- **Exemplo**: `0 5 * * 1 comando` executará o comando às 05:00 todas as segundas-feiras.

---

### 3. **Comandos Básicos para Trabalhar com Cron**

1. **Listar os CronJobs do Usuário Atual**:

   ```bash
   crontab -l
   ```

2. **Editar CronJobs**:

   ```bash
   crontab -e
   ```

   Abre o editor para modificar os cron jobs.

3. **Remover Todos os CronJobs**:

   ```bash
   crontab -r
   ```

4. **Exibir CronJobs de Outro Usuário (como root)**:

   ```bash
   sudo crontab -u username -l
   ```

---

### 4. **Exemplos de CronJobs**

#### 4.1. Agendar um Script para Rodar Diariamente às 2h da Manhã

```bash
0 2 * * * /path/to/script.sh
```

#### 4.2. Limpar Logs a Cada Hora

```bash
0 * * * * rm -f /var/log/*.log
```

#### 4.3. Executar um Comando a Cada 5 Minutos

```bash
*/5 * * * * /usr/bin/python3 /path/to/script.py
```

#### 4.4. Backup Todo Dia 1º de Cada Mês às 3h da Manhã

```bash
0 3 1 * * tar -czf /backup/home-$(date +\%Y-\%m-\%d).tar.gz /home
```

#### 4.5. Rodar Somente em Dias Úteis (Segunda a Sexta)

```bash
30 6 * * 1-5 /path/to/weekday_task.sh
```

#### 4.6. Obtendo a data e salvando em um arquivo

```bash
* * * * * echo $(date) >> /home/mauriciobenjamin700/projects/my/cron-learning/cron_log.txt
*/2 * * * * echo $(date) >> /home/mauriciobenjamin700/projects/my/cron-learning/cron_log2.txt
```

---

### 5. **Agendamento com Arquivos Crontab**

Para configurar cron jobs globais (válidos para todos os usuários), edite o arquivo:

```bash
sudo nano /etc/crontab
```

A sintaxe neste arquivo inclui um campo extra para especificar o usuário:

```bash
* * * * * username comando
```

---

### 6. **Usando Logs para Verificar Tarefas Cron**

Por padrão, o Cron envia a saída de erros para o e-mail do usuário, mas você pode redirecionar explicitamente para um arquivo de log:

```bash
0 2 * * * /path/to/script.sh >> /var/log/myscript.log 2>&1
```

- `>>`: Acrescenta a saída ao arquivo.
- `2>&1`: Redireciona erros para o mesmo local que a saída padrão.

---

### 7. **Diretórios Especiais para Cron**

O cron possui diretórios específicos para agendar tarefas com diferentes frequências, localizados em `/etc/`:

- **`/etc/cron.hourly`**: Scripts executados a cada hora.
- **`/etc/cron.daily`**: Scripts executados diariamente.
- **`/etc/cron.weekly`**: Scripts executados semanalmente.
- **`/etc/cron.monthly`**: Scripts executados mensalmente.

Para adicionar um script, basta copiá-lo para o diretório correspondente. Certifique-se de que ele é executável:

```bash
chmod +x /etc/cron.daily/meu_script.sh
```

---

### 8. **Testando CronJobs**

Para testar um CronJob sem esperar pela execução, use o comando `run-parts`:

```bash
run-parts /etc/cron.daily
```

---

### 9. **Desativar ou Pausar Cron Temporariamente**

1. **Parar o Serviço Cron**:

   ```bash
   sudo systemctl stop cron
   ```

2. **Reiniciar o Serviço Cron**:

   ```bash
   sudo systemctl start cron
   ```

3. **Desabilitar Cron na Inicialização**:

   ```bash
   sudo systemctl disable cron
   ```

---

### 10. **Dicas para Uso de Cron**

- **Certifique-se de usar caminhos absolutos**: Scripts ou comandos no cron não possuem o mesmo ambiente de variáveis que sua sessão interativa.
- **Inclua variáveis de ambiente necessárias no script**:

  ```bash
  PATH=/usr/bin:/bin
  ```

- **Teste scripts manualmente antes de agendar no cron** para evitar falhas.

---

### 11. **Depuração de Cron**

- Verifique os logs do cron:

  ```bash
  cat /var/log/syslog | grep cron
  ```

---

### Conclusão

O cron é uma ferramenta simples, mas extremamente eficaz para automação. Com uma boa compreensão de sua sintaxe e funcionalidade, você pode simplificar muitas tarefas repetitivas no Linux. Experimente diferentes configurações e combine-as com scripts para maximizar sua produtividade!

## Referências

[Cron, crontab e cronjob: agendando tarefas automáticas](https://www.youtube.com/watch?v=TG--rQkZvGc)
