# tmux-labs

Automação prática para criação de ambientes multi-pane no **tmux**, com geração dinâmica de layouts e execução automática de comandos por terminal.

Ideal para:

* laboratórios de rede
* pentest
* debugging distribuído
* relay / pivoting
* monitoramento
* desenvolvimento multi-processo

---

# ✨ Visão geral

O `tmux` sempre inicia com **1 pane**.
Cada `split-window` adiciona **+1 pane**.

Portanto:

```
panes = 1 + splits
splits = panes_desejados - 1
```

Os scripts deste repositório automatizam exatamente isso.

---

# 📦 Scripts

## tmux-panes

Cria **N panes organizados automaticamente em grid**.

### Uso

```
tmux-panes <num_panes> [sessao]
```

### Exemplos

```
tmux-panes 4
tmux-panes 6 lab
tmux-panes 9 pentest
```

### Resultado

```
┌────┬────┬────┐
│ 0  │ 1  │ 2  │
├────┼────┼────┤
│ 3  │ 4  │ 5  │
└────┴────┴────┘
```

---

## tmux-lab

Cria panes e executa **um comando diferente em cada pane**.

### Uso

```
tmux-lab <sessao> "cmd1" "cmd2" "cmd3" ...
```

### Exemplo (ncat relay lab)

```
tmux-lab relay \
"ncat -lv 80" \
"ncat -lv 443" \
"ncat 127.0.0.1 80" \
"ncat 127.0.0.1 443"
```

Layout gerado:

```
┌────────────┬────────────┐
│ listener80 │ listener443│
├────────────┼────────────┤
│ client80   │ client443  │
└────────────┴────────────┘
```

---

# 🧠 Como funciona

## tmux-panes

1. cria sessão tmux
2. executa N-1 splits
3. aplica layout `tiled`
4. conecta à sessão

---

## tmux-lab

1. conta comandos recebidos
2. cria panes equivalentes
3. aplica layout grid
4. envia comando para cada pane
5. conecta à sessão

---

# 🚀 Instalação

```
git clone https://github.com/SEU_USUARIO/tmux-labs.git
cd /naldodj-tmux-labs/src
sudo cp tmux-panes /usr/local/bin/
sudo cp tmux-lab /usr/local/bin/
sudo chmod +x /usr/local/bin/tmux-*
```

---

# 🎯 Casos de uso

## Pentest

```
tmux-lab pentest \
"sudo tcpdump -i eth0" \
"nmap -sV target" \
"msfconsole" \
"htop"
```

## Debug distribuído

```
tmux-lab debug \
"appserver" \
"tail -f console.log" \
"top" \
"sqlcmd"
```

## Monitoramento

```
tmux-lab monitor \
"htop" \
"iotop" \
"iftop" \
"watch df -h"
```

---

# 📐 Layout

O layout final utiliza:

```
tmux select-layout tiled
```

Isso reorganiza automaticamente todos os panes em grade balanceada.

---

# 🧬 Conceito

Esses scripts tratam o tmux como um **orquestrador leve de processos interativos**, permitindo montar ambientes complexos com um único comando.

---

# 🔮 Possíveis evoluções

* perfis YAML de labs
* restore de sessões
* layouts salvos
* bibliotecas de cenários
* integração com containers

---

# 📜 Licença

MIT

---

# 👤 Autor

Naldo DJ

---