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