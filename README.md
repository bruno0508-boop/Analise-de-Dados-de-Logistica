# Dashboard de Análise de Dados de Logística com Power BI
Este projeto apresenta uma análise detalhada da performance logística de uma operação, focando em eficiência de entregas, pontualidade por região e performance de vendedores. O objetivo é identificar gargalos operacionais e monitorar o nível de serviço (SLA).

# 📊 Visão Geral do Dashboard
<p align="center">
  <img src="img/Analise de logistica.JPG" alt="Visão Cliente" width="800">
</p>

O relatório fornece uma visão 360º da operação logística através dos seguintes indicadores:

**Total de Entregas:** Volume total processado no período.

**Total de Entregas no Prazo:** Soma das entregas "Antecipadas" e "No Prazo".

**Percentual de Entregas por Equipe:** Gráfico de barras destacando o desempenho regional (Norte e Sudeste liderando o volume).

**Análise por Status:** Distribuição percentual entre entregas Antecipadas, No Prazo e Atrasadas.

**Ranking de Vendedores:** Tabela com sistema de Rating Visual (Estrelas) baseado na produtividade.

**Monitoramento de Atrasos:** Listagem das cidades com maior incidência de entregas fora do prazo.

# 🛠️ Tecnologias e Ferramentas
**Power BI:** Desenvolvimento de dashboards, modelagem de dados e publicação de relatórios.

**Linguagem DAX:** Criação de medidas complexas, tabelas calculadas e lógica de ranking (Rating).

**Power Query:** Processos de ETL (Extração, Transformação e Carga) e limpeza de dados.

# 🧮 Inteligência de Dados (DAX)
Neste projeto, destaquei-me pela criação de métricas de visualização avançada e filtros calculados:

**🌟 Rating de Estrelas Dinâmico**
Utilizei variáveis (VAR) para normalizar o volume de entregas em uma escala de 1 a 5, convertendo valores numéricos em símbolos visuais:

```
Classificação por estrelas TotalEntregas = 
VAR __MAX_NUMBER_OF_STARS = 5
VAR __MIN_RATED_VALUE = 1500
VAR __MAX_RATED_VALUE = 2500
VAR __BASE_VALUE = [TotalEntregas]
VAR __NORMALIZED_BASE_VALUE = 
    MIN(MAX(DIVIDE(__BASE_VALUE - __MIN_RATED_VALUE, __MAX_RATED_VALUE - __MIN_RATED_VALUE), 0), 1)
VAR __STAR_RATING = ROUND(__NORMALIZED_BASE_VALUE * __MAX_NUMBER_OF_STARS, 0)
RETURN
    IF(NOT ISBLANK(__BASE_VALUE),
        REPT(UNICHAR(9733), __STAR_RATING) & REPT(UNICHAR(9734), __MAX_NUMBER_OF_STARS - __STAR_RATING)
    )
```

**🚚 Indicadores de Performance (KPIs)**
Total de Entregas no Prazo:

```
TotalEntregaNoPrazo = CALCULATE([TotalEntregas], 
    FILTER(Logistica, Logistica[Status_Entrega] = "Antecipado" || Logistica[Status_Entrega] = "No prazo")
)
```

**Quantidade de Atrasos por Cidade:**

```
Qtd Atrasados por Cidade = CALCULATE(COUNTROWS('Logistica'), 'Logistica'[Status_Entrega] = "Atrasado")
```

# 📈 Insights Extraídos
**Eficiência Operacional:** Cerca de 87% das entregas são concluídas dentro ou antes do prazo, com uma forte dominância de entregas antecipadas (70,71%).

**Sazonalidade:** O gráfico de "Total de Entregas por Mês" revela uma queda acentuada em Setembro, sugerindo a necessidade de investigação sobre possíveis falhas na cadeia de suprimentos ou demanda nesse período.

**Desempenho Regional:** As equipes do Norte e Sudeste representam a maior fatia das entregas, sendo críticas para o sucesso da operação.
