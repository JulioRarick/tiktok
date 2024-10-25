Para definir o **Zsh** como shell padrão com o **Oh My Zsh** e melhorar a experiência dos usuários no seu servidor, você pode seguir estas etapas. Isso fornecerá uma interface mais atraente e produtiva para quem se conectar via **SSH**.

---

## Passo a passo

### 1. **Instalar Zsh**  
Primeiro, você precisa instalar o Zsh no servidor.

**Debian/Ubuntu:**
```bash
sudo apt update && sudo apt install zsh -y
```

**CentOS/RHEL:**
```bash
sudo yum install zsh -y
```

**Arch Linux:**
```bash
sudo pacman -S zsh --noconfirm
```

Verifique se o Zsh foi instalado corretamente:
```bash
zsh --version
```

### 2. **Instalar o Oh My Zsh**  
Instale o **Oh My Zsh**, uma estrutura que oferece temas e plugins para Zsh.

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Se preferir usar o `wget`:
```bash
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

---

### 3. **Tornar Zsh o shell padrão para os usuários**
Após a instalação, mude o shell padrão para **Zsh**:

**Para um único usuário:**
```bash
chsh -s $(which zsh)
```

**Para outro usuário específico (ex: `john`):**
```bash
sudo usermod --shell $(which zsh) john
```

**Para todos os usuários no servidor:**
1. Edite o arquivo `/etc/passwd`:
   ```bash
   sudo nano /etc/passwd
   ```
2. Substitua todas as entradas de `/bin/bash` por `/bin/zsh`.

---

### 4. **Instalar Plugins e Melhorar a Interface**

- **Habilitar Plugins no Oh My Zsh:**  
  Edite o arquivo de configuração `~/.zshrc` e adicione os plugins desejados. Exemplo:
  ```bash
  plugins=(git docker sudo ssh-agent)
  ```

- **Trocar o Tema para Agnoster ou Powerlevel10k:**
  No mesmo arquivo `~/.zshrc`, altere o tema:
  ```bash
  ZSH_THEME="agnoster"
  ```

  Ou, para instalar **Powerlevel10k**:
  ```bash
  git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
  ```
  Depois, no `~/.zshrc`:
  ```bash
  ZSH_THEME="powerlevel10k/powerlevel10k"
  ```

---

### 5. **Configurar Alias e Melhorar a Produtividade**

Adicione alias úteis no `~/.zshrc`:
```bash
alias ll='ls -lah --color=auto'
alias grep='grep --color=auto'
alias gs='git status'
alias ..='cd ..'
```

---

### 6. **Aplicar as Configurações**

Após modificar o `~/.zshrc`, aplique as configurações:
```bash
source ~/.zshrc
```

---

### 7. **Instalar Dependências para Temas e Plugins (Opcional)**

Se estiver usando temas como **Agnoster** ou **Powerlevel10k**, é recomendável instalar fontes Nerd Fonts para ícones e símbolos.

**Exemplo para Ubuntu:**
```bash
sudo apt install fonts-powerline -y
```

Para instalar manualmente outras fontes, baixe-as de:  
https://www.nerdfonts.com/

---

### 8. **Garantir que o Zsh seja usado ao conectar via SSH**

Verifique se as configurações estão corretas e que Zsh é o shell padrão:
```bash
echo $SHELL
```
Se ele retornar `/bin/zsh`, está tudo pronto!

---

Com isso, toda vez que um usuário se conectar via SSH, ele terá o Zsh com o Oh My Zsh como shell, melhorando tanto a produtividade quanto a interface do terminal.


---

Para garantir que **todos os usuários que se conectarem ao servidor via SSH** utilizem **Zsh** com **Oh My Zsh** por padrão, você precisará fazer alguns ajustes adicionais. Seguir os passos anteriores muda o shell apenas para usuários específicos. Agora, você vai configurar o **Zsh como o shell padrão global** e garantir que o **Oh My Zsh** seja carregado corretamente para novos e antigos usuários.

---

## Passos para Aplicar Zsh + Oh My Zsh Globalmente para Todos os Usuários

### 1. **Definir Zsh como Shell Padrão Globalmente**
1. Abra o arquivo `/etc/passwd` e substitua a entrada padrão de `/bin/bash` por `/bin/zsh`.  
   ```bash
   sudo sed -i 's#/bin/bash#/bin/zsh#g' /etc/passwd
   ```

2. Para garantir que novos usuários criados futuramente também tenham **Zsh** como padrão, edite o arquivo de configuração do shell padrão:

   **Debian/Ubuntu:**  
   ```bash
   sudo useradd -D -s /bin/zsh
   ```

   **RHEL/CentOS:**  
   ```bash
   sudo sed -i 's/SHELL=\/bin\/bash/SHELL=\/bin\/zsh/g' /etc/default/useradd
   ```

---

### 2. **Instalar e Configurar Oh My Zsh para Todos os Usuários**

#### Para Usuários Existentes
Execute o seguinte comando no diretório `home` de cada usuário para garantir que o **Oh My Zsh** seja configurado:

```bash
sudo -u username sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Repita para cada usuário substituindo `username` pelo nome de usuário correto.

---

#### Para Novos Usuários
Crie um **template de `.zshrc`** para garantir que qualquer novo usuário tenha Oh My Zsh configurado:

1. Copie o arquivo `.zshrc` do Oh My Zsh para `/etc/skel/`:
   ```bash
   cp /home/seu-usuario/.zshrc /etc/skel/
   ```

2. Isso garante que novos usuários terão uma cópia do `.zshrc` quando forem criados:
   ```bash
   sudo useradd -m novo_usuario -s /bin/zsh
   sudo passwd novo_usuario
   ```

---

### 3. **Personalizar o Ambiente para Todos os Usuários**

- **Criar Plugins e Aliases Globais:**  
  Você pode criar arquivos de configuração adicionais que serão carregados para todos os usuários.

  Crie um arquivo `/etc/zsh/zshrc` com aliases e plugins comuns:
  ```bash
  sudo nano /etc/zsh/zshrc
  ```

  Exemplo:
  ```bash
  alias ll='ls -lah --color=auto'
  alias ..='cd ..'
  plugins=(git sudo docker)
  ```

  Salve e feche o arquivo.

- **Aplicar Configurações de Tema Globalmente:**  
  Se quiser que todos usem o **Powerlevel10k**, você pode adicionar a seguinte linha ao `/etc/zsh/zshrc`:
  ```bash
  ZSH_THEME="powerlevel10k/powerlevel10k"
  ```

---

### 4. **Configurar SSH para Usar Zsh Corretamente**

Certifique-se de que o shell **Zsh** será carregado quando os usuários se conectarem via SSH. Verifique o arquivo de configuração do SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

- Procure pela linha `ForceCommand` e **comente-a** (caso exista). Isso evitará que o shell seja forçado para Bash:
  ```bash
  # ForceCommand /bin/bash
  ```

- Reinicie o serviço SSH:
  ```bash
  sudo systemctl restart sshd
  ```

---

### 5. **Testar a Conexão SSH com Zsh Ativado**

Agora, conecte-se ao servidor via SSH e verifique se o shell está funcionando como esperado:

```bash
ssh usuario@seu-servidor
```

Após conectar, digite:
```bash
echo $SHELL
```

Se o comando retornar `/bin/zsh`, você configurou tudo corretamente. Todos os usuários que se conectarem via SSH, antigos ou novos, agora terão Zsh com **Oh My Zsh** como shell.

---

### 6. **(Opcional) Melhorar a Aparência com Nerd Fonts**

Se estiver usando temas como **Agnoster** ou **Powerlevel10k**, é recomendável instalar **Nerd Fonts** localmente (no cliente SSH) para garantir a exibição correta dos ícones.

Baixe uma fonte Nerd Font e aplique no seu terminal:  
https://www.nerdfonts.com/

---

Com essas configurações, todos que se conectarem ao seu servidor via **SSH** terão uma experiência aprimorada com **Zsh** e **Oh My Zsh**, com plugins e temas configurados por padrão.
