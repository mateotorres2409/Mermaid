graph TD
    %% Estilos Generales y Paleta Profesional de Ingeniería
    classDef default fill:#FAFAFA,stroke:#333,stroke-width:1px,color:#111,font-family:monospace;
    classDef layerBox fill:#F4F6F9,stroke:#1A365D,stroke-width:2px,stroke-dasharray: 5 5,color:#1A365D,font-weight:bold;
    classDef component fill:#FFFFFF,stroke:#2B6CB0,stroke-width:1.5px,color:#2D3748;
    classDef iaComponent fill:#EDF2F7,stroke:#319795,stroke-width:2px,color:#234E52,font-weight:bold;

    %% Control Plane Central Superior
    SLAaC["Plano de Control Declarativo: SLA as Code (SLAaC YAMLs en Git)"]
    class SLAaC iaComponent;

    %% Capa 1: Generate
    subgraph Capa1 ["Capa 1: GENERATE (Origen de Telemetría)"]
        direction LR
        A[Microservicios Bancarios] --> B(Instrumentación Nativa OpenTelemetry SDKs)
        A --> C(Sondas eBPF a Nivel de Kernel/Red)
    end
    class Capa1 layerBox;
    class A,B,C component;

    %% Capa 2: Optimize
    subgraph Capa2 ["Capa 2: OPTIMIZE (Procesamiento e Ingesta Elásticas)"]
        direction LR
        D[OTel Collectors / Fluent Bit] --> E{Filtros de Edge Masking: Anonimización PII}
        E -->|Datos Limpios| F[Buffer Asíncrono: Apache Kafka]
    end
    class Capa2 layerBox;
    class D,E,F component;

    %% Capa 3: Illuminate
    subgraph Capa3 ["Capa 3: ILLUMINATE (Cerebro Analítico & AIOps)"]
        direction TB
        G[(TimescaleDB / Mimir: Series de Tiempo)]
        H[(Vector Database: Embeddings de Logs)]
        
        G --> I[Modelos de IA: Isolation Forests & LSTM]
        H --> J[Asistente RAG Cognitivo N1]
        
        K[Agrupamiento Topológico Causal] --- I
    end
    class Capa3 layerBox;
    class G,H component;
    class I,J,K iaComponent;

    %% Salida / Orquestación Final
    ITSM[Plataforma ITSM Centralizada / Flujos Self-Healing]
    class ITSM component;

    %% Conexiones Estructurales entre Capas y Planos
    SLAaC -.->|Gobierna Métricas y SLOs| Capa1
    SLAaC -.->|Define Umbrales Dinámicos| Capa3
    
    B & C -->|Métricas, Eventos, Logs, Trazas MELT| D
    F -->|Streaming de Telemetría Enriquecida| G & H
    
    I & J & K -->|Diagnóstico Semántico & Alertas Consolidadas| ITSM