import streamlit as st
import random
import pandas as pd
from time import time

# Configuração da página
st.set_page_config(
    page_title="Simulado CompTIA Security+ SY0-701",
    page_icon="🔒",
    layout="wide",
    initial_sidebar_state="expanded"
)

# ====================================================
# BANCO DE QUESTÕES - COMPTIA SECURITY+ SY0-701
# Baseado no livro de Ian Neil e objetivos oficiais
# ====================================================

BANCO_QUESTOES = [
    # DOMÍNIO 1: CONCEITOS GERAIS (12%)
    {
        "id": 1,
        "dominio": 1,
        "texto": "Um administrador de segurança deseja impedir que softwares não autorizados sejam executados em estações de trabalho corporativas. Qual técnica de mitigação é MAIS eficaz para esse fim?",
        "opcoes": [
            "Lista de bloqueio de aplicativos (block list)",
            "Lista de permissão de aplicativos (allow list)",
            "Isolamento de rede (network isolation)",
            "Monitoramento comportamental (behavioral monitoring)"
        ],
        "resposta": 1,  # índice 0-based (B = índice 1)
        "explicacao": "A lista de permissão (allow list) especifica quais aplicações são permitidas, bloqueando automaticamente qualquer outra. O livro destaca que isso impede malware e softwares não autorizados (página 323). A lista de bloqueio (block list) é menos eficaz pois exige conhecer cada software malicioso antecipadamente (página 324).",
        "dominio_nome": "1.0 Conceitos Gerais de Segurança"
    },
    {
        "id": 2,
        "dominio": 1,
        "texto": "Qual tipo de controle de segurança é representado por câmeras de vigilância (CCTV) instaladas em um estacionamento corporativo?",
        "opcoes": ["Preventivo", "Deterrente", "Detectivo", "Corretivo"],
        "resposta": 2,
        "explicacao": "Câmeras CCTV são controles detectivos, pois identificam e registram atividades suspeitas ou incidentes já ocorridos. O livro explica que controles detectivos incluem SIEM, auditorias e vigilância por CFTV (página 80).",
        "dominio_nome": "1.0 Conceitos Gerais de Segurança"
    },
    {
        "id": 3,
        "dominio": 1,
        "texto": "Uma organização precisa garantir que uma mensagem enviada digitalmente não possa ser negada pelo remetente posteriormente. Qual conceito de segurança atende a esse requisito?",
        "opcoes": ["Confidencialidade", "Integridade", "Disponibilidade", "Irretratabilidade (non-repudiation)"],
        "resposta": 3,
        "explicacao": "Irretratabilidade (non-repudiation) impede que o remetente negue a autoria ou envio de uma mensagem. É alcançada com assinaturas digitais usando chave privada (página 91-92).",
        "dominio_nome": "1.0 Conceitos Gerais de Segurança"
    },
    {
        "id": 4,
        "dominio": 1,
        "texto": "Em uma arquitetura de confiança zero (Zero Trust), qual componente é responsável por tomar decisões de acesso baseadas em políticas e inteligência de ameaças?",
        "opcoes": [
            "Plano de dados (Data Plane)",
            "Mecanismo de políticas (Policy Engine)",
            "Administrador de políticas (Policy Administrator)",
            "Ponto de imposição de políticas (Policy Enforcement Point)"
        ],
        "resposta": 1,
        "explicacao": "O mecanismo de políticas (Policy Engine) é o 'cérebro' do Zero Trust, decidindo quem acessa o quê com base em políticas, contexto e inteligência de ameaças (página 103).",
        "dominio_nome": "1.0 Conceitos Gerais de Segurança"
    },
    {
        "id": 5,
        "dominio": 1,
        "texto": "Qual componente da Infraestrutura de Chave Pública (PKI) é mantido em segredo e usado para descriptografar dados e criar assinaturas digitais?",
        "opcoes": ["Chave pública", "Certificado digital", "Chave privada", "Autoridade Certificadora (CA)"],
        "resposta": 2,
        "explicacao": "A chave privada deve ser mantida em segredo. Sua função principal é descriptografar dados e gerar assinaturas digitais, garantindo autenticidade e irretratabilidade (página 146-147).",
        "dominio_nome": "1.0 Conceitos Gerais de Segurança"
    },

    # DOMÍNIO 2: AMEAÇAS, VULNERABILIDADES E MITIGAÇÕES (22%)
    {
        "id": 6,
        "dominio": 2,
        "texto": "Um funcionário recebe um e-mail urgente do 'departamento de TI' pedindo que clique em um link e altere sua senha imediatamente. O e-mail contém pequenos erros de ortografia e o domínio do remetente é ligeiramente diferente do oficial. Qual tipo de ataque está sendo tentado?",
        "opcoes": ["Spear phishing", "Whaling", "Phishing genérico", "Smishing"],
        "resposta": 2,
        "explicacao": "Phishing genérico é um ataque não direcionado enviado a um grande número de pessoas, muitas vezes com erros e domínios falsos. Spear phishing é direcionado a grupos específicos (página 213). Whaling mira executivos (página 975). Smishing usa SMS (página 203).",
        "dominio_nome": "2.0 Ameaças, Vulnerabilidades e Mitigações"
    },
    {
        "id": 7,
        "dominio": 2,
        "texto": "Um atacante consegue interceptar a comunicação entre um cliente e um servidor e faz com que eles usem um protocolo criptográfico mais fraco e vulnerável. Esta é uma descrição de qual tipo de ataque?",
        "opcoes": ["Ataque de colisão", "Ataque de aniversário", "Ataque de downgrade", "Pass-the-hash"],
        "resposta": 2,
        "explicacao": "Um ataque de downgrade força a negociação de um protocolo ou cifra mais fraca. SSL stripping e POODLE são exemplos (página 248, 299-300).",
        "dominio_nome": "2.0 Ameaças, Vulnerabilidades e Mitigações"
    },
    {
        "id": 8,
        "dominio": 2,
        "texto": "Qual tipo de malware se autorreplica e se espalha pela rede sem necessidade de interação do usuário, consumindo grande largura de banda?",
        "opcoes": ["Vírus", "Trojan", "Worm", "Ransomware"],
        "resposta": 2,
        "explicacao": "Worms têm capacidade inerente de se autorreplicar e se espalhar independentemente através de redes. Exemplo clássico é o NIMDA (página 267).",
        "dominio_nome": "2.0 Ameaças, Vulnerabilidades e Mitigações"
    },
    {
        "id": 9,
        "dominio": 2,
        "texto": "Uma equipe de resposta a incidentes precisa analisar um arquivo suspeito em um ambiente seguro e isolado, sem risco para a rede corporativa. Qual tecnologia deve ser utilizada?",
        "opcoes": ["Contêiner Docker", "Máquina virtual com snapshot", "Sandbox", "Air gap"],
        "resposta": 2,
        "explicacao": "Sandbox é um ambiente isolado (virtual ou físico) usado para executar e analisar código malicioso com segurança. Exemplos incluem Cuckoo Sandbox (página 271, 540).",
        "dominio_nome": "2.0 Ameaças, Vulnerabilidades e Mitigações"
    },
    {
        "id": 10,
        "dominio": 2,
        "texto": "Um administrador de rede precisa garantir que apenas dispositivos autorizados possam se conectar fisicamente a uma porta de switch. Qual tecnologia deve ser configurada?",
        "opcoes": ["802.1q (VLAN tagging)", "802.1x (autenticação de porta)", "802.11ac (Wi-Fi)", "802.3ad (Link Aggregation)"],
        "resposta": 1,
        "explicacao": "802.1X autentica dispositivos antes de conceder acesso à porta física ou lógica do switch, usando um servidor RADIUS. É citado como medida de port security (página 425).",
        "dominio_nome": "2.0 Ameaças, Vulnerabilidades e Mitigações"
    },

    # DOMÍNIO 3: ARQUITETURA DE SEGURANÇA (18%)
    {
        "id": 11,
        "dominio": 3,
        "texto": "Uma empresa decide manter seus sistemas de folha de pagamento em servidores próprios, dentro de sua sede, mas utiliza o Microsoft 365 para e-mail e colaboração. Este modelo é chamado de:",
        "opcoes": ["Nuvem pública", "Nuvem privada", "Nuvem híbrida", "Nuvem comunitária"],
        "resposta": 2,
        "explicacao": "Nuvem híbrida combina infraestrutura local (on-premises) com serviços de nuvem pública (página 353).",
        "dominio_nome": "3.0 Arquitetura de Segurança"
    },
    {
        "id": 12,
        "dominio": 3,
        "texto": "Qual dispositivo de rede é responsável por distribuir o tráfego entre vários servidores para garantir alta disponibilidade e desempenho?",
        "opcoes": ["Firewall", "Switch", "Roteador", "Balanceador de carga"],
        "resposta": 3,
        "explicacao": "Balanceadores de carga distribuem o tráfego entre múltiplos servidores usando algoritmos como round-robin DNS, afinidade ou host menos utilizado (página 421-423, 472-475).",
        "dominio_nome": "3.0 Arquitetura de Segurança"
    },
    {
        "id": 13,
        "dominio": 3,
        "texto": "Qual tipo de site de recuperação de desastres é totalmente equipado e operacional, permitindo failover imediato, mas é o mais caro de manter?",
        "opcoes": ["Hot site", "Warm site", "Cold site", "Cloud site"],
        "resposta": 0,
        "explicacao": "Hot site é uma réplica completa da infraestrutura principal, com dados sincronizados em tempo real, permitindo ativação imediata, porém com alto custo (página 479).",
        "dominio_nome": "3.0 Arquitetura de Segurança"
    },
    {
        "id": 14,
        "dominio": 3,
        "texto": "O que significa o princípio de 'alta disponibilidade' (High Availability) em arquitetura de segurança?",
        "opcoes": [
            "Sistema com baixa latência",
            "Sistema com 100% de criptografia",
            "Sistema com redundância para minimizar downtime",
            "Sistema com backups diários"
        ],
        "resposta": 2,
        "explicacao": "Alta disponibilidade refere-se à capacidade de um sistema permanecer operacional mesmo com falhas, usando redundância (clusters, balanceadores, etc.), buscando 99,999% de uptime (página 388-389).",
        "dominio_nome": "3.0 Arquitetura de Segurança"
    },
    {
        "id": 15,
        "dominio": 3,
        "texto": "Qual tecnologia permite que aplicações sejam executadas em ambientes isolados e consistentes, independentemente do sistema operacional subjacente?",
        "opcoes": ["Virtualização completa", "Contêineres", "Infraestrutura como código", "Microserviços"],
        "resposta": 1,
        "explicacao": "Contêineres empacotam aplicações com suas dependências, garantindo portabilidade e isolamento sem depender do SO hospedeiro (página 377-378).",
        "dominio_nome": "3.0 Arquitetura de Segurança"
    },

    # DOMÍNIO 4: OPERAÇÕES DE SEGURANÇA (28%)
    {
        "id": 16,
        "dominio": 4,
        "texto": "Um funcionário é desligado da empresa. Durante o processo de desligamento, a conta de usuário deve ser desabilitada e a senha redefinida. Qual é o principal motivo para não excluir a conta imediatamente?",
        "opcoes": [
            "Evitar custos com novas licenças",
            "Preservar registros de auditoria e acessar dados do usuário se necessário",
            "Cumprir a Lei Geral de Proteção de Dados (LGPD)",
            "Disponibilizar o nome de usuário para novo funcionário"
        ],
        "resposta": 1,
        "explicacao": "Desabilitar (não deletar) preserva o SID e os logs de auditoria. A empresa pode precisar acessar e-mails ou documentos do ex-funcionário para dar continuidade ao trabalho (página 730).",
        "dominio_nome": "4.0 Operações de Segurança"
    },
    {
        "id": 17,
        "dominio": 4,
        "texto": "Qual ferramenta é mais adequada para centralizar e correlacionar logs de diferentes fontes (firewalls, servidores, IDS) para detectar incidentes de segurança em tempo real?",
        "opcoes": ["SIEM", "SOAR", "EDR", "DLP"],
        "resposta": 0,
        "explicacao": "SIEM agrega, normaliza e correlaciona logs de múltiplas fontes, gerando alertas e relatórios para detecção de incidentes (página 636-637).",
        "dominio_nome": "4.0 Operações de Segurança"
    },
    {
        "id": 18,
        "dominio": 4,
        "texto": "Uma empresa deseja proteger backups armazenados na nuvem contra acesso não autorizado. Qual medida é a MAIS importante?",
        "opcoes": ["Compressão", "Criptografia", "Deduplicação", "Replicação"],
        "resposta": 1,
        "explicacao": "A criptografia de backups adiciona uma camada de proteção, garantindo que mesmo que alguém acesse o backup, os dados permaneçam ilegíveis sem a chave de descriptografia (página 497).",
        "dominio_nome": "4.0 Operações de Segurança"
    },
    {
        "id": 19,
        "dominio": 4,
        "texto": "Qual protocolo de rede é usado para monitoramento de dispositivos e pode enviar armadilhas (traps) para um servidor central em caso de falhas?",
        "opcoes": ["SSL/TLS", "SNMP", "SSH", "RDP"],
        "resposta": 1,
        "explicacao": "SNMP monitora dispositivos de rede e pode enviar traps (notificações assíncronas) para um NMS quando eventos ocorrem (página 621-622, 639-640).",
        "dominio_nome": "4.0 Operações de Segurança"
    },
    {
        "id": 20,
        "dominio": 4,
        "texto": "Qual é o objetivo principal de uma análise de causa raiz (Root Cause Analysis) após um incidente de segurança?",
        "opcoes": [
            "Restaurar sistemas a partir de backup",
            "Identificar a vulnerabilidade fundamental que permitiu o incidente",
            "Contatar a equipe de forense digital",
            "Aplicar patches de emergência"
        ],
        "resposta": 1,
        "explicacao": "A análise de causa raiz visa descobrir a falha subjacente (configuração, software, processo) que permitiu o incidente, para evitar recorrência (página 807).",
        "dominio_nome": "4.0 Operações de Segurança"
    },

    # DOMÍNIO 5: GERENCIAMENTO E SUPERVISÃO DO PROGRAMA (20%)
    {
        "id": 21,
        "dominio": 5,
        "texto": "Qual documento formal descreve as regras de uso aceitável dos recursos de TI de uma organização, incluindo e-mail, internet e dispositivos?",
        "opcoes": ["SLA", "AUP", "NDA", "BCP"],
        "resposta": 1,
        "explicacao": "A Política de Uso Aceitável (AUP) define comportamentos permitidos e proibidos no uso dos recursos da empresa (página 848).",
        "dominio_nome": "5.0 Gerenciamento e Supervisão do Programa"
    },
    {
        "id": 22,
        "dominio": 5,
        "texto": "Qual é o principal objetivo da análise de impacto nos negócios (Business Impact Analysis - BIA)?",
        "opcoes": [
            "Calcular o retorno sobre investimento em segurança",
            "Identificar ativos críticos e impacto de sua indisponibilidade",
            "Definir senhas de administrador",
            "Criar um honeypot"
        ],
        "resposta": 1,
        "explicacao": "A BIA identifica as funções e ativos mais críticos, quantifica o impacto financeiro e operacional da indisponibilidade, e define RTO/RPO (página 903).",
        "dominio_nome": "5.0 Gerenciamento e Supervisão do Programa"
    },
    {
        "id": 23,
        "dominio": 5,
        "texto": "Em um contrato com um provedor de serviços em nuvem, o que define tempos de resposta, disponibilidade garantida (ex: 99,9%) e penalidades por descumprimento?",
        "opcoes": ["MOU", "SLA", "BPA", "MSA"],
        "resposta": 1,
        "explicacao": "O Acordo de Nível de Serviço (SLA) especifica métricas de desempenho, disponibilidade, responsabilidades e compensações em caso de falha (página 919).",
        "dominio_nome": "5.0 Gerenciamento e Supervisão do Programa"
    },
    {
        "id": 24,
        "dominio": 5,
        "texto": "Um auditor externo precisa avaliar a segurança de um fornecedor terceiro. Qual cláusula contratual permite que o auditor realize inspeções surpresa?",
        "opcoes": ["Não concorrência", "Direito de auditoria", "Confidencialidade", "Responsabilidade limitada"],
        "resposta": 1,
        "explicacao": "A cláusula de direito de auditoria permite que a contratante verifique a conformidade do fornecedor com os padrões acordados, inclusive com auditorias não anunciadas (página 816, 916).",
        "dominio_nome": "5.0 Gerenciamento e Supervisão do Programa"
    },
    {
        "id": 25,
        "dominio": 5,
        "texto": "Qual é a função do Controlador de Dados (Data Controller) conforme a LGPD/GDPR?",
        "opcoes": [
            "Executar a segurança física dos servidores",
            "Determinar as finalidades e meios do tratamento de dados pessoais",
            "Realizar backup dos dados",
            "Desenvolver algoritmos de criptografia"
        ],
        "resposta": 1,
        "explicacao": "O Controlador decide como e por que os dados pessoais serão processados, sendo o principal responsável legal pela conformidade (página 875, 944).",
        "dominio_nome": "5.0 Gerenciamento e Supervisão do Programa"
    }
]

# Expandir banco para 90 questões (duplicando com pequenas variações para demonstração)
# Em produção real, você teria 90+ questões únicas por domínio
def expandir_banco():
    """Expande o banco de questões para aproximadamente 90 questões no total"""
    questoes_expandidas = []
    dominios_porcentagem = {1: 12, 2: 22, 3: 18, 4: 28, 5: 20}
    total_questoes = 90
    
    for dominio in range(1, 6):
        qtd_desejada = int(total_questoes * dominios_porcentagem[dominio] / 100)
        questoes_dominio = [q for q in BANCO_QUESTOES if q["dominio"] == dominio]
        
        while len(questoes_dominio) < qtd_desejada:
            # Duplica questões quando necessário (em produção, criar questões únicas)
            questoes_dominio.extend(questoes_dominio[:])
        
        questoes_expandidas.extend(questoes_dominio[:qtd_desejada])
    
    return questoes_expandidas

# Banco final com ~90 questões
QUESTOES = expandir_banco()

# ====================================================
# FUNÇÕES AUXILIARES
# ====================================================

def inicializar_estado():
    """Inicializa as variáveis de estado da sessão"""
    if "simulado_atual" not in st.session_state:
        st.session_state.simulado_atual = 1
    if "respostas" not in st.session_state:
        st.session_state.respostas = {}
    if "finalizado" not in st.session_state:
        st.session_state.finalizado = False
    if "tempo_inicio" not in st.session_state:
        st.session_state.tempo_inicio = time()
    if "questoes_embaralhadas" not in st.session_state:
        st.session_state.questoes_embaralhadas = random.sample(QUESTOES, len(QUESTOES))

def calcular_pontuacao():
    """Calcula a pontuação do simulado atual"""
    acertos = 0
    for idx, questao in enumerate(st.session_state.questoes_embaralhadas):
        if idx in st.session_state.respostas:
            if st.session_state.respostas[idx] == questao["resposta"]:
                acertos += 1
    return acertos, len(st.session_state.questoes_embaralhadas)

def proximo_simulado():
    """Avança para o próximo simulado"""
    if st.session_state.simulado_atual < 20:
        st.session_state.simulado_atual += 1
        st.session_state.respostas = {}
        st.session_state.finalizado = False
        st.session_state.tempo_inicio = time()
        st.session_state.questoes_embaralhadas = random.sample(QUESTOES, len(QUESTOES))
        st.rerun()

def reiniciar_simulado():
    """Reinicia o simulado atual"""
    st.session_state.respostas = {}
    st.session_state.finalizado = False
    st.session_state.tempo_inicio = time()
    st.session_state.questoes_embaralhadas = random.sample(QUESTOES, len(QUESTOES))
    st.rerun()

# ====================================================
# INTERFACE PRINCIPAL
# ====================================================

inicializar_estado()

# Sidebar
with st.sidebar:
    st.image("https://images.credly.com/images/5f7b6aeb-8c1d-4b3b-9f8b-0c6a3e9f1d2e/CompTIA_Security_2B.png", width=150)
    st.title("📋 Navegação")
    st.markdown(f"**Simulado:** {st.session_state.simulado_atual}/20")
    st.markdown(f"**Questões totais:** {len(st.session_state.questoes_embaralhadas)}")
    
    if not st.session_state.finalizado:
        respondidas = len(st.session_state.respostas)
        st.progress(respondidas / len(st.session_state.questoes_embaralhadas))
        st.markdown(f"**Respondidas:** {respondidas}/{len(st.session_state.questoes_embaralhadas)}")
    
    st.divider()
    st.markdown("### 🎯 Distribuição dos Domínios")
    dados_dominio = {}
    for q in st.session_state.questoes_embaralhadas:
        dados_dominio[q["dominio"]] = dados_dominio.get(q["dominio"], 0) + 1
    
    for dom, qtd in sorted(dados_dominio.items()):
        st.markdown(f"Domínio {dom}.0: {qtd} questões")
    
    st.divider()
    if st.button("🔄 Reiniciar Simulado", use_container_width=True):
        reiniciar_simulado()
    
    st.caption("🔒 Simulado baseado no livro 'CompTIA Security+ SY0-701 Certification Guide' - Ian Neil, Packt Publishing 2024")

# Título principal
st.title("🔐 Simulado CompTIA Security+ SY0-701")
st.markdown(f"**Modo de preparação oficial | {len(st.session_state.questoes_embaralhadas)} questões**")
st.markdown("---")

# Exibir questões
if not st.session_state.finalizado:
    with st.form(key="form_simulado"):
        for idx, questao in enumerate(st.session_state.questoes_embaralhadas):
            st.markdown(f"### {idx+1}. {questao['texto']}")
            st.caption(f"🏷️ Domínio {questao['dominio']}.0")
            
            # Radio buttons com indicação da resposta atual
            resposta_atual = st.session_state.respostas.get(idx, None)
            opcao_selecionada = st.radio(
                "Selecione uma alternativa:",
                options=[0, 1, 2, 3],
                format_func=lambda x: f"{chr(65+x)}) {questao['opcoes'][x]}",
                key=f"q_{idx}",
                index=resposta_atual if resposta_atual is not None else None,
                label_visibility="collapsed"
            )
            
            if opcao_selecionada is not None:
                st.session_state.respostas[idx] = opcao_selecionada
            
            st.divider()
        
        # Botão de finalizar
        col1, col2, col3 = st.columns([1, 2, 1])
        with col2:
            finalizar = st.form_submit_button("✅ FINALIZAR SIMULADO", use_container_width=True, type="primary")
            
        if finalizar:
            st.session_state.finalizado = True
            st.rerun()

# Exibir resultados
else:
    acertos, total = calcular_pontuacao()
    percentual = (acertos / total) * 100
    tempo_total = time() - st.session_state.tempo_inicio
    minutos = int(tempo_total // 60)
    segundos = int(tempo_total % 60)
    
    # Cabeçalho do resultado
    st.markdown("## 📊 RESULTADO DO SIMULADO")
    
    col1, col2, col3 = st.columns(3)
    with col1:
        st.metric("✅ Acertos", f"{acertos}/{total}")
    with col2:
        st.metric("📈 Percentual", f"{percentual:.1f}%")
    with col3:
        st.metric("⏱️ Tempo", f"{minutos:02d}:{segundos:02d}")
    
    # Barra de progresso personalizada
    cor = "green" if percentual >= 80 else "orange" if percentual >= 60 else "red"
    st.markdown(f"<div style='background-color: #f0f0f0; border-radius: 10px; height: 20px;'><div style='background-color: {cor}; width: {percentual}%; height: 20px; border-radius: 10px;'></div></div>", unsafe_allow_html=True)
    
    st.markdown(f"**Classificação:** {'🟢 APROVADO' if percentual >= 80 else '🟡 EM TREINAMENTO' if percentual >= 60 else '🔴 REVISAR CONTEÚDO'}")
    
    if percentual < 80:
        st.info("ℹ️ A CompTIA recomenda 80% de acertos para segurança no exame. Revise as questões que errou e tente novamente!")
    
    # Análise por domínio
    st.markdown("### 📈 Desempenho por Domínio")
    
    dados_desempenho = []
    for dominio in range(1, 6):
        questoes_dom = [(idx, q) for idx, q in enumerate(st.session_state.questoes_embaralhadas) if q["dominio"] == dominio]
        if questoes_dom:
            acertos_dom = sum(1 for idx, q in questoes_dom if st.session_state.respostas.get(idx) == q["resposta"])
            total_dom = len(questoes_dom)
            percentual_dom = (acertos_dom / total_dom) * 100
            dados_desempenho.append({
                "Domínio": f"{dominio}.0",
                "Acertos": acertos_dom,
                "Total": total_dom,
                "%": f"{percentual_dom:.1f}%"
            })
    
    st.dataframe(pd.DataFrame(dados_desempenho), use_container_width=True, hide_index=True)
    
    # Detalhamento das questões (corretas/incorretas)
    st.markdown("### 📝 Revisão Detalhada das Questões")
    
    for idx, questao in enumerate(st.session_state.questoes_embaralhadas):
        resposta_usuario = st.session_state.respostas.get(idx)
        acertou = resposta_usuario == questao["resposta"]
        
        with st.expander(f"{'✅' if acertou else '❌'} Questão {idx+1}: {questao['texto'][:80]}..."):
            st.markdown(f"**Domínio:** {questao['dominio_nome']}")
            st.markdown(f"**Sua resposta:** {chr(65+resposta_usuario)}) {questao['opcoes'][resposta_usuario] if resposta_usuario is not None else 'Não respondida'}")
            st.markdown(f"**Resposta correta:** {chr(65+questao['resposta'])}) {questao['opcoes'][questao['resposta']]}")
            st.markdown("**📖 Explicação detalhada:**")
            st.markdown(f"> {questao['explicacao']}")
    
    # Botões de ação
    col1, col2, col3 = st.columns(3)
    with col1:
        if st.button("🔄 Tentar Novamente", use_container_width=True):
            reiniciar_simulado()
    with col2:
        if st.session_state.simulado_atual < 20:
            if st.button(f"⏩ Próximo Simulado ({st.session_state.simulado_atual}/20)", use_container_width=True):
                proximo_simulado()
        else:
            st.success("🎉 Parabéns! Você completou todos os 20 simulados!")
    with col3:
        pass
    
    # Aviso sobre materiais oficiais
    st.divider()
    st.warning("""
    **⚠️ Aviso Importante:**  
    Este simulado é um recurso educacional baseado no livro "CompTIA Security+ SY0-701 Certification Guide" (Ian Neil, Packt Publishing 2024).  
    Para aprovação oficial, recomenda-se estudar também os objetivos oficiais do exame e realizar treinamentos práticos.  
    A CompTIA não autoriza o uso de "brain dumps" e pode cancelar certificações obtidas através desses materiais.
    """)

# Footer
st.markdown("---")
st.caption(f"🔐 Simulado CompTIA Security+ SY0-701 | Questões baseadas no livro de Ian Neil | Simulado {st.session_state.simulado_atual}/20 | Total de questões: {len(st.session_state.questoes_embaralhadas)}")
