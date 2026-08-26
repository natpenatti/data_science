# 🏠 Análise de Dados — Airbnb na cidade de São Paulo

O Airbnb se consolidou como uma das maiores plataformas de hospitalidade do mundo, conectando viajantes a anfitriões em mais de 220 países e regiões — sem possuir um único hotel. Recentemente, a empresa ultrapassou a marca de 1,5 bilhão de hóspedes recebidos desde sua fundação, com mais de 8 milhões de imóveis ativos e receita anual superior a US$ 12 bilhões.

No Brasil, São Paulo se destaca como um dos mercados mais dinâmicos da plataforma na América Latina. Neste notebook, fazemos uma análise exploratória e modelagem preditiva sobre os anúncios do Airbnb no município de São Paulo, utilizando dados públicos do [Inside Airbnb](http://insideairbnb.com/get-the-data.html) (recorte de julho/2026).

O projeto percorre todo o fluxo de um problema de ciência de dados: **limpeza e tratamento de dados → análise exploratória (EDA) → visualização geográfica → modelagem preditiva de preços (Machine Learning supervisionado) → segmentação de mercado (Machine Learning não supervisionado)**.

## 🎯 Objetivos

- Compreender o perfil dos imóveis anunciados (tipo de acomodação, número de quartos, capacidade);
- Analisar a distribuição de preços por noite e identificar outliers;
- Mapear a concentração geográfica dos anúncios por bairro e zona da cidade;
- Investigar padrões de disponibilidade e sua relação com avaliações de hóspedes;
- Avaliar o perfil dos anfitriões (casuais vs. profissionais) e o impacto no mercado;
- Extrair insights acionáveis para potenciais anfitriões, investidores e pesquisadores do mercado de *short-term rental*.

## 🗂️ Sobre os dados

- **Fonte:** [Inside Airbnb](http://insideairbnb.com/get-the-data.html) — dados públicos de anúncios da cidade de São Paulo (`listings.csv.gz`).
- **Volume original:** ~42.354 anúncios × 90 variáveis.
- Das 90 colunas, foram selecionadas **24 variáveis** relevantes (identificação, anfitrião, localização, características do imóvel, preço, avaliações e disponibilidade), descartando metadados de scraping e colunas redundantes.
- Etapas de tratamento: conversão de tipos (preço de string para numérico, datas, booleanos), diagnóstico e tratamento de valores ausentes, remoção de duplicatas e de outliers extremos (baseado em percentis).

## 🔍 Estrutura da análise

1. Análise inicial do dataset (dimensões, tipos de variáveis, dicionário de dados)
2. Tratamento de dados (seleção de colunas, conversão de tipos, valores ausentes, outliers)
3. Estatística descritiva e análise de variáveis categóricas
4. Matriz de correlação
5. Análise geográfica (distribuição espacial dos anúncios)
6. Preço por tipo de acomodação, por Superhost, por bairro
7. Disponibilidade e análise temporal
8. Perfil dos anfitriões (escala, profissionalização, identidade verificada)
9. Estadia mínima
10. **Modelagem preditiva de preço** — comparação entre Regressão Linear e Random Forest
11. **Interpretabilidade com SHAP** — quais variáveis mais influenciam o preço
12. **Mapa coroplético** — preço mediano por bairro
13. **Clusterização (K-Means)** — segmentação da cidade em "micro-mercados" por perfil de localização, preço e capacidade

## 📊 Principais insights

**Visão geral do mercado**
O mercado é dominado por imóveis inteiros (76,7% dos anúncios) e estadias curtas (61% com mínimo de 1 noite). Preço médio de R$ 376 e mediana de R$ 320.

**Um mercado profissionalizado**
35,1% dos anúncios pertencem a anfitriões com mais de 20 imóveis — o mercado é composto majoritariamente por operadores em escala, não por pessoas alugando espaços ociosos. Curiosamente, escala não é premiada com preços mais altos, mas está associada a notas de avaliação levemente menores.

**Localização importa, mas não é tudo**
O preço se concentra fortemente na zona centro-oeste (Itaim Bibi, Pinheiros, Jardim Paulista). Ainda assim, o modelo preditivo mostrou que a **capacidade do imóvel** pesa mais do que o **bairro** na formação do preço — e que a presença de **piscina** é o segundo fator mais relevante, à frente até da localização, no modelo com engenharia de atributos.

**Superhost é selo de qualidade, não de preço**
Apenas 16,1% dos anúncios são de superhosts, que cobram preços praticamente iguais aos demais anfitriões, mas recebem notas consistentemente melhores em todas as dimensões avaliadas.

**Modelagem preditiva**
O Random Forest superou a Regressão Linear (R² de 0,54 vs. 0,42; RMSE de R$ 200,61 vs. R$ 219,86), confirmando relações não-lineares entre localização, tipo de acomodação e capacidade. Com engenharia de atributos adicional (amenidades extraídas do texto), o modelo final atingiu R² ≈ 0,43 e MAE ≈ R$ 101.

**Segmentação por micro-mercados (K-Means)**
A cidade foi dividida em 5 clusters que revelam perfis de consumo distintos — do "reduto premium/executivo" concentrado no eixo nobre, a clusters econômicos e descentralizados, até um cluster voltado a grupos maiores. A recomendação de negócio é que preço, marketing e mobília sejam direcionados ao perfil de cada micro-mercado, e não apenas ao bairro.

## 🛠️ Tecnologias e bibliotecas

- **Manipulação e análise de dados:** `pandas`
- **Visualização:** `matplotlib`, `seaborn`, `plotly.express`
- **Dados geoespaciais:** `geopandas`, `folium`
- **Machine Learning:** `scikit-learn` (Regressão Linear, Random Forest, K-Means, pré-processamento e métricas)
- **Interpretabilidade de modelos:** `shap`

## ▶️ Como executar

1. Clone o repositório e instale as dependências:
   ```bash
   pip install pandas matplotlib seaborn plotly geopandas folium shap scikit-learn
   ```
2. Abra o notebook `Airbnb.ipynb` em Jupyter ou VS Code.
3. O dataset é carregado diretamente da URL pública do Inside Airbnb — não é necessário baixar arquivos manualmente.

## ⚠️ Limitações

- `last_review` captura apenas a data mais recente, não permitindo análise de sazonalidade real.
- `price` reflete o valor no momento da coleta e pode não representar preços efetivamente praticados (descontos para estadias longas não são capturados).
- Variáveis como área do imóvel, qualidade das fotos e taxa de ocupação real não estão disponíveis, o que limita a capacidade preditiva dos modelos.
- Anúncios sem avaliação (~12,9%) foram excluídos das análises de nota, o que pode introduzir viés de seleção.

## 👩‍💻 Autora

**Natasha Penatti**
*Bióloga | Doutora em Geociências | Especialista em Sensoriamento Remoto | Estudante de Data Science & Computer Vision*

- [LinkedIn](https://www.linkedin.com/in/natasha-penatti/)
- [Google Scholar](https://scholar.google.com/citations?user=ThzGTyUAAAAJ&hl=en)