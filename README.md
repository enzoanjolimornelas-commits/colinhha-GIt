# 🖥️ Terminal & Git — Guia Completo

> Referência rápida de comandos essenciais do terminal e do Git, do básico ao avançado.

![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![Terminal](https://img.shields.io/badge/Terminal-000000?style=for-the-badge&logo=gnubash&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

---

## 📋 Índice

- [Terminal](#-comandos-do-terminal)
- [Configuração do Git](#-configuração-do-git)
- [Comandos Básicos](#-comandos-básicos)
- [Branches](#-branches)
- [Repositório Remoto](#-repositório-remoto-push-pull-fetch)
- [Merge & Rebase](#-merge--rebase)
- [Stash](#-stash)
- [Log & Diff](#-log--diff)
- [Avançado](#-avançado)

---

## ⬛ Comandos do Terminal

<details>
<summary><strong>Navegação</strong></summary>

```bash
pwd               # Exibe o diretório atual (Print Working Directory)
ls -la            # Lista arquivos: -l formato longo, -a inclui ocultos
cd pasta/         # Entra na pasta
cd ..             # Volta um nível
cd ~              # Vai para o diretório home
clear             # Limpa o terminal (atalho: Ctrl + L)
```

</details>

<details>
<summary><strong>Arquivos & Pastas</strong></summary>

```bash
mkdir nome-da-pasta          # Cria uma nova pasta
touch arquivo.txt            # Cria um arquivo vazio
cp origem destino            # Copia arquivo
cp -r pasta/ destino/        # Copia pasta inteira (recursivo)
mv origem destino            # Move ou renomeia arquivo/pasta
rm arquivo.txt               # Remove arquivo
rm -rf pasta/                # Remove pasta recursivamente ⚠️ irreversível!
```

</details>

<details>
<summary><strong>Texto & Busca</strong></summary>

```bash
cat arquivo.txt                  # Exibe o conteúdo de um arquivo
grep -r "texto" .                # Busca texto recursivamente em arquivos
grep -ri "texto" .               # Busca ignorando maiúsculas/minúsculas
echo "texto" > arquivo.txt       # Escreve no arquivo (substitui)
echo "texto" >> arquivo.txt      # Adiciona ao final do arquivo
```

</details>

<details>
<summary><strong>Processos & Sistema</strong></summary>

```bash
ps aux              # Lista todos os processos em execução
kill PID            # Encerra processo pelo ID
kill -9 PID         # Força encerramento do processo
chmod +x script.sh  # Torna um arquivo executável
which git           # Mostra o caminho do executável de um comando
```

</details>

---

## ⚙️ Configuração do Git

> A flag `--global` aplica a configuração a todos os repositórios do sistema.  
> Sem ela, vale apenas para o repositório atual.

<details>
<summary><strong>Identidade (user.name, user.email)</strong></summary>

```bash
# Definir nome e e-mail do autor (aparecem em todos os commits)
git config --global user.name "Seu Nome"
git config --global user.email "email@exemplo.com"

# Verificar configurações ativas
git config --list

# Ver um valor específico
git config user.name
```

</details>

<details>
<summary><strong>Editor & Preferências</strong></summary>

```bash
# Definir VS Code como editor padrão
git config --global core.editor "code --wait"

# Definir 'main' como branch padrão em novos repositórios
git config --global init.defaultBranch main

# Normalizar quebras de linha (input = macOS/Linux | true = Windows)
git config --global core.autocrlf input

# Habilitar cores no terminal
git config --global color.ui auto
```

</details>

<details>
<summary><strong>SSH & Credenciais</strong></summary>

```bash
# Gerar par de chaves SSH (copie a .pub para GitHub → Settings → SSH Keys)
ssh-keygen -t ed25519 -C "email@exemplo.com"

# Testar conexão SSH com o GitHub
ssh -T git@github.com

# Salvar credenciais HTTPS em disco
git config --global credential.helper store

# Alternativa: cache temporário (15 min padrão)
git config --global credential.helper cache
```

</details>

---

## ◈ Comandos Básicos

### Os 3 estados do Git

```
Working Directory  ──git add──▶  Staging Area  ──git commit──▶  Repository
```

| Estado | Descrição |
|--------|-----------|
| **Working Directory** | Arquivos modificados localmente |
| **Staging Area** | Arquivos preparados para o próximo commit |
| **Repository** | Histórico permanente de snapshots |

<details>
<summary><strong>Inicializar</strong></summary>

```bash
git init                          # Cria repositório Git no diretório atual
git clone https://github.com/user/repo.git   # Clona repositório remoto
git clone https://github.com/user/repo.git minha-pasta  # Clona com nome customizado
```

</details>

<details>
<summary><strong>Status & Staging</strong></summary>

```bash
git status                        # Mostra arquivos modificados, staged e untracked
git add arquivo.txt               # Adiciona arquivo específico ao stage
git add .                         # Adiciona todos os arquivos modificados
git add -p                        # Adiciona interativamente (escolhe partes do arquivo)
git restore --staged arquivo.txt  # Remove arquivo do stage sem descartar alterações
```

</details>

<details>
<summary><strong>Commit</strong></summary>

```bash
git commit -m "mensagem do commit"        # Cria um commit com mensagem
git commit -am "mensagem"                 # Adiciona e commita arquivos já rastreados
git commit --amend -m "nova mensagem"     # Corrige a mensagem do último commit
git commit --amend --no-edit              # Adiciona arquivos ao último commit sem mudar mensagem
```

> ⚠️ Evite usar `--amend` em commits já enviados ao repositório remoto.

</details>

<details>
<summary><strong>Desfazer Alterações</strong></summary>

```bash
git restore arquivo.txt           # Descarta alterações não staged (⚠️ irreversível)
git restore --staged arquivo.txt  # Retira do stage sem descartar alterações
git revert abc1234                # Cria commit que desfaz o commit indicado (seguro)
git reset --soft HEAD~1           # Remove último commit, mantém alterações no stage
git reset --mixed HEAD~1          # Remove último commit, mantém alterações no working dir
git reset --hard HEAD~1           # Remove último commit E as alterações ⚠️ destrutivo!
```

</details>

---

## ⑃ Branches

> Uma branch é um ponteiro leve para um commit. Isola funcionalidades e permite desenvolvimento paralelo sem conflitos.

<details>
<summary><strong>Criar & Navegar</strong></summary>

```bash
git branch nome-da-branch         # Cria uma nova branch (sem mudar para ela)
git switch nome-da-branch         # Muda para a branch indicada
git switch -c nova-branch         # Cria e já muda para a nova branch
git checkout -b nova-branch       # Equivalente clássico ao comando acima
```

</details>

<details>
<summary><strong>Listar, Renomear & Deletar</strong></summary>

```bash
git branch                        # Lista branches locais (* = branch atual)
git branch -a                     # Lista branches locais e remotas
git branch -m novo-nome           # Renomeia a branch atual
git branch -d nome-da-branch      # Deleta branch local (só se já foi mergeada)
git branch -D nome-da-branch      # Força deleção da branch local
git push origin --delete branch   # Deleta branch no repositório remoto
```

</details>

---

## ☁️ Repositório Remoto (push, pull, fetch)

### Fetch vs Pull

| Comando | O que faz |
|---------|-----------|
| `git fetch` | Baixa mudanças do remoto, **mas não integra** ao código local |
| `git pull` | `git fetch` + `git merge` — baixa e integra automaticamente |

> Use `fetch` para inspecionar mudanças antes de integrar. Use `pull` para atualizar diretamente.

<details>
<summary><strong>Gerenciar Remotos</strong></summary>

```bash
git remote -v                                          # Lista remotos com seus URLs
git remote add origin https://github.com/user/repo.git # Conecta ao remoto 'origin'
git remote set-url origin novo-url                     # Altera URL de um remoto
git remote remove origin                               # Remove um remoto
```

</details>

<details>
<summary><strong>Fetch & Pull</strong></summary>

```bash
git fetch origin                  # Baixa mudanças do remoto sem integrar
git fetch --all --prune           # Busca todos os remotos e remove branches deletadas
git pull origin main              # Busca e integra a branch remota 'main'
git pull --rebase                 # Puxa mudanças usando rebase (histórico mais limpo)
```

</details>

<details>
<summary><strong>Push</strong></summary>

```bash
git push origin main              # Envia commits da branch local 'main' para o remoto
git push -u origin feature/login  # Define upstream: próximos git push dispensam argumentos
git push --force-with-lease       # Force push seguro (falha se alguém enviou commits antes)
git push origin --delete branch   # Deleta branch no remoto
git push origin --tags            # Envia todas as tags para o remoto
```

</details>

---

## ⥯ Merge & Rebase

### Quando usar cada um?

| Critério | `git merge` | `git rebase` |
|----------|-------------|--------------|
| **Histórico** | Preserva exatamente o que aconteceu | Reescreve como se os commits fossem feitos em cima da outra branch |
| **Segurança** | Sempre seguro, mesmo em branches públicas | ⚠️ Nunca rebasear branches públicas |
| **Uso típico** | Integrar feature branches em main | Atualizar feature branch com mudanças de main |

<details>
<summary><strong>Merge</strong></summary>

```bash
git merge feature/login           # Integra a branch na branch atual (cria merge commit)
git merge --no-ff feature/login   # Força merge commit mesmo em fast-forward
git merge --squash feature/login  # Junta todos os commits da branch em um só
git merge --abort                 # Cancela merge com conflitos, restaura estado anterior
```

</details>

<details>
<summary><strong>Rebase</strong></summary>

```bash
git rebase main                   # Reaplica commits da branch atual em cima de 'main'
git rebase -i HEAD~3              # Rebase interativo: squash, reorder, reword ou drop
git rebase --continue             # Continua o rebase após resolver conflitos
git rebase --abort                # Cancela o rebase e restaura estado original
```

</details>

<details>
<summary><strong>Resolução de Conflitos</strong></summary>

Quando há conflito, o Git marca o arquivo assim:

```
<<<<<<< HEAD
código da branch atual
=======
código da branch que está sendo mergeada
>>>>>>> feature/login
```

Passos para resolver:

1. Edite o arquivo e escolha o código correto
2. Remova os marcadores `<<<<<<<`, `=======`, `>>>>>>>`
3. Adicione o arquivo ao stage: `git add arquivo.txt`
4. Finalize: `git commit` (merge) ou `git rebase --continue` (rebase)

</details>

---

## ⏸ Stash

> O stash é uma "gaveta" temporária para guardar alterações não commitadas, útil ao trocar de branch rapidamente.

```bash
git stash                         # Guarda todas as alterações não commitadas
git stash push -m "descrição"     # Guarda com mensagem descritiva
git stash list                    # Lista todos os stashes armazenados
git stash pop                     # Restaura o stash mais recente e o remove da lista
git stash apply stash@{2}         # Aplica stash específico sem removê-lo
git stash drop stash@{0}          # Remove um stash específico
git stash clear                   # Remove todos os stashes
```

---

## ◎ Log & Diff

<details>
<summary><strong>Histórico</strong></summary>

```bash
git log                                    # Histórico completo de commits
git log --oneline                          # Uma linha por commit
git log --oneline --graph --all            # Gráfico visual de branches (muito útil!)
git log -p arquivo.txt                     # Todas as mudanças feitas em um arquivo
git log --author="Nome" --since="1 week ago"  # Filtra por autor e período
git show abc1234                           # Detalhes e diff de um commit específico
```

</details>

<details>
<summary><strong>Diff & Blame</strong></summary>

```bash
git diff                          # Alterações no working dir (ainda não staged)
git diff --staged                 # Diferenças entre stage e último commit
git diff main..feature/login      # Compara duas branches
git blame arquivo.txt             # Mostra quem escreveu cada linha e em qual commit
```

</details>

---

## ⚡ Avançado

<details>
<summary><strong>Tags & Releases</strong></summary>

```bash
git tag v1.0.0                             # Cria tag leve no commit atual
git tag -a v1.0.0 -m "Release 1.0.0"      # Cria tag anotada (recomendada para releases)
git tag                                    # Lista todas as tags
git push origin --tags                     # Envia todas as tags para o remoto
git tag -d v1.0.0                          # Deleta tag local
git push origin --delete v1.0.0           # Deleta tag no remoto
```

</details>

<details>
<summary><strong>Cherry-pick</strong></summary>

```bash
git cherry-pick abc1234           # Aplica um commit específico na branch atual
git cherry-pick abc1234..def5678  # Aplica um intervalo de commits
git cherry-pick --abort           # Cancela o cherry-pick em andamento
```

> Útil para trazer correções pontuais de uma branch para outra sem fazer merge completo.

</details>

<details>
<summary><strong>Bisect — Encontrar bugs</strong></summary>

```bash
git bisect start                  # Inicia busca binária pelo commit com bug
git bisect bad                    # Marca o commit atual como com problema
git bisect good abc1234           # Marca um commit como funcionando
# O Git vai bisectar automaticamente até encontrar o commit culpado
git bisect reset                  # Encerra o bisect e volta ao estado original
```

</details>

<details>
<summary><strong>.gitignore & Utilidades</strong></summary>

```bash
# Adicionar padrão ao .gitignore
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore

# Parar de rastrear arquivo sem deletá-lo do disco
git rm --cached arquivo.txt

# Ranking de contribuidores por commits
git shortlog -sn

# Remover arquivos não rastreados (⚠️ irreversível)
git clean -fd

# Ver qual branch está rastreando qual remoto
git branch -vv

# Salvar alias útil (atalho personalizado)
git config --global alias.lg "log --oneline --graph --all"
# Uso: git lg
```

</details>

---

## 📌 Referência Rápida

| Ação | Comando |
|------|---------|
| Iniciar repositório | `git init` |
| Clonar repositório | `git clone <url>` |
| Ver status | `git status` |
| Adicionar ao stage | `git add .` |
| Criar commit | `git commit -m "msg"` |
| Enviar para remoto | `git push origin main` |
| Receber do remoto | `git pull origin main` |
| Criar branch | `git switch -c nome` |
| Trocar de branch | `git switch nome` |
| Integrar branch | `git merge nome` |
| Ver histórico | `git log --oneline --graph` |
| Guardar temporário | `git stash` |

---

<div align="center">
  <sub>Clique nas seções acima para expandir os comandos · Feito com ❤️ e Git</sub>
</div>
