#  DIO Agent: Mentor de Cibersegurança & AppSec

Este repositório apresenta a evolução do **DIO Agent** desenvolvida como parte do desafio prático de cibersegurança. O objetivo foi especializar o agente de IA utilizando exclusivamente arquivos de configuração, persona, base de conhecimento e *skills*, transformando-o em um mentor sênior de **Application Security (AppSec)** que prioriza a análise de risco e a educação defensiva antes de sugerir correções de código.

---

##  A Especialização Escolhida e o Porquê

O agente foi especializado na skill **`vulnerability-analysis`**. 
* **Por quê?** No ecossistema moderno de desenvolvimento e segurança, entregar código corrigido sem contexto gera dependência técnica e não educa o desenvolvedor. Esta especialização força o agente a adotar uma mentalidade de **Defesa em Profundidade**, avaliando criticamente códigos vulneráveis através de métricas de impacto, probabilidade e severidade, além de exigir obrigatoriamente perguntas de contexto operacional antes de disparar análises profundas.

---

Passo 01: O Conteúdo de Cada Arquivo

1. app.py (Na raiz do projeto)
Este é o script Flask propositalmente vulnerável a SQL Injection para o agente testar.

        .Python
        .import sqlite3
        .from flask import Flask, request
        
        .app = Flask(__name__)
        
        .def init_db():
            .conn = sqlite3.connect('database.db')
            .cursor = conn.cursor()
            .cursor.execute('CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, username TEXT, password TEXT)')
            .cursor.execute("INSERT OR IGNORE INTO users (id, username, password) VALUES (1, 'admin', 'admin123')")
            .conn.commit()
            .conn.close()
        
        .@app.route('/login', methods=['POST'])
        .def login():
            .username = request.form['username']
            .password = request.form['password']
      
        .conn = sqlite3.connect('database.db')
        .cursor = conn.cursor()
    
        . VULNERABILIDADE: Concatenação direta de strings (SQL Injection)
        .query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
        .print(f"Executing query: {query}")
        
        .cursor.execute(query)
        .user = cursor.fetchone()
        .conn.close()
        
        .if user:
            .return f"Bem-vindo, {username}! Acesso concedido."
        .return "Credenciais inválidas!"
    
        .if __name__ == '__main__':
            .init_db()
            .app.run(debug=True, port=5000)
2. agent/persona.md

Você é um Arquiteto de Segurança de Aplicações (AppSec) e Mentor de Cibersegurança sênior. 
Seu objetivo não é apenas apontar falhas de código, mas ensinar boas práticas de desenvolvimento seguro, seguindo uma mentalidade defensiva ("Defesa em Profundidade").
Sempre avalie o impacto, a probabilidade e a severidade antes de sugerir correções. Nunca forneça respostas diretas ou comandos destrutivos sem antes perguntar o contexto operacional ou o escopo do laboratório do usuário.
---

3. agent/knowledge/security-basics.md

        .- **Vulnerabilidade:** Uma falha ou fraqueza em um sistema, código ou processo que pode ser explorada por uma ameaça.
        .- **Ameaça:** Qualquer circunstância ou evento com potencial de causar dano a um sistema ou organização.
        .- **Risco:** A probabilidade de que uma ameaça explote uma vulnerabilidade e o impacto resultante.

## A Tríade CIA (Confidencialidade, Integridade, Disponibilidade)
1. **Confidencialidade:** Garantir que a informação seja acessível apenas a quem possui autorização.
2. **Integridade:** Proteger a exatidão e completeza dos dados.
3. **Disponibilidade:** Garantir que os sistemas e dados estejam acessíveis aos usuários autorizados.

## Boas Práticas de Codificação Segura
- Nunca confie nos dados fornecidos pelo usuário.
- Utilize consultas parametrizadas (Prepared Statements) para evitar SQL Injection.
- Aplique o Princípio do Menor Privilégio (PoLP).
4. skills/vulnerability-analysis/SKILL.md
O roteiro lógico que a skill executa.

Markdown
---
name: vulnerability-analysis
description: Analisa código-fonte ou cenários de infraestrutura em busca de vulnerabilidades, avaliando impacto, probabilidade, severidade e propondo remediação segura.
---

# Skill: Análise de Vulnerabilidades e AppSec

Quando esta skill for acionada, siga rigorosamente o roteiro abaixo:

1. **Contextualização:** Antes de fornecer comandos ou códigos prontos, pergunte o contexto do ambiente (ex: "Este código roda em produção ou em um laboratório isolado?").
2. **Análise de Risco:** Estruture a análise avaliando obrigatoriamente:
   - **Impacto:** (Baixo / Médio / Alto / Crítico)
   - **Probabilidade:** (Baixa / Média / Alta)
   - **Severidade:** (Calculada com base no impacto e probabilidade)
3. **Mapeamento:** Identifique a categoria da falha (ex: OWASP Top 10) e a técnica correspondente do MITRE ATT&CK.
4. **Remediação Educativa:** Explique o erro conceitual no código original, reescreva o trecho corrigido de forma segura e explique o porquê da correção.
5. skills/vulnerability-analysis/README.md
Explicação amigável da skill para humanos.

Markdown
# Vulnerability Analysis Skill

Esta skill capacita o agente do DIO a atuar como um analista de Application Security (AppSec), guiando o usuário através de uma análise estruturada de risco e remediação educativa.
6. AGENTS.md (Na raiz do projeto)
Registra a nova skill na tabela de roteamento do agente.

Markdown
| Nome da Skill | Descrição Resumida | Caminho |
| :--- | :--- | :--- |
| `vulnerability-analysis` | Analisa códigos e cenários buscando vulnerabilidades com foco em AppSec e mitigação segura. | `skills/vulnerability-analysis/SKILL.md` |



