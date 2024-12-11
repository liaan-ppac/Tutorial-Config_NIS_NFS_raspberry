# 📦 Instalação do Debian no Raspberry Pi

Bem-vindo ao guia de instalação do Debian no Raspberry Pi! Este documento oferece um passo a passo detalhado para você configurar seu Raspberry com o Debian de forma eficiente.

---

## 🛠️ Pré-requisitos

Antes de começar, certifique-se de ter os seguintes itens:
- Um **Raspberry Pi** (modelo compatível).
- Um cartão microSD de pelo menos **8 GB**, o ideal e **32 GB**.
- Um **computador** com acesso à internet.
- Um leitor de cartões SD (caso seu computador não tenha um embutido).
- Software para gravar a imagem no cartão SD:
  - [Raspberry Pi Imager](https://www.raspberrypi.com/software/) (recomendado).
  - Ou uma alternativa como o **balenaEtcher**.

---

## 🚀 Passo a Passo

### 1️⃣ Baixar a Imagem do Debian
1. Acesse o site oficial do Debian: [Debian para Raspberry Pi](https://raspi.debian.net/tested-images/).
2. Baixe a imagem compatível com o Raspberry Pi (geralmente em formato `.img.xz`).
![Alt text](./images-iso-rasp-debian.png "site do Debian com as issos para rapberry")
no meu caso essa foi a imagem compativel(eu esta usando um raspberry pi 4 com 8GB de RAM)

### 2️⃣ Preparar o Cartão SD
1. Insira o cartão microSD no seu computador.
2. Abra o **Raspberry Pi Imager** ou o software de sua preferência.
3. Selecione a imagem baixada do Debian:
   - No Raspberry Pi Imager, clique em **Choose OS** > **Use custom** e selecione a imagem `.img.xz`.
   ![Alt text](./imager-custom.jpg "nessa opção")
4. Escolha o cartão SD:
   - Clique em **Choose Storage** e selecione o seu cartão.
5. Clique em **Write** para iniciar a gravação.
   - Aguarde a conclusão do processo. Pode levar alguns minutos.

### 3️⃣ Configurar o Raspberry Pi
1. Insira o cartão microSD no Raspberry Pi.
2. Conecte:
   - Um monitor via HDMI.
   - Um teclado e mouse.
   - Cabo de rede (opcional, só é necessário caso o modelo não tiver Wi-Fi integrado, porem é mais facil de fazer as configurações iniciais).
3. Conecte o cabo de alimentação.
   - O Raspberry Pi deve inicializar automaticamente.

### 4️⃣ Configuração Inicial do Debian
1. Durante a primeira inicialização:
   - Escolha o idioma e fuso horário.
   - Configure o teclado
   ```bash
   apt update
   apt install keyboard-configuration console-setup
   ```
   - Crie um usuário e senha(essa etapa é importante, pos se não quando iniciar o motografico voce entra no root, não consegue visualizar configurações nem apps, para e vitar e melhor adicionar o usuaria, demorei um pouco pra enter isso 😂).
   ```bash
   adduser <username>
   ```
   - Configure a rede Wi-Fi, caso aplicável.
2. Atualize o sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
    ```
3. Instale o Desktop enviroment e a interface grafica
    - use o comando tasksel e instale no Gnome de preferência
    ```bash
        sudo tasksel
    ```
    ele ja deve vir intalado por padrão no debian, mas caso contrario instale com o apt
    ![Alt text](./tasksell-debian-conf.png "tela do tasksel")
    aqui devemos selecionar as opções debian desktop enviroment, GNOME e SSH sever
    voce pode usar o espaço ou tab para selecionar, e mover com as setas, dependendo da configuração, quando terminar selecione ok e enter, ps: vc so conseguirar usar o teclado

4. Ultimos Passos
    - Nesse tudo deve ter corrido bem e a interfaçe grafica deve estar funcionando, configure o resto pelo settings na interface grafica
    - Desativar o modo Hibernar: caso você queira usar o raspberi como servidor, ou simplesmente não queira que ele desliguue você pode simplesmente desativar o modo hibernar, caso contrario você tera problemas, e como o raspberry as vezees não tem o botão power exposto, a alternativa seria ligar e deligar ele na força
    ```bash
    sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
    ``` 







qualque duvida eu me inspirei no seguinte tutorial [How to Install Debian 12 Bookworm on Raspberry Pi (2024 update)](https://raspberrytips.com/install-debian-on-raspberry-pi/).

dicas para desabilitar hibernação [Desabilite as opções de suspensão e hibernação no Debian](https://elias.praciano.com/2016/09/desabilite-as-opcoes-de-suspensao-e-hibernacao-no-debian/#:~:text=Se%20voc%C3%AA%20quiser%20impedir%20que,systemd%20para%20desabilitar%20a%20fun%C3%A7%C3%A3o.&text=ou%20reinicie%20a%20m%C3%A1quina.)