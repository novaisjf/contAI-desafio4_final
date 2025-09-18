# 🍽️ Sistema de Automação VR/VA

Sistema completo para automatização do processo mensal de compra de Vale Refeição (VR) e Vale Alimentação (VA), com interface Streamlit e assistente IA integrado.

## 📋 Visão Geral

Este sistema automatiza todo o processo de cálculo de benefícios VR/VA, desde a consolidação de bases de dados até a geração do relatório final para envio à operadora, respeitando todas as regras de negócio e acordos coletivos.

### ✨ Principais Funcionalidades

- 🔄 **Consolidação Automática**: Unifica 5 bases separadas em uma base única
- 🚫 **Exclusões Inteligentes**: Remove automaticamente diretores, estagiários, afastados, etc.
- ✅ **Validações Completas**: Verifica datas, campos obrigatórios, feriados e inconsistências
- 📊 **Cálculo Automatizado**: Calcula dias úteis considerando férias, desligamentos e sindicatos
- 💰 **Rateio 80/20**: Aplica automaticamente o custo empresa (80%) e desconto funcionário (20%)
- 🤖 **IA Integrada**: Interface de chat com assistente especializado
- 📁 **Relatório Padronizado**: Gera planilha no formato exigido pela operadora

## 🎯 Bases de Dados Processadas

| Base           | Descrição                   | Campos Principais                        |
| -------------- | --------------------------- | ---------------------------------------- |
| **Ativos**     | Colaboradores ativos no mês | Matrícula, Nome, CPF, Cargo, Sindicato   |
| **Férias**     | Funcionários em férias      | Matrícula, Data Início, Data Fim, Tipo   |
| **Desligados** | Funcionários desligados     | Matrícula, Data Desligamento, Comunicado |
| **Cadastral**  | Admissões do mês            | Matrícula, Nome, Data Admissão, Cargo    |
| **Sindicatos** | Valores por sindicato       | Sindicato, Valor VR, Valor VA            |
| **Dias Úteis** | Calendário por colaborador  | Matrícula, Mês/Ano, Dias Úteis           |

## 🚀 Instalação e Configuração

### 1. Pré-requisitos

```bash
Python 3.8+
pip (gerenciador de pacotes Python)
```

### 2. Instalação

```bash
# Clone o repositório
git clone https://github.com/novaisjf/contAI-desafio4_final
cd sistema-contAI-vr-va

# Instale as dependências
pip install -r requirements.txt

# Configure o ambiente
cp .env.example .env
```

### 3. Configuração do .env

```env
# IA - Google Gemini
GOOGLE_API_KEY=sua_chave_gemini_aqui

# IA - LLaMA Local (opcional)
LLAMA_MODEL_PATH=models/llama-2-7b-chat.gguf

# Configurações gerais
LOG_LEVEL=INFO
ENVIRONMENT=production
```

### 4. Estrutura de Diretórios

```
sistema-vr-va/
├── app.py                 # Interface Streamlit principal
├── main.py               # Execução via linha de comando
├── config.yaml           # Configurações do sistema
├── requirements.txt      # Dependências
├── agents/               # Arquitetura de agentes
│   └── orchestrator_agent.py
├── documentos/           # Arquivos de entrada (criar)
├── output/              # Relatórios gerados (criado automaticamente)
├── logs/                # Logs do sistema (criado automaticamente)
└── backup/              # Backups (criado automaticamente)
```

## 💻 Como Usar

### Interface Streamlit (Recomendado)

```bash
streamlit run app.py
```

Acesse `http://localhost:8501` no navegador.

#### Modo Interface Gráfica

1. **Configure** o mês/ano de competência na barra lateral
2. **Faça upload** dos arquivos Excel necessários
3. **Ajuste** as regras de negócio (desligamento, validações)
4. **Execute** o processamento
5. **Baixe** o relatório gerado

#### Modo Chat IA

1. **Configure** sua chave de API (Gemini ou LLaMA)
2. **Faça upload** dos arquivos Excel
3. **Converse** com o assistente: "Calcular VR para maio/2024"
4. **Acompanhe** o processamento em tempo real
5. **Baixe** o resultado final

### Linha de Comando

```bash
python main.py -c 2024-05-01 -i documentos -o output
```

Parâmetros:

- `-c, --competencia`: Data de competência (YYYY-MM-DD)
- `-i, --input`: Diretório de entrada (padrão: documentos)
- `-o, --output`: Diretório de saída (padrão: output)

## 🔧 Configurações Avançadas

### Regras de Desligamento

**Proporcional (Recomendado):**

- Comunicado até dia 15: Não paga VR
- Comunicado após dia 15: Paga proporcional aos dias trabalhados

**Integral:**

- Sempre paga o mês completo
- Ajustes são feitos na rescisão

### Exclusões Automáticas

**Por Cargo:**

- Diretores
- Estagiários
- Aprendizes
- Terceirizados

**Por Situação:**

- Afastados
- Licença maternidade/paternidade
- Funcionários no exterior
- Suspensos

### Validações Aplicadas

✅ **Datas:** Formato, consistência, períodos válidos  
✅ **CPF:** Formato e dígitos verificadores  
✅ **Campos:** Obrigatórios por base  
✅ **Valores:** Faixas mínimas e máximas  
✅ **Duplicatas:** Matrículas repetidas  
✅ **Feriados:** Estaduais e municipais

## 📊 Relatório Final

O sistema gera uma planilha Excel com:

| Coluna            | Descrição                  |
| ----------------- | -------------------------- |
| MATRICULA         | Matrícula do funcionário   |
| NOME              | Nome completo              |
| CPF               | CPF formatado              |
| CARGO             | Cargo atual                |
| SINDICATO         | Sindicato vinculado        |
| DIAS_UTEIS        | Dias úteis no mês          |
| VALOR_UNITARIO_VR | Valor unitário do VR       |
| VR_TOTAL          | Valor total do benefício   |
| EMPRESA_80        | Custo para empresa (80%)   |
| FUNCIONARIO_20    | Desconto funcionário (20%) |

## 🤖 Assistente IA

### Comandos Disponíveis

```
"Calcular VR para maio/2024"
"Processar competência 05/2024 com regra proporcional"
"Gerar relatório para junho de 2024"
"Aplicar validações completas para abril/2024"
```

### Provedores Suportados

**Google Gemini (Recomendado):**

- Rápido e preciso
- Suporte nativo ao português
- Requer chave de API

**LLaMA Local:**

- Execução offline
- Privacidade total dos dados
- Requer modelo baixado localmente

## 📋 Troubleshooting

### Problemas Comuns

**❌ Erro: "Chave de API não encontrada"**

```bash
# Solução: Configure a chave no .env ou na interface
export GOOGLE_API_KEY="sua_chave_aqui"
```

**❌ Erro: "Arquivo não encontrado"**

```bash
# Solução: Verifique se os arquivos estão no diretório correto
ls documentos/
```

**❌ Erro: "Modelo LLaMA não encontrado"**

```bash
# Solução: Baixe o modelo ou ajuste o caminho
mkdir models
# Baixe o modelo para models/
```

**❌ Erro: "Memória insuficiente"**

```bash
# Solução: Processe arquivos menores ou aumente a RAM
# Configure no config.yaml: max_workers: 2
```

### Logs e Debugging

```bash
# Logs detalhados
tail -f logs/sistema_vr_va.log

# Debug no Streamlit
streamlit run app.py --logger.level debug
```

## 📈 Performance

### Benchmarks Típicos

| Cenário      | Funcionários | Tempo Médio | Memória |
| ------------ | ------------ | ----------- | ------- |
| Pequeno      | < 100        | 30s         | 512MB   |
| Médio        | 100-1000     | 2min        | 1GB     |
| Grande       | 1000-5000    | 5min        | 2GB     |
| Muito Grande | > 5000       | 10min+      | 4GB+    |

### Otimizações

- Use arquivos .xlsx ao invés de .xls
- Remova colunas desnecessárias dos arquivos de entrada
- Configure `max_workers` no config.yaml conforme sua CPU
- Use SSD para melhor I/O

## 🔒 Segurança

- ✅ **Dados Locais**: Processamento 100% local
- ✅ **Backup Automático**: Backup antes do processamento
- ✅ **Logs de Auditoria**: Rastreamento completo das operações
- ✅ **Sanitização**: Limpeza automática de dados de entrada
- ✅ **Validação**: Múltiplas camadas de validação

## 🆙 Atualizações

### Versão 2.0 (Atual)

- ✨ Interface Streamlit moderna
- 🤖 Assistente IA integrado
- 🔧 Suporte a múltiplos LLMs
- 📊 Dashboard de resultados
- ⚡ Performance otimizada

### Roadmap v2.1

- 📱 Interface mobile responsiva
- 🔗 Integração com APIs de RH
- 📧 Notificações por email
- 🌐 Deploy na nuvem
- 📊 Relatórios avançados

## 🆘 Suporte

- 📧 **Email:** suporte@empresa.com
- 📖 **Documentação:** [docs.empresa.com/vr-va](https://docs.empresa.com/vr-va)
- 🐛 **Issues:** [GitHub Issues](https://github.com/empresa/sistema-vr-va/issues)
- 💬 **Chat:** Slack #sistema-vr-va

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

---

**Desenvolvido com ❤️ pela equipe de RH Digital**

## 📚 Apêndices

### A. Exemplo de Arquivos de Entrada

#### Arquivo "Ativos.xlsx"

```
MATRICULA | NOME           | CPF           | CARGO      | SINDICATO | DATA_ADMISSAO | STATUS
12345     | João Silva     | 111.222.333-44| ANALISTA   | SINDICA_A | 2023-01-15    | ATIVO
12346     | Maria Santos   | 222.333.444-55| ASSISTENTE | SINDICA_B | 2023-02-20    | ATIVO
```

#### Arquivo "Ferias.xlsx"

```
MATRICULA | DATA_INICIO | DATA_FIM   | TIPO_FERIAS
12345     | 2024-05-10  | 2024-05-20 | PARCIAL
12347     | 2024-05-01  | 2024-05-30 | INTEGRAL
```

#### Arquivo "Desligados.xlsx"

```
MATRICULA | DATA_DESLIGAMENTO | MOTIVO    | COMUNICADO_ATE_DIA15
12348     | 2024-05-10        | DEMISSAO  | SIM
12349     | 2024-05-25        | PEDIDO    | NAO
```

### B. Estrutura do Arquivo de Saída

#### "VR MENSAL 05.2024.xlsx"

```
MATRICULA | NOME         | CPF           | SINDICATO | DIAS_UTEIS | VALOR_UNITARIO | VR_TOTAL | EMPRESA_80 | FUNCIONARIO_20
12345     | João Silva   | 111.222.333-44| SINDICA_A | 22         | 25.00         | 550.00   | 440.00     | 110.00
12346     | Maria Santos | 222.333.444-55| SINDICA_B | 20         | 30.00         | 600.00   | 480.00     | 120.00
```

### C. Códigos de Status

| Código | Descrição                           | Ação              |
| ------ | ----------------------------------- | ----------------- |
| ✅ 200 | Processamento concluído com sucesso | Arquivo gerado    |
| ⚠️ 201 | Processado com avisos               | Verificar logs    |
| ❌ 400 | Erro nos dados de entrada           | Corrigir arquivos |
| ❌ 404 | Arquivos não encontrados            | Verificar upload  |
| ❌ 500 | Erro interno do sistema             | Contatar suporte  |

### D. Configurações por Sindicato

#### Exemplo de Configuração Avançada

```yaml
sindicatos:
  COMERCIARIOS_SP:
    valor_vr: 28.50
    valor_va: 22.00
    regras_especiais:
      ferias_integral: false
      desconto_domingos: true
      dias_uteis_mes: 22

  METALURGICOS_ABC:
    valor_vr: 32.00
    valor_va: 25.00
    regras_especiais:
      ferias_integral: true
      desconto_domingos: false
      dias_uteis_mes: 20
```

### E. Fórmulas de Cálculo

#### Cálculo Básico de VR

```
VR_TOTAL = DIAS_UTEIS × VALOR_UNITARIO_VR
EMPRESA_80 = VR_TOTAL × 0.80
FUNCIONARIO_20 = VR_TOTAL × 0.20
```

#### Cálculo com Desligamento Proporcional

```
Se COMUNICADO_ATE_DIA15 = "SIM":
    VR_TOTAL = 0

Se COMUNICADO_ATE_DIA15 = "NAO" e DATA_DESLIGAMENTO > 15:
    DIAS_PROPORCIONAIS = DATA_DESLIGAMENTO - 1
    VR_TOTAL = DIAS_PROPORCIONAIS × VALOR_UNITARIO_VR
```

#### Cálculo com Férias

```
Se TIPO_FERIAS = "INTEGRAL":
    DIAS_UTEIS = 0

Se TIPO_FERIAS = "PARCIAL":
    DIAS_DESCONTO = DIAS_FERIAS_UTEIS
    DIAS_UTEIS = DIAS_UTEIS_MES - DIAS_DESCONTO
```

### F. Checklist de Validação

#### Antes do Processamento

- [ ] Todos os 6 arquivos foram carregados?
- [ ] Competência está correta?
- [ ] Regras de desligamento definidas?
- [ ] Chave de API configurada (se usar IA)?

#### Durante o Processamento

- [ ] Logs estão sendo gerados?
- [ ] Nenhum erro crítico reportado?
- [ ] Progress bar avançando normalmente?

#### Após o Processamento

- [ ] Arquivo Excel foi gerado?
- [ ] Totais estão coerentes?
- [ ] Número de beneficiados está correto?
- [ ] Exclusões foram aplicadas?

### G. Troubleshooting Avançado

#### Erro de Memória

```python
# Reduzir workers no config.yaml
processamento:
  max_workers: 1  # Ao invés de 4

# Ou processar em lotes menores
python main.py -c 2024-05-01 --batch-size 500
```

#### Erro de Encoding

```python
# Salvar arquivos Excel com UTF-8
pd.read_excel(arquivo, encoding='utf-8-sig')
```

#### Erro de Data

```python
# Formato aceito: YYYY-MM-DD
# Converter: DD/MM/YYYY -> YYYY-MM-DD
pd.to_datetime(data, format='%d/%m/%Y').strftime('%Y-%m-%d')
```

### H. Integrações Futuras

#### API REST (Planejado v2.1)

```bash
# Endpoint para cálculo via API
POST /api/v1/calcular-vr
Content-Type: application/json

{
  "competencia": "2024-05-01",
  "arquivos": ["base64_encoded_files"],
  "config": {
    "regra_desligamento": "proporcional"
  }
}
```

### I. Backup e Recuperação

#### Backup Automático

O sistema cria backups automáticos em:

```
backup/
├── 2024-05-01_12-30-45/
│   ├── arquivos_originais/
│   ├── config_usado.yaml
│   └── resultado_final.xlsx
```

#### Recuperação de Dados

```bash
# Restaurar processamento anterior
python main.py --restore backup/2024-05-01_12-30-45/

# Reprocessar com nova configuração
python main.py -c 2024-05-01 --config backup/2024-05-01_12-30-45/config_usado.yaml
```

### J. Monitoramento e Métricas

#### Métricas Coletadas

- Tempo de processamento por etapa
- Número de registros processados
- Número de exclusões por categoria
- Uso de memória e CPU
- Erros e avisos por tipo

#### Dashboard de Monitoramento

```python
# Visualização em tempo real
streamlit run app.py
```

### K. Contribuição e Desenvolvimento

#### Setup de Desenvolvimento

```bash
# Instalar dependências de desenvolvimento
pip install -r requirements.txt

# Executar testes
pytest tests/

# Formatação de código
black .
flake8 .

# Type checking
mypy .
```

#### Estrutura de Testes

```
tests/
├── unit/               # Testes unitários
├── integration/        # Testes de integração
├── fixtures/          # Dados de teste
└── conftest.py        # Configuração dos testes
```

#### Pull Request Guidelines

1. Fork do repositório
2. Criar branch feature/nova-funcionalidade
3. Implementar com testes
4. Documentação atualizada
5. PR com descrição detalhada

---

**🎯 Sistema VR/VA v2.0** - Automatização completa e inteligente do seu processo de benefícios!
