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
```
## 2. Design Tokens (Premium Archival Aesthetic)

### Paleta de Cores e Materiais
* **Fundo da Aplicação (Floor):** Superfície escura (`#121415`) com gradiente sutil (Vinho Profundo para Preto) simulando iluminação de galeria.
* **Texturas (Prateleiras):** Uso de textura orgânica de madeira Nogueira (Walnut Wood) para criar divisão estrutural.
* **Cards e Contêineres (Archive Case):** Efeito *Glassmorphism* (vidro fumê) em carvão escuro (`rgba(26, 26, 36, 0.60)`) com desfoque de fundo (backdrop-blur).
* **Cor de Destaque (Botões e Tags):** Dourado Polido (`#D4AF37` / `#e9c349`) reservado para ações principais e itens raros.
* **Texto Principal:** Off-white (`#e2e2e3`) para conforto visual, e Cinza Suave (`#d4c2c6`) para metadados.

### Tipografia
* **Títulos (Headings):** Playfair Display (trazendo voz editorial, autoritária e clássica).
* **Textos Normais e Metadados (Body/Label):** Inter (garantindo legibilidade máxima para especificações técnicas e inventário, mesmo em tamanhos pequenos).
* **Espaçamentos:** Sistema de grid base de 8pt e utilitários do Bootstrap (rem).

## 3. Protótipo Interativo
* **Figma / Stitch.AI:** https://stitch.withgoogle.com/projects/17137094067812874499
  
## 4. Dicionário de Dados

Detalhamento dos atributos que compõem a entidade principal do nosso sistema.

### Entidade: Álbum
| Campo | Tipo | Obrigatório | Descrição |
| :--- | :--- | :---: | :--- |
| `id` | String | Sim | Identificador único gerado automaticamente. |
| `titulo` | String | Sim | Nome oficial do álbum. |
| `artista` | String | Sim | Nome da banda, artista principal ou grupo. |
| `ano` | Number | Não | Ano de lançamento original. |
| `genero` | String | Sim | Gênero musical (ex: Jazz, Rock, MPB). |
| `formato` | String | Sim | Tipo de mídia física (Vinil, CD, K7). |
| `capaUrl` | String | Não | URL da imagem da capa em alta resolução. |

---

## 5. Mapeamento de Rotas (APIs)

O sistema fará uso de duas frentes de consumo de dados: uma API pública para enriquecimento e um Mock Backend para persistência.

### API Externa (iTunes Search API)
Responsável por buscar capas e dados originais dos álbuns (Atende ao **ID 24**).
* **`GET`** `https://stitch.withgoogle.com/projects/6420219798709879911`
  * *Descrição:* Retorna uma lista de álbuns correspondentes à busca. O parâmetro `term` deve conter o nome do artista ou álbum (espaços substituídos por `+`).

### Mock Backend (JSON Server)
Responsável por simular nosso servidor local (Atende aos **IDs 22 e 23**).
* **Base URL:** `http://localhost:3000`
* **`GET /albuns`**: Retorna a lista completa do acervo salvo.
* **`POST /albuns`**: Cadastra um novo disco na estante.
* **`PUT /albuns/:id`**: Atualiza as informações de um disco existente.
* **`DELETE /albuns/:id`**: Remove um disco da coleção.
* **`GET /fila`**: Retorna os itens adicionados à fila de reprodução.

---

## 6. Estrutura do Banco de Dados (db.json)

Como utilizaremos o **JSON Server** para simular nosso banco de dados relacional, a persistência ocorrerá em um arquivo estático `db.json` localizado na raiz do projeto. O esqueleto inicial será este:

```json
{
  "albuns": [
    {
      "id": "1",
      "titulo": "Kind of Blue",
      "artista": "Miles Davis",
      "ano": 1959,
      "genero": "Jazz",
      "formato": "Vinil",
      "capaUrl": "[https://exemplo.com/kind-of-blue.jpg](https://exemplo.com/kind-of-blue.jpg)"
    }
  ],
  "fila": []
}
