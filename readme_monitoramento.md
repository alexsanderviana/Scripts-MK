Configuração no MikroTik
Acesse o RB4011 via WinBox/SSH/WebFig

Crie o Script:

Vá em System > Scripts

Clique em Add New

Nome: monitoramento-links-telegram

Copie e cole o script completo

Substitua SEU_TOKEN_AQUI e SEU_CHAT_ID_AQUI

Ajuste as interfaces conforme sua configuração

Clique em OK

Configure o Scheduler:

Vá em System > Scheduler

Clique em Add New

Nome: executa-monitoramento

Start Time: startup (inicia quando o roteador ligar)

Interval: 00:01:00 (executa a cada 1 minuto)

On Event: /system script run monitoramento-links-telegram

Clique em OK

PASSO 5: Teste
Teste manualmente:

text
/system script run monitoramento-links-telegram
Verifique os logs:

text
/log print where message~"monitoramento"
Simule uma queda:

Desconecte o cabo da WAN principal por alguns segundos

Verifique se recebe o alerta no Telegram

PASSO 6: Ajustes e Otimizações
Para monitorar mais interfaces:
Adicione no final do script:

bash
# Monitorar interface adicional
:local statusOutraInterface [$verificarLink "ether3" "Outra Interface"]
# Adicione a lógica de verificação similar às anteriores
Para alterar o IP de teste:
Substitua 8.8.8.8 por:

1.1.1.1 (Cloudflare)

208.67.222.222 (OpenDNS)

Ou IP de algum servidor confiável da sua rede

Para ajustar frequência de alertas:
Modifique o intervalo no Scheduler:

00:00:30 = 30 segundos

00:05:00 = 5 minutos

Manutenção
Verifique logs regularmente: /log print where topics~"script"

Teste o bot: Envie /start para o bot no Telegram

Atualize o token se necessário regenerar no BotFather

Troubleshooting
Problema: Não recebe mensagens no Telegram

Verifique se o token e chat ID estão corretos

Verifique se o roteador tem acesso à internet (API Telegram)

Teste manualmente: /system script run monitoramento-links-telegram

Problema: Script não executa

Verifique se o scheduler está ativo

Verifique permissões do script

Analise os logs de erro

Passo a Passo Geral de Configuração
1. Pré-requisitos
Tenha em mãos o Token do seu Bot do Telegram e o seu Chat ID (conforme explicado na resposta anterior).

Acesso administrativo ao RB4011 via WinBox ou SSH.

2. Inserindo os Scripts no MikroTik
Acesse o menu System > Scripts.

Clique em Add New.

No campo Name, dê um nome sugestivo, como monitor-saude, qos-automatico, ou resumo-clientes.

No campo Source, cole o código do script desejado.

Atenção: Lembre-se de substituir SEU_TOKEN_AQUI e SEU_CHAT_ID_AQUI pelas suas informações em cada um dos scripts.

Ajuste as variáveis de configuração (limites de temperatura, banda, interfaces, etc.) conforme sua realidade.

Clique em OK para salvar.

3. Agendando a Execução (Scheduler)
Para que os scripts rodem automaticamente, você precisa criar tarefas agendadas.

Acesse o menu System > Scheduler.

Clique em Add New para cada script que deseja automatizar.

Configure:

Monitor de Saúde (Script 1):

Name: exec-saude

Interval: 00:05:00 (a cada 5 minutos) 

On Event: /system script run monitor-saude

QoS Automático (Script 2):

Name: exec-qos

Interval: 00:01:00 (a cada 1 minuto para resposta rápida)

On Event: /system script run qos-automatico

Resumo de Clientes (Script 3):

Name: exec-resumo

Start Time: 08:00:00 (para enviar um resumo diário, por exemplo)

Interval: 24:00:00 (se quiser a cada 24h) ou 01:00:00 (para um resumo a cada hora).

On Event: /system script run resumo-clientes