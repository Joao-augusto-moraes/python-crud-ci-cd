# CRUD Python com Docker e CI/CD

> 🎓 **Projeto acadêmico** · Análise e Desenvolvimento de Sistemas, PUCPR · 2026

Um CRUD simples de usuários em Python, usado como base para praticar **testes automatizados, containerização e pipeline de CI/CD** com GitHub Actions.

O foco aqui não é a aplicação, e sim o caminho que ela percorre: cada push dispara o pipeline, que instala as dependências, roda os testes e avisa o resultado no Discord.

## 🧱 Stack

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white&style=flat-square)
![Pytest](https://img.shields.io/badge/Pytest-0A9EDC?logo=pytest&logoColor=white&style=flat-square)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white&style=flat-square)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white&style=flat-square)

## ⚙️ Como o pipeline funciona

```
push em qualquer branch / PR para main
        │
        ▼
 Job 1 · Testes (CI)
   1. Checkout do código
   2. Configura Python 3.11 (a mesma versão da imagem Docker)
   3. Instala dependências (falha de verdade se algo der errado)
   4. Roda os testes do CRUD
        │ só segue se os testes passarem
        ▼
 Job 2 · Docker (CD)
   5. Constrói a imagem Docker
   6. Executa o container como teste rápido (smoke test)
        │
        ▼
 Job 3 · Notifica o status no Discord (sempre, passando ou falhando)
```

O arquivo do pipeline fica em [`.github/workflows/ci-cd.yml`](.github/workflows/ci-cd.yml).

## 📂 Estrutura

| Arquivo | Para que serve |
|---|---|
| `crud.py` | Funções de criar, listar, buscar, atualizar e deletar usuários (em memória) |
| `test_crud.py` | 5 testes com pytest, um para cada operação |
| `Dockerfile` | Empacota a aplicação numa imagem `python:3.11-slim` |
| `requirements.txt` | Dependências do projeto |
| `.github/workflows/ci-cd.yml` | Pipeline de CI/CD |

## ▶️ Como rodar

**Localmente**
```bash
pip install -r requirements.txt
python crud.py          # executa a demonstração
pytest test_crud.py     # roda os testes
```

**Com Docker**
```bash
docker build -t crud-python .
docker run --rm crud-python
```

## 🧠 O que pratiquei

- Escrever testes isolados com `pytest` e fixtures (a lista é limpa antes de cada teste)
- Montar um pipeline com gatilhos por push e por pull request
- Usar *secrets* do GitHub para guardar o webhook do Discord
- Containerizar uma aplicação Python

## 🔭 Próximos passos

- [x] Renomear o arquivo de dependências para `requirements.txt` e remover o `|| true` da instalação
- [x] Alinhar a versão do Python entre CI e Docker (3.11)
- [x] Fazer o pipeline construir e executar a imagem Docker
- [ ] Retornar erro ao deletar ou atualizar um ID inexistente
- [x] Adicionar `.gitignore` e `.dockerignore` (tirar a pasta `__pycache__`)
