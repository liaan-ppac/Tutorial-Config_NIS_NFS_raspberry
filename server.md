
# Configuração de Servidor NIS e NFS no Linux

Este tutorial detalha os passos para configurar um servidor NIS e NFS utilizando um script shell, além de fornecer dicas e comandos adicionais para verificar e solucionar problemas comuns.

---

## Configuração Utilizando o Script `server_packages.sh`

Os comandos abaixo estão encapsulados no arquivo de script `server_packages.sh`. Você pode executar este script diretamente para configurar os serviços.

### Executando o Script

1. execute o arquivo `server_packages.sh` com os seguintes comandos:
   ```bash
   source server_packages.sh
   ```
   ou 
   ```bash
   chmod +x server_packages.sh

   server_packages.sh
   ```

2. Certifique-se de revisar e ajustar os seguintes arquivos antes da execução do script:

   - **hosts**: Adicione o IP do servidor seguido pelo seu apelido. Exemplo:
     ```
     192.168.68.110 labian-6.liian labian-6
     ```

   - **defaultdomain**: Insira o domínio da sua rede local. Exemplo:
     ```
     liian
     ```

   - **idmapd.conf**: Substitua o nome do domínio pelo utilizado na sua rede. Exemplo:
     ```
     Domain = liian
     ```

   - **exports**: Defina as pastas que serão compartilhadas pelo NFS. Exemplo:
     ```
     /export/dir1 *(rw,sync,no_subtree_check)
     /export/dir2 *(rw,sync,no_subtree_check)
     ```

---

## Verificando e Corrigindo Problemas no NFS

1. Verifique o status do serviço NFS:
   ```bash
   sudo systemctl status nfs-common
   ```

2. Caso o serviço esteja inativo ou mascarado, execute os seguintes comandos:
   ```bash
   sudo rm /lib/systemd/system/nfs-common.service
   systemctl daemon-reload
   systemctl status nfs-common
   ```

3. Caso o serviço continue inativo:
   ```bash
   systemctl start nfs-common
   systemctl status nfs-common
   systemctl enable nfs-common
   systemctl status nfs-common
   ```

---

## Verificando os Serviços do NIS

1. Verifique o status dos serviços relacionados ao NIS:
   ```bash
   systemctl status rpcbind nscd ypbind
   ```

2. Teste a conectividade com o servidor usando `ypwhich`:
   ```bash
   ypwhich labian-6.liian
   ```

3. Caso o teste falhe, verifique as configurações dos arquivos `yp.conf` e `hosts`:
   ```bash
   nano /etc/yp.conf
   nano /etc/hosts
   ```

---

## Validando as Tabelas de Usuários

1. Verifique se as tabelas de usuários estão sendo retornadas corretamente:
   ```bash
   ypcat -h labian-6.liian passwd
   ```

---

## Adicionando usuários

1. Para adicionar novos usuários junto com o comando server_packages.sh deve ter uma fução de adicionar usuario customizado execute o useradd.sh pasando o parametro, nome do usuário e seu id, os id começam de 1010 devem ser inseridos manualmente e sempre incrementados ex 1010 1011 1012 .., os usuários criados NÃO SÃO SUPER USUÁRIOS!
   ```bash
   source useradd.sh 1010 arthur
   ```
   ou
   ```bash
   chmod +x useradd.sh
   useradd.sh 1010 arthur
   ```
   

---

## Observações Finais

- Certifique-se de que os serviços NIS e NFS estão ativos e configurados corretamente.
- Os arquivos copiados para `/etc/` devem ser ajustados às necessidades do ambiente local.
- Utilize logs dos serviços (`journalctl -u <nome_serviço>`) para diagnósticos detalhados em caso de falhas.
- Caso queira montar pastas em dispositivos externos com hd externo monte eles no etc/fstab automaticamente
- o Super usuário do sistema e o sudo criado na hora da formatação, nesse caso é o nome do laboratório.