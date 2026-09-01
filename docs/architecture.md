# Especificação Técnica e Arquitetura - Minha Discoteca

## 1. Modelo de Dados (Diagrama ER)

```mermaid
erDiagram
    COLECIONADOR ||--o{ ALBUM : gerencia
    ALBUM ||--o{ FILA_REPRODUCAO : possui

    COLECIONADOR {
        string id PK
        string nome
    }

    ALBUM {
        string id PK
        string titulo
        string artista
        int ano
        string genero
        string formato
        string capaUrl
    }

    FILA_REPRODUCAO {
        string id PK
        string albumId FK
        datetime adicionadoEm
    }
