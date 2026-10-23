# LW0 Parte 2 — projeto Python do grupo (Robotics 2026/2027, Lite 6)

Bernardo Pereira (2022219048) · João Lavos (<2021222529>)

Esta pasta é `lw0/parte2/` do repositório https://github.com/Baldaia12/Rob-ticaBaldaiaLavos.
Todos os comandos das secções 1–4 correm **a partir desta pasta**
(`cd Rob-ticaBaldaiaLavos\lw0\parte2`).

## 1. Recriar o ambiente a partir do zero (Python ≥ 3.10)

Linux / macOS:

```bash
python3 -m venv ~/venvs/robotica
source ~/venvs/robotica/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -e .
```

Windows (Linha de comandos, `cmd`):

```bat
python -m venv %USERPROFILE%\venvs\robotica
%USERPROFILE%\venvs\robotica\Scripts\activate
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install -e .
```

No PowerShell (ex.: terminal do VS Code) a ativação é
`& $env:USERPROFILE\venvs\robotica\Scripts\Activate.ps1`.

O `pip install -e .` instala o pacote `lite6`, para os imports funcionarem em `scripts/` e no IDE.
No IDE, escolher o interpretador de `venvs/robotica`.

## 2. Verificar (sem robô)

```
pytest -q
```

Tudo deve passar (o teste do `domain_id` falha até se definir o número do grupo).

## 3. Simulador de firmware UFACTORY (§C.4)

Primeira vez (uma só linha):

```
docker run -it --name uf_software -p 18333:18333 -p 502:502 -p 503:503 -p 504:504 -p 30000:30000 -p 30001:30001 -p 30002:30002 -p 30003:30003 danielwang123321/uf-ubuntu-docker
```

Vezes seguintes:

```
docker start -ai uf_software
```

Dentro do contentor, arrancar o firmware com o perfil Lite 6:

```
/xarm_scripts/xarm_start.sh 6 9
```

Studio: http://127.0.0.1:18333

## 4. Correr

Noutra janela, com o ambiente ativo:

```
python scripts/b5_connect.py --move
python scripts/test8_fk_compare.py --pose A --tf2-xyz 0.087 0.000 0.154
python scripts/lifecycle_tests.py
```

`b5_connect`: §B.5 / Teste 8. `test8_fk_compare`: Teste 8 (valores tf2 em metros).
`lifecycle_tests`: exercício de ciclo de vida.

Alvo: `target:` em `config/robot.yaml` (`sim` ou `lab`).
Contra o robô real: só com autorização do docente e um membro na paragem de emergência.

## 5. Nível avançado (ROS 2 Jazzy + MoveIt 2 + Gazebo Harmonic) — Via 1, Docker

O ambiente ROS 2 é a **raiz deste repositório** (cópia do ramo `windows` de
https://github.com/cazevedo/robotica). Windows, PowerShell, com o Docker Desktop
instalado e aberto, a partir da raiz `Rob-ticaBaldaiaLavos`:

```
copy .env.example .env
.\lab.ps1 build
.\lab.ps1 up
.\lab.ps1 doctor
```

O `build` faz-se uma vez (descarrega e compila tudo). Depois, em cada sessão:
`.\lab.ps1 up` e, em cada terminal, `.\lab.ps1 shell` (fica `ros@lab0:~/dev_ws$`).
Lá dentro, os testes do §C.5 são `lab test 1` … `lab test 7`
(o Teste 1 precisa de `lab test 1b` num segundo terminal). O PDF do Teste 4 aparece
em `shared/` (não versionada); as evidências usadas no relatório estão em
`docs/evidencias/`. Para parar: `.\lab.ps1 down`.

Se o Gazebo não mostrar o braço na primeira vez (`Failed to download model`),
parar com Ctrl+C e repetir: os modelos ficam em cache.

A instalação de raiz (Apêndice C da ficha, opcional) também foi feita numa VM
Ubuntu 24.04; não é necessária para nada neste projeto.

## Etiquetas entregues

`delivery-1` (LW0 + LW1 + LW2), `delivery-2`, `delivery-3`.
