# Painel de Entregas — PIT Engenharia de Software II

Sistema web para acompanhar o status de entregas de cartões (pedido → romaneio → transportadora → entrega),
com histórico de atualizações por pedido e status composto calculado automaticamente.

## Stack
Python 3 · Flask (padrão **MVC**) · SQLite · HTML/CSS · pytest

## Arquitetura (MVC)
| Camada | Arquivo | Papel |
|---|---|---|
| Model | `painel/models.py` | Acesso ao banco SQLite (CRUD de pedidos e eventos) |
| View | `painel/templates/*.html` | Telas (lista/dashboard, formulário, detalhe com histórico) |
| Controller | `painel/controllers.py` | Rotas, validação de entrada e escolha da view |
| Regras | `painel/services.py` | Validação (CPF, datas), dias úteis e status — funções puras testáveis |

## Como rodar
```
pip install -r requirements.txt
python run.py        # http://localhost:5010
pytest -v            # testes
```

## Requisitos funcionais
- RF01 Cadastrar, editar, excluir e consultar pedidos (busca por nome, CPF, pedido, romaneio, empresa)
- RF02 Registrar atualizações no histórico (normal, ocorrência, entrega, devolução)
- RF03 Calcular o limite de entrega em dias úteis e o status composto
- RF04 Dashboard com contagem por status

## Requisitos não funcionais
Validação com mensagens de erro claras (IHC: feedback imediato) · integridade (CPF+pedido único, FK com cascade) ·
testabilidade (regras separadas do Flask) · portabilidade (SQLite, sem servidor de banco).

## Regra de status
Devolução → **Devolvido**; com entrega → **Entregue no Prazo** / **Entregue em Atraso** (vs. limite);
sem romaneio → **Em Produção**; senão **Em Rota** (ou **Em Atraso** se passou do limite).
Se houver evento de ocorrência, acrescenta o sufixo **" - Ocorrência"**.

## Estrutura de pastas
`docs/` modelagem e dicionário de dados · `tests/` testes automatizados (verificação e validação)
