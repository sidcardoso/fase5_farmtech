# 🌾 FarmTech Solutions — Previsão de Rendimento de Safra
### FIAP | Fase 5 | Machine Learning na Cabeça

---

## 📋 Sobre o Projeto

A **FarmTech Solutions** presta serviços de IA para uma fazenda de médio porte (200 hectares) que produz múltiplas culturas agrícolas. Este projeto analisa dados de condições climáticas e de solo para:

1. **Prever o rendimento de safra** (Entrega 1 — Machine Learning Supervisionado)
2. **Explorar tendências de produtividade** via clusterização (ML Não Supervisionado)
3. **Estimar custos de infraestrutura em nuvem** para hospedar a solução (Entrega 2 — AWS)

---

## 📁 Estrutura do Repositório

```
📦 fase5farmtech/
├── 📓 SidneyCardoso_rm567808_pbl_fase5.ipynb  ← Notebook principal (toda a análise)
├── 📂 arquivo/
│   └── crop_yield.csv                          ← Dataset com dados climáticos e rendimento
└── 📄 README.md                                ← Este arquivo
```

---

## 📓 Jupyter Notebook

Todo o desenvolvimento técnico, análises, modelos e conclusões estão documentados no notebook:

**➡️ [`SidneyCardoso_rm567808_pbl_fase5.ipynb`](./SidneyCardoso_rm567808_pbl_fase5.ipynb)**

### Conteúdo do Notebook:

| Seção | Descrição |
|---|---|
| 1. Setup & Importações | Bibliotecas utilizadas |
| 2. Carregamento dos Dados | Leitura do CSV, inspeção e estatísticas |
| 3. Análise Exploratória (EDA) | Distribuições, correlações, scatter plots, boxplots |
| 4. Clusterização & Outliers | KMeans, DBSCAN e método IQR |
| 5. Modelagem (5 Algoritmos) | Linear, Ridge, Random Forest, Gradient Boosting, SVR |
| 6. Avaliação & Comparação | R², MAE, RMSE, validação cruzada, radar chart |
| 7. Conclusões | Achados, pontos fortes, limitações e recomendações |

---

## 🎥 Vídeo Demonstrativo — Entrega 1

> **📹 Link do vídeo (YouTube — não listado):**  
https://youtu.be/fo2wfJZq-oA

---

---

## ☁️ Entrega 2 — Computação em Nuvem (AWS)

### Contexto

A API que receberá dados dos sensores de campo e executará o modelo de Machine Learning precisa ser hospedada em nuvem. Realizamos uma estimativa de custos **On-Demand (100%)** na **AWS Calculator** para duas regiões: **São Paulo (sa-east-1)** e **Virgínia do Norte (us-east-1)**.

---

### ⚙️ Configuração da Máquina Analisada

| Parâmetro | Especificação |
|---|---|
| **Sistema Operacional** | Linux |
| **vCPUs** | 2 |
| **Memória RAM** | 1 GiB |
| **Largura de Banda de Rede** | Até 5 Gigabit |
| **Armazenamento** | 50 GB (EBS gp3) |
| **Modelo de Pagamento** | On-Demand (100%) |

**Instância equivalente identificada:** `t3.micro` (2 vCPU, 1 GiB RAM — a mais próxima das especificações)

---

### 💰 Comparativo de Custos — AWS Calculator

#### Região: São Paulo (sa-east-1 — Brasil)

| Componente | Custo Mensal (USD) |
|---|---|
| EC2 `t3.micro` On-Demand (Linux) | ~$12,56/mês |
| Armazenamento EBS gp3 (50 GB) | ~$4,75/mês |
| **Total Estimado** | **~$17,31/mês** |

> Referência: instância `t3.micro` em sa-east-1 custa aproximadamente USD 0,0168/hora × 730h = ~$12.26/mês

---

#### Região: Virgínia do Norte (us-east-1 — EUA)

| Componente | Custo Mensal (USD) |
|---|---|
| EC2 `t3.micro` On-Demand (Linux) | ~$7,59/mês |
| Armazenamento EBS gp3 (50 GB) | ~$4,00/mês |
| **Total Estimado** | **~$11,59/mês** |

> Referência: instância `t3.micro` em us-east-1 custa aproximadamente USD 0,0104/hora × 730h = ~$7.59/mês

---

### 📊 Gráfico Comparativo de Custos

```
Custo Mensal Estimado (USD)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

São Paulo (BR)     ████████████████████  $17,31
Virgínia (EUA)     █████████████         $11,59

                   0      5     10     15     20 USD/mês

Economia optando pela Virgínia: ~$5,72/mês (~33% mais barato)
```

> ⚠️ **Nota:** Os valores acima foram estimados com base nos preços públicos da AWS em março/2026. Para valores exatos e atualizados, consulte: https://calculator.aws/pricing/2/homescreen

---

### ✅ Qual Região Escolher?

#### Cenário 1: Critério principal = **Menor Custo**

**→ Resposta: Virgínia do Norte (us-east-1)**  
A região us-east-1 é aproximadamente **33% mais barata** que sa-east-1. A AWS usa essa região como baseline de preços globais, beneficiando-se de economias de escala por ser o maior datacenter da empresa no mundo.

---

#### Cenário 2: Dados de sensores em tempo real + Restrições legais

**→ Resposta: São Paulo (sa-east-1) — RECOMENDADA para este projeto**

**Justificativa técnica:**

**1. Latência de Rede:**  
Os sensores IoT estão fisicamente localizados na fazenda no Brasil. Dados enviados para a Virgínia percorrem ~8.000 km via cabo submarino, resultando em **latência de 100-200ms**. No datacenter de São Paulo, a latência seria de **5-20ms** — 10x melhor para aplicações em tempo real críticas para irrigação e tomada de decisão agrícola imediata.

**2. Conformidade Legal (LGPD):**  
A **Lei Geral de Proteção de Dados (Lei 13.709/2018)**, a LGPD brasileira, impõe restrições à transferência internacional de dados pessoais. Embora dados de sensores agrícolas (temperatura, umidade, rendimento) não sejam dados pessoais no sentido estrito, **dados de produtividade da fazenda são estratégicos e comercialmente sensíveis**. Armazená-los no Brasil:
- Facilita auditoria e compliance regulatório
- Evita exposição à jurisdição estrangeira (leis dos EUA como CLOUD Act)
- Garante soberania sobre dados produtivos da operação

**3. Disponibilidade e SLA:**  
Embora ambas as regiões ofereçam SLA de 99.99%, um datacenter local elimina a dependência de conectividade internacional — se o link transatlântico degradar, a operação da fazenda não é impactada.

**4. Análise Custo-Benefício:**  
A diferença de ~$5,72/mês (~R$29,00/mês) é **negligenciável** diante dos riscos de:
- Latência inadequada para decisões em tempo real
- Exposição regulatória
- Dependência de infraestrutura internacional para operações críticas

---

### 📝 Resumo da Decisão

| Critério | São Paulo (BR) | Virgínia do Norte (EUA) | Recomendado |
|---|:---:|:---:|:---:|
| Custo mensal | $17,31 | $11,59 | 🇺🇸 EUA |
| Latência para sensores no BR | ✅ Baixa (~10ms) | ❌ Alta (~150ms) | 🇧🇷 BR |
| Conformidade LGPD | ✅ Facilitada | ⚠️ Requer contratos específicos | 🇧🇷 BR |
| Soberania dos dados | ✅ Total | ❌ Jurisdição EUA | 🇧🇷 BR |
| Escalabilidade | ✅ | ✅ | Empate |
| **DECISÃO FINAL** | ⭐ **ESCOLHIDA** | Alternativa p/ dev | **🇧🇷 São Paulo** |

> **Conclusão:** Para um sistema de produção que recebe dados de sensores em tempo real de uma fazenda no Brasil e opera sob restrições legais de soberania de dados, **São Paulo (sa-east-1) é a escolha correta**, mesmo com custo superior. A economia de ~R$29/mês na Virgínia não justifica os riscos de latência, compliance e soberania de dados.

---

## 🎥 Vídeo Demonstrativo — Entrega 2 (AWS)

> **📹 Link do vídeo (YouTube — não listado):**  
> *(Inserir link após gravação)*

---

## 🛠️ Como Executar o Notebook

### Pré-requisitos

```bash
# Python 3.8+
pip install pandas numpy matplotlib seaborn scikit-learn scipy
```

### Execução

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/fase5farmtech.git
cd fase5farmtech

# Execute o Jupyter
jupyter notebook SidneyCardoso_rm567808_pbl_fase5.ipynb
```

> **Importante:** Execute as células em ordem sequencial, pois cada seção depende das variáveis definidas nas anteriores.

---

## 📊 Dataset

**Arquivo:** `arquivo/crop_yield.csv`  
**Registros:** 156 linhas  
**Features:**

| Coluna | Tipo | Descrição |
|---|---|---|
| `Crop` | Categórico | Nome da cultura agrícola |
| `Precipitation (mm day-1)` | Numérico | Precipitação em mm/dia |
| `Specific Humidity at 2 Meters (g/kg)` | Numérico | Umidade específica a 2m de altura |
| `Relative Humidity at 2 Meters (%)` | Numérico | Umidade relativa a 2m de altura |
| `Temperature at 2 Meters (C)` | Numérico | Temperatura a 2m de altura (°C) |
| `Yield` | Numérico | **Target:** Rendimento da safra |

---

## 👥 Equipe

**FarmTech Solutions — FIAP Pós-Tech AI for Devs**  
*Fase 5 — Machine Learning na Cabeça*

---

## 📄 Licença

Projeto acadêmico — FIAP 2025/2026. Repositório público para fins de avaliação.
