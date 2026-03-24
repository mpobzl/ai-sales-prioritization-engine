🚀 AI Sales Prioritization Engine — Phase 1 (Data + Scoring)
🎯 Objetivo
Desenvolver uma solução funcional para priorização de oportunidades comerciais, permitindo que vendedores saibam onde focar seus esforços com base em dados, substituindo decisões baseadas apenas em intuição.

Este é o link do projeto final já funcional: https://ai-sales-prioritization-engine-vbayfyecuz7lyyejzgpvst.streamlit.app/
________________________________________
📊 Contexto do Problema
O time comercial trabalha com ~8.800 oportunidades ativas, mas:
•	A priorização é feita de forma subjetiva (“feeling”)
•	Leads de alta qualidade podem ser negligenciados
•	Tempo é gasto em oportunidades com baixa chance de conversão
A solução proposta resolve esse problema com um modelo de priorização baseado em dados históricos reais.
________________________________________
🧱 1. Integração de Dados (Merge)
Foram integradas 4 bases principais:
Arquivo	Conteúdo
sales_pipeline.csv	Pipeline de oportunidades
accounts.csv	Dados dos clientes
products.csv	Catálogo de produtos
sales_teams.csv	Estrutura comercial
🔗 Estratégia de Merge
sales_pipeline (base principal)
    + accounts (account)
    + products (product)
    + sales_teams (sales_agent)
⚠️ Validação do Merge
•	Identificação de registros left_only
•	Verificação de chaves inconsistentes
•	Diagnóstico de dados faltantes (ex: account, sales_price)
________________________________________
🧹 2. Limpeza e Tratamento de Dados
✔️ Padronização
•	Lowercase em campos categóricos
•	Remoção de espaços (strip)
•	Tratamento de strings inconsistentes (ex: "nan" vs NaN)
________________________________________
✔️ Tratamento de Missing Data
Identificado problema crítico:
•	~1.425 registros com account como string "nan"
Solução:
account != "" AND account != "nan"
________________________________________
✔️ Classificação de Qualidade
Criação de flag:
Tipo	Regra
high	dados completos
medium	dados incompletos
________________________________________
✔️ Separação operacional
•	Leads válidos → entram no modelo
•	Leads incompletos → excluídos da priorização
________________________________________
📊 3. Definição de Métricas
🔹 Win Rate
Win Rate = Won / (Won + Lost)
Calculado com base em dados históricos reais por contexto.
________________________________________
🔹 Ticket (Valor)
Utilizado:
sales_price (products.csv)
Como proxy de valor do deal (já que leads ativos não têm close_value).
________________________________________
🔹 Price Ratio
close_value / sales_price
Indica eficiência de negociação (desconto vs valor cheio).
________________________________________
🔹 Sales Cycle (Tempo de Venda)
close_date - engage_date
Utilizado como variável de apoio para decisão (não entra no score).
________________________________________
🔹 Deal Stage
Filtrados apenas:
•	Engaging
•	Prospecting
Excluídos:
•	Won (já fechados)
•	Lost (já perdidos)
________________________________________
🧠 4. Lógica de Scoring
🎯 Fórmula Principal
Score =
0.5 × Win Rate +
0.3 × Ticket (normalizado) +
0.2 × Price Ratio
________________________________________
⚠️ Importante
•	Stage NÃO entra no score
•	Cycle NÃO entra no score
•	Ambos são usados como contexto de decisão
________________________________________
🔁 5. Fallback Hierárquico (Diferencial do Modelo)
Para evitar distorções por baixo volume, foi implementado um sistema de fallback:
🔥 Ordem de prioridade:
1.	sales_agent + product + sector + region
2.	sales_agent + product + sector
3.	sales_agent + sector
4.	sales_agent + product
5.	sales_agent
________________________________________
📊 Regra de seleção:
Usar o primeiro nível com volume ≥ 30
________________________________________
🎯 Benefício
•	Evita overfitting
•	Garante robustez estatística
•	Melhora confiabilidade do score
________________________________________
🧪 6. Validação do Modelo
Foi realizada validação manual completa:
✔️ Reproduzido cálculo de:
•	Win Rate
•	Price Ratio
•	Score final
✔️ Resultado:
•	Score validado: 0.89
•	Consistência com output do modelo
________________________________________
🔥 Insight importante
Amostras pequenas (ex: 3 deals com 100% win rate) são descartadas em favor de bases maiores e mais confiáveis.
________________________________________
⚙️ 7. Lógica de Priorização
🎯 Ranking final:
1.	Score (descendente)
2.	Sales Cycle (crescente — desempate)
________________________________________
🧠 Interpretação
•	Score → prioridade estratégica
•	Cycle → velocidade de retorno
•	Stage → momento da negociação
________________________________________
📊 8. Output Executivo
A solução gera uma tabela pronta para uso pelo time comercial:
Rank | Conta | Produto | País | Segmento | Stage | Ciclo | Valor | WinRate | Score
________________________________________
🎯 Exemplo:
1º | zoomit | gtx plus pro | USA | entertainment | engaging | 50 dias | 5482 | 78.9% | 0.89
________________________________________
🧠 Benefícios para o vendedor
•	Clareza de onde focar
•	Entendimento do porquê (explicabilidade)
•	Visão de velocidade (cycle)
•	Contexto de negociação (stage)
________________________________________
🚀 Conclusão
Esta primeira fase entregou:
✔️ Modelo funcional e robusto
✔️ Priorização baseada em dados reais
✔️ Lógica explicável e auditável
✔️ Output pronto para uso comercial
________________________________________

________________________________________
🚀 AI Sales Prioritization Engine — Phase 2 (Usabilidade + Explainability + Escalabilidade)
________________________________________
🧠 9. Explainability (Explicabilidade do Modelo)
Um dos principais requisitos do challenge era garantir que o vendedor entendesse claramente por que um lead está priorizado.
Para isso, foi adotada uma abordagem de explicabilidade orientada ao negócio:
🎯 Estratégia adotada
Ao invés de explicações técnicas por linha (que dificultam leitura), foi implementado:
✔️ Explicação centralizada no cabeçalho
✔️ Linguagem executiva (não técnica)
✔️ Foco em decisão rápida
________________________________________
📌 Como o modelo se explica
COMO USAR ESTE RELATÓRIO

Este ranking foi gerado a partir de análise estatística do seu histórico de vendas.
O score combina taxa de conversão, ciclo de venda e qualidade de preço por cliente e produto.

Siga a ordem da lista para focar nos leads com maior probabilidade de fechamento e retorno.
________________________________________
🧠 Decisão de design
Abordagem	Decisão
Explicação por linha	❌ removido (poluição visual)
Fórmula técnica	❌ removida
Explicação central	✅ adotada
________________________________________
🎯 Benefício
•	Reduz esforço cognitivo do vendedor 
•	Aumenta confiança no modelo 
•	Facilita adoção prática no dia a dia 
________________________________________
⚙️ 10. Filtros Dinâmicos (Bonus do Challenge)
A solução foi evoluída para permitir diferentes níveis de análise:
🔹 Níveis suportados
Tipo de análise	Uso
Vendedor	Priorização individual
Gerente	Gestão de equipe
Região	Visão estratégica
________________________________________
🎯 Implementação
A mesma engine de scoring foi reutilizada com filtros dinâmicos:
sales_agent=None
manager=None
region=None
________________________________________
🧠 Benefício
•	Reutilização do modelo 
•	Escalabilidade 
•	Aplicação real em contexto organizacional 
________________________________________
🧩 11. Contexto Dinâmico no Output
Para evitar ambiguidade no uso do relatório, foi adicionado um cabeçalho contextual:
🎯 Exemplo
📊 CONTEXTO DA ANÁLISE
👤 Vendedor: Corliss Cosme
ou
👨‍💼 Gerente: Cara Losch
🌎 Região: East
________________________________________
🎯 Benefício
•	Clareza sobre escopo da análise 
•	Evita interpretações incorretas 
•	Facilita uso em reuniões e reporting 
________________________________________
🧱 12. Tratamento de Qualidade de Dados no Output
Foi incorporada uma camada explícita de qualidade de dados:
Leads ativos encontrados: 81

Qualidade dos dados:
✔️ Completo: 27
⚠️ Incompleto: 54
________________________________________
🧠 Insight gerado
Além da priorização, a solução também identifica:
Problemas estruturais no CRM (dados incompletos)
________________________________________
🎯 Impacto
•	Possibilita ações de melhoria de dados 
•	Aumenta confiabilidade do modelo 
•	Gera valor além da priorização 
________________________________________
📊 13. Output Final (Versão Executiva)
A versão final foi refinada para máxima clareza e uso prático:
🎯 Estrutura final
Rank | Conta | Produto | Stage | Ciclo | Valor | Preço | WinRate | Score
________________________________________
🎯 Características
✔️ Sem ruído visual
✔️ Sem elementos desnecessários
✔️ Ordenação clara
✔️ Pronto para decisão imediata
________________________________________
🧠 Princípio adotado
“Uma linha = uma decisão”
________________________________________
🚀 14. Decisões de Produto (Diferencial)
Durante o desenvolvimento, foram tomadas decisões importantes que elevam a solução:
🔥 1. Simplicidade > Complexidade
•	Evitado uso de modelos complexos (ex: XGBoost) 
•	Priorizado modelo explicável e utilizável 
________________________________________
🔥 2. Explainability > Black Box
•	Vendedor entende o porquê do ranking 
•	Aumenta confiança e adoção 
________________________________________
🔥 3. Uso real > Perfeição técnica
•	Pensado para uso na rotina (segunda-feira de manhã) 
•	Foco em ação, não em análise 
________________________________________
🔥 4. Reutilização de engine
•	Um único modelo 
•	Múltiplos níveis de aplicação (vendedor, gerente, região) 
________________________________________
🏁 15. Resultado Final
A solução entregue nesta fase é:
✔️ Funcional
✔️ Escalável
✔️ Explicável
✔️ Utilizável por não técnicos
✔️ Baseada em dados reais
________________________________________
🎯 Impacto direto
•	Redução de tempo gasto em leads ruins 
•	Aumento de taxa de conversão potencial 
•	Melhor uso do tempo do vendedor 
•	Maior previsibilidade comercial 


🌐 16. Interface com Streamlit (Aplicação Real)
Após a construção da engine de priorização, foi desenvolvida uma interface interativa utilizando Streamlit, com foco em uso prático pelo time comercial.
🎯 Objetivo
Transformar o modelo analítico em uma ferramenta utilizável no dia a dia, acessível via navegador, sem necessidade de conhecimento técnico.
________________________________________
🧱 17. Arquitetura da Solução
A arquitetura foi desenhada com separação clara entre lógica e apresentação:
project/
├── app.py              # Interface (Streamlit)
├── model.py            # Engine de priorização
├── data/
│   ├── sales_pipeline.csv
│   ├── accounts.csv
│   ├── products.csv
│   └── sales_teams.csv
🧠 Princípio adotado
•	model.py → responsável pelo cálculo 
•	app.py → responsável pela experiência do usuário 
🎯 Benefício:
•	Manutenção simplificada 
•	Reutilização da engine 
•	Separação clara de responsabilidades 
________________________________________
🎛️ 18. Filtros Interativos
A interface permite análise em múltiplos níveis:
•	Sales Agent (Vendedor) 
•	Manager (Gerente) 
•	Region (Região) 
🎯 Comportamento
Os filtros impactam:
•	Ranking exibido 
•	Métricas de qualidade 
•	Contexto da análise 
________________________________________
🧠 19. Context-Aware Analytics (Diferencial)
A interface foi projetada para adaptar automaticamente a informação exibida de acordo com o nível de análise:
Contexto	Informação adicional exibida
Vendedor	—
Gerente	Nome do vendedor
Região	Nome do gerente
Além disso, sempre são exibidos:
•	Setor (sector) 
•	Região operacional (office_location) 
🎯 Benefício:
•	Evita perda de contexto 
•	Facilita leitura executiva 
•	Permite uso em diferentes níveis da organização 
________________________________________
📊 20. Qualidade de Dados (Camada Estratégica)
A aplicação não apenas prioriza leads, mas também evidencia problemas estruturais de dados:
🎯 Métricas exibidas
•	Leads ativos encontrados 
•	Leads completos (utilizados no modelo) 
•	Leads incompletos (excluídos) 
🧠 Insight
A solução passa a atuar também como ferramenta de governança de dados comerciais
________________________________________
📈 21. Output Final (Interface)
A interface entrega:
•	Ranking ordenado automaticamente 
•	Destaque visual por score 
•	Dados formatados para leitura executiva 
•	Scroll completo (todos os leads acessíveis) 
🎯 Estrutura final
Rank | Conta | Produto | Stage | Setor | Região | Ciclo (dias) | Valor | Preço Alvo (%) | WinRate | Score
________________________________________
⚙️ 22. Decisões Técnicas Importantes
🔹 Precisão vs Visualização
•	Model: mantém precisão (3 casas decimais) 
•	Interface: exibe 2 casas (clareza) 
🔹 Formatação executiva
•	Valores monetários formatados ($) 
•	Percentuais (%) 
•	Ciclo sem casas decimais 
________________________________________
🧠 23. Princípios de Produto Aplicados
Durante a construção da interface, foram reforçados princípios fundamentais:
✔️ Data > Design
A qualidade do dado foi priorizada em relação à estética
✔️ Simplicidade operacional
Interface pensada para uso rápido (menos de 1 minuto)
✔️ Uma tela, uma decisão
O usuário não precisa navegar — tudo está na mesma view
________________________________________
🚀 24. Deploy e Acesso
A aplicação foi preparada para deploy via Streamlit Cloud.
🎯 Estratégia
•	Código hospedado no GitHub 
•	Dados em CSV dentro do repositório 
•	Deploy automático 
🌐 Resultado esperado
•	Link público acessível 
•	Sem necessidade de instalação 
•	Pronto para uso em contexto real 
________________________________________
💼 25. Aplicação no Mundo Real
Esta solução pode ser aplicada diretamente em:
•	Times de vendas B2B 
•	Operações de CRM 
•	Gestão comercial 
•	RevOps 
🎯 Casos de uso
•	Priorização semanal de pipeline 
•	Reuniões de forecast 
•	Gestão de performance de vendedores 
•	Identificação de gargalos de conversão 
________________________________________
🏁 26. Conclusão Final
O projeto evoluiu de um modelo analítico para uma solução completa de decisão comercial:
✔️ Engine robusta e explicável
✔️ Interface funcional e utilizável
✔️ Lógica estatística consistente
✔️ Aplicação real em ambiente corporativo
________________________________________
🔥 Diferencial Principal
Mais do que prever resultados, a solução:
👉 orienta ação

🌐 27. Deploy em Produção (Streamlit Cloud)
A aplicação foi publicada utilizando o Streamlit Cloud, permitindo acesso público via navegador.

🎯 Estratégia de Deploy
•	Repositório hospedado no GitHub 
•	Branch: main 
•	Arquivo principal: app.py 
•	Deploy automático integrado ao GitHub 
🔁 Pipeline de Deploy
1.	Push no GitHub 
2.	Streamlit detecta alteração 
3.	Instala dependências (requirements.txt) 
4.	Executa aplicação 
5.	Gera URL pública 
________________________________________
⚠️ 28. Problemas Encontrados em Produção
Durante o deploy, foram identificados desafios típicos de ambiente real:
________________________________________
🔴 Problema 1 — Dependências ausentes
Erro:
ImportError: background_gradient requires matplotlib
Causa:
A biblioteca matplotlib não estava listada no requirements.txt.
________________________________________
✅ Solução
Atualização do arquivo:
streamlit
pandas
numpy
matplotlib
________________________________________
🎯 Aprendizado
Ambientes locais ≠ ambientes de produção
Todas as dependências devem ser explicitamente declaradas
________________________________________
🔴 Problema 2 — requirements.txt vazio
Erro:
Aplicação rodava sem instalar dependências
Causa:
Arquivo requirements.txt não estava preenchido no repositório
________________________________________
✅ Solução
Preenchimento manual diretamente no GitHub
________________________________________
🎯 Aprendizado
Arquivos de configuração são críticos para deploy automatizado
________________________________________
🔴 Problema 3 — Erros silenciosos na interface
A aplicação apresentava falhas sem mensagens claras no front-end.
________________________________________
✅ Solução
Uso dos logs do Streamlit Cloud:
•	“Manage app” 
•	Acesso ao stack trace completo 
________________________________________
🎯 Aprendizado
Leitura de logs é essencial para debugging em produção
________________________________________
🧠 29. Robustez da Solução
Após ajustes, a aplicação passou a operar corretamente em ambiente cloud:
✔️ Interface carregando corretamente
✔️ Dados sendo processados em tempo real
✔️ Filtros funcionando
✔️ Ranking exibido corretamente
✔️ Formatação aplicada
________________________________________
🔁 30. Ciclo de Atualização
A arquitetura permite atualização contínua:
Editar código → Commit → Push → Deploy automático
🎯 Benefício:
•	Iteração rápida 
•	Evolução contínua do produto 
•	Facilidade de manutenção 
________________________________________
🌍 31. Resultado Final (Produto Publicado)
A solução está disponível como:
👉 Aplicação web acessível
👉 Sem necessidade de instalação
👉 Pronta para uso por usuários finais
________________________________________
💡 32. Maturidade do Projeto
O projeto evoluiu por 4 níveis:
Fase	Entrega
Phase 1	Modelagem e scoring
Phase 2	Usabilidade e explicabilidade
Phase 3	Interface e aplicação
Phase 4	Deploy real e produção
________________________________________
🏁 33. Conclusão Final do Projeto
Este projeto não é apenas um exercício técnico.
Ele representa:
✔️ Uma solução de negócio aplicável
✔️ Um produto funcional
✔️ Um modelo explicável e confiável
✔️ Uma aplicação pronta para uso real
✔️ Um sistema completo de decisão comercial



