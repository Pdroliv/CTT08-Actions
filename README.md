Para um README.md de alto nível no GitHub, o segredo é usar uma formatação limpa, ícones (emojis) para facilitar a leitura e blocos de código bem destacados. Esta versão foca em apresentar o laboratório como um projeto de portfólio ou documentação técnica.
🧪 Laboratório Prático: Local GitHub Actions com act

Este repositório contém a documentação e os artefatos do laboratório prático para simulação de pipelines de CI/CD localmente. O objetivo principal é validar workflows do GitHub Actions sem a necessidade de realizar commits repetitivos, utilizando a ferramenta act.
🎯 Objetivos do Laboratório

    Simular o ambiente de Runners do GitHub localmente via Docker.

    Validar a estrutura de dependências (DAG - Directed Acyclic Graph).

    Testar o paralelismo de Jobs e o uso de imagens customizadas.

    Otimizar o tempo de desenvolvimento e o consumo de recursos na nuvem.

📂 Estrutura do Projeto

A organização dos arquivos segue a convenção padrão do GitHub:
Plaintext

meu-projeto-ci/
├── .github/
│   └── workflows/
│       ├── 01-hello-local.yml        # Teste de fumaça (Hello World)
│       └── 02-pipeline-principal.yml # Pipeline complexo com dependências
├── comandos.txt                      # Log de comandos utilizados
└── README.md                         # Documentação do laboratório

🛠️ Execução e Validação
1. Validação da Estrutura (DAG)

Para garantir que a lógica de dependências (needs) está correta, utilizamos o comando de listagem. Ele mapeia a ordem de execução dos estágios:
Bash

act -l

Resultado esperado no terminal:
| Stage | Job ID | Job Name |
| :--- | :--- | :--- |
| 0 | setup-e-lint | Configuração inicial e análise estática |
| 1 | testes-unitarios | Execução da suíte de testes |
| 1 | scan-de-seguranca | Verificação de vulnerabilidades |
| 2 | build-e-deploy | Empacotamento e entrega final |

    Insight: Observe que os jobs no Stage 1 rodam simultaneamente, economizando tempo total de execução.

2. Execução com Imagem Customizada

Para alinhar o ambiente local com o de produção, executamos o workflow mapeando o Runner para uma imagem Docker específica:
Bash

act -W .github/workflows/02-pipeline-principal.yml -P ubuntu-latest=node:slim

3. Teste de Job Isolado

Caso precise validar apenas uma etapa específica (ex: Segurança):
Bash

act -j scan-de-seguranca

⚡ Comprovação de Paralelismo

Um dos pontos altos deste laboratório foi a validação do paralelismo real. Durante a execução do Stage 1, foram observadas as seguintes evidências:

    Logs Concorrentes: As saídas de testes-unitarios e scan-de-seguranca aparecem intercaladas no console.

    Containers Simultâneos: O act inicializa múltiplos containers Docker ao mesmo tempo.

    Independência: A falha ou sucesso de um job no Stage 1 não interrompe o início imediato do outro.

🚀 Conclusão

O uso do act permitiu uma validação 100% fiel ao ambiente do GitHub Actions. Com isso, garantimos que o pipeline está estruturalmente íntegro, que as dependências estão respeitando a hierarquia correta e que o paralelismo está otimizando o fluxo de CI/CD antes mesmo do primeiro git push.

Este laboratório faz parte dos meus estudos de DevOps e Automação.
