# Configuração do Cliente para Servidor NIS e NFS

Este tutorial explica como configurar o cliente para um servidor NIS e NFS, utilizando um arquivo shell script (`client_packages.sh`) e ajustes adicionais. Ele também fornece passos para verificar e garantir o funcionamento adequado dos serviços, além de criar atalhos para as pastas montadas.

---

## Pré-requisitos

1. Acesso ao servidor configurado para NIS e NFS.
2. Permissões administrativas no cliente.
3. O arquivo `client_packages.sh` com os comandos necessários.

---

## Passos de Configuração

### 1. Executando o Script de Configuração

Execute o script para configurar os serviços necessários:

```bash
source client_packages.sh
```

Isso irá executar os seguintes passos:

- **Atualizar o arquivo `/etc/hosts`:**
  Adicione o IP do servidor seguido pelo apelido, como no exemplo:
  ```
  192.168.68.110 labian-6.liian
  ```

- **Configurar o NIS:**
  - Instalar o pacote NIS.
  - Copiar os arquivos de configuração necessários (`yp.conf`, `nsswitch.conf`, `defaultdomain`, `common-session`).
  - Reiniciar e habilitar os serviços NIS:
    ```bash
    systemctl restart rpcbind nscd ypbind
    systemctl enable rpcbind ypbind
    ```

- **Configurar o NFS:**
  - Instalar o pacote NFS.
  - Copiar o arquivo de configuração `idmapd.conf`.
  - Criar a pasta de montagem e adicionar uma entrada no arquivo `/etc/fstab` para montagem automática:
    ```bash
    mkdir /storage
    echo "labian-6.liian:/storage	/storage	nfs	defaults	0	0" >> /etc/fstab
    ```

---

### 2. Verificando o Serviço NFS

1. Verifique se o serviço NFS está ativo:

   ```bash
   sudo systemctl status nfs-common
   ```

2. Caso esteja inativo ou mascarado:

   ```bash
   sudo rm /lib/systemd/system/nfs-common.service
   systemctl daemon-reload
   systemctl status nfs-common
   ```

3. Caso esteja apenas inativo:

   ```bash
   systemctl start nfs-common
   systemctl enable nfs-common
   systemctl status nfs-common
   ```

---

### 3. Verificando os Serviços do NIS

1. Confira o status dos serviços do NIS:

   ```bash
   systemctl status rpcbind nscd ypbind
   ```

2. Teste a conexão com o servidor:

   ```bash
   ypwhich labian-6.liian
   ```

3. Caso o comando não funcione, verifique os arquivos:

   - **`/etc/yp.conf`:**
     ```bash
     nano /etc/yp.conf
     ```

   - **`/etc/hosts`:**
     ```bash
     nano /etc/hosts
     ```

     Certifique-se de que o apelido e o IP do servidor estejam corretos:
     ```
     192.168.68.110 labian-6.liian labian-6
     ```

4. Teste novamente a conexão:

   ```bash
   ypwhich labian-6.liian
   ```

5. Verifique as tabelas de usuários do servidor:

   ```bash
   ypcat -h labian-6.liian passwd
   ```

---

### 4. Montando a Pasta de Armazenamento

1. Crie a pasta de armazenamento, se ainda não tiver sido criada:

   ```bash
   mkdir /storage
   ```

2. Verifique o arquivo `/etc/fstab` para montagem automática:

   ```bash
   nano /etc/fstab
   ```

   Deve conter uma linha como esta:
   ```
   labian-6.liian:/storage	/storage	nfs	defaults	0	0
   ```

3. Para montar manualmente:

   ```bash
   mount labian-6.liian:/storage /storage
   ```

4. Reinicie os serviços para garantir o funcionamento:

   ```bash
   systemctl daemon-reload
   ```

5. Navegue até a pasta montada para verificar:

   ```bash
   cd /storage
   ```

---

### 5. Criando um Atalho para a Pasta Montada

Para facilitar o acesso à pasta montada, crie um atalho no desktop. Siga o exemplo abaixo para criar o arquivo `AtalhoStorage.desktop`:

```ini
[Desktop Entry]
Name=Storage
Exec=xdg-open /storage
Type=Application
Icon=folder
Terminal=false
```

Salve o arquivo na área de trabalho com o nome `AtalhoStorage.desktop` e certifique-se de que ele tem permissões de execução:

```bash
chmod +x ~/Desktop/AtalhoStorage.desktop
```

Agora você pode acessar a pasta montada diretamente pelo atalho.


