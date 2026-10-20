<div align="center">

# 🚴 RouteCraft v3.0

### Uma aplicação web e API REST do estúdio **Velora Labs** para transformar as métricas das suas pedaladas em arte épica.

**Registre sua rota. Aplique seus filtros. Compartilhe sua conquista.**

[![Python Requirements](https://img.shields.io/badge/Python->=3.10-339933?style=flat-square&logo=python&logoColor=white)](#especificações-técnicas)
[![FastAPI Backend](https://img.shields.io/badge/FastAPI-Backend-009688?style=flat-square&logo=fastapi&logoColor=white)](#como-funciona-pipeline-v3)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-Frontend-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](#-quick-start)
[![Clean Architecture](https://img.shields.io/badge/Architecture-Clean_Code-8A2BE2?style=flat-square)](#✨-novidades-da-versão-30)
[![MIT license](https://img.shields.io/badge/license-MIT-f4c430?style=flat-square)](LICENSE)

<div align="center">
  <a href="https://routecraft-app.onrender.com/" target="_blank">
    <img src="https://img.shields.io/badge/🚀_Acessar_Web_Service-000000?style=for-the-badge&logo=vercel&logoColor=white" alt="Acessar Web Service">
  </a>
</div>

<br />

<img width="1536" height="1024" src="https://github.com/user-attachments/assets/9f3b070b-acc4-4155-ab74-4af738eb53d1" alt="Demonstração da Interface do RouteCraft"/>

<br /><br />

</div>

---

Fez uma pedalada épica e quer compartilhar os dados no Instagram com qualidade profissional? O **RouteCraft** (anteriormente *Cycling Poster Generator*) é uma aplicação web completa e independente desenvolvida sob o guarda-chuva da **Velora Labs** — um projeto focado na criação de um estúdio *open-source* de softwares e ferramentas digitais para o universo do ciclismo.

O sistema funde fotografias, dados geoespaciais (Strava Polyline ou GPX) e telemetria, renderizando pôsteres de alta qualidade diretamente no seu navegador.

A **versão 3.0** foca na maturidade de marca e design da aplicação. O motor de processamento, a arquitetura limpa (**Clean Architecture**), o processamento **100% In-Memory** e a **Integração OAuth2 com o Strava** consolidados na V2 continuam intactos no backend, ganhando agora uma nova interface moderna e coesa.

## ✨ Novidades da Versão 3.0

*   **Identidade Visual Renovada & Rebranding (RouteCraft):** Evolução do nome genérico para **RouteCraft**, estabelecendo uma marca forte e reconhecida.
*   **Visão de Estúdio Open-Source (Velora Labs):** O projeto agora é assinado pela **Velora Labs**, a semente de um estúdio focado em engenharia de dados e soluções visuais para ciclistas.
*   **Logotipos e Isotipos Vetoriais Nativos:**
    *   **Isotipo RouteCraft (`simbolo_RC.svg`):** A rota GPS contínua emoldurada em um retângulo estilo pôster, utilizando o gradiente entre *Adrenaline Orange* (`#FC5200`) e *Telemetry Cyan* (`#06B6D4`).
    *   **Logotipo Velora Labs (`logo_VL.svg`):** O "V" angular com inspiração técnica e de código, assinando a aplicação no rodapé.
*   **Design System Atualizado:**
    *   **Paleta de Cores:** Fundo *Asfalto Espacial* (`#0F172A`), botões de ação e Strava em *Laranja Adrenalina* (`#FC5200`), e detalhes de foco, bordas, métricas e destaques do tutorial em *Cyan Telemetria* (`#06B6D4` / `#01D3F0`).
    *   **Tipografia Oficial:** Integração das fontes *Outfit* (títulos do produto) e *Space Grotesk* (identidade do estúdio) via Google Fonts, mantendo a *Montserrat* para leitura de métricas e formulários.
*   **Ajustes Finos de UI/UX:** Destaques visuais do assistente de tutorial adaptados para a cor de destaque principal, mantendo toda a lógica funcional da versão anterior.

---

## 🚀 Quick Start

### 1. Clonar e Configurar

Com o Python 3.10+ instalado, faça o clone do repositório e prepare o ambiente:

```bash
git clone https://github.com/Ghost-Do-While/cycling-poster-generator.git
cd cycling-poster-generator

# Instalar dependências
pip install -r requirements.txt
```

### 2. Variáveis de Ambiente (.env)

Crie um arquivo `.env` na raiz do projeto e insira suas credenciais da API do Strava (necessárias para o login automático):

```env
STRAVA_CLIENT_ID=seu_client_id_aqui
STRAVA_CLIENT_SECRET=seu_client_secret_aqui
STRAVA_REDIRECT_URI=http://127.0.0.1:8000/strava/callback
```

### 3. Iniciar o Servidor

O sistema gera automaticamente as pastas de *assets* na primeira inicialização.

```bash
uvicorn server:app --reload
```

Abra o seu navegador em `[http://127.0.0.1:8000](http://127.0.0.1:8000)`. A mágica acontece instantaneamente sem recarregar a página.

---

## ⚙️ Como Funciona (Pipeline V3)

A arquitetura do sistema permanece desenhada em camadas estritamente isoladas:

1. **Camada de Rede (`strava_client.py` & `server.py`):** Gerencia a conexão OAuth2, converte os *Authorization Codes* em Tokens, normaliza as respostas JSON e fornece os *endpoints* REST estáticos e dinâmicos.
2. **Orquestração (`poster_service.py`):** Recebe o fluxo de bytes de imagens e parâmetros via HTTP e atua como o maestro do sistema, passando os dados entre as várias ferramentas matemáticas e visuais.
3. **Motor Gráfico e Geométrico (`photo.py`, `effects.py`, `route.py`):**
   * Corrige a orientação EXIF e executa o *Crop Lanczos*.
   * Transforma as polilinhas do Strava ou dados XML de um arquivo `.gpx` em arrays geográficos, transpondo as latitudes/longitudes perfeitamente escaladas para a Bounding Box de pixels do canvas.
   * Converte matrizes bidimensionais PNG (HaldCLUT) em Filtros LUT 3D matemáticos (`NumPy`) para coloração profissional.
4. **Exportação:** O arquivo `JPEG` final é renderizado e devolvido em uma única resposta assíncrona para o usuário.

---

## 📊 Especificações Técnicas

| Módulo | Responsabilidade Core | Stack / Motor Utilizado |
| :--- | :--- | :--- |
| **Integração Strava** | Autenticação OAuth2 e conversão em tempo real de `summary_polyline`. | `requests`, `polyline` |
| **Fotografias Base** | Orientação EXIF corrigida e Crop inteligente. Processamento *In-Memory*. | `Pillow`, `io.BytesIO` |
| **Geometria Espacial** | Desenho matemático do trajeto da pedalada com escala dinâmica baseada em limites geográficos. | Python *Standard Library* (`xml.etree.ElementTree`) |
| **Filtros de Cor (LUT)** | Modificação colorimétrica profunda na matriz RGB via matrizes HaldCLUT. | `Pillow` (Color3DLUT), `NumPy` |
| **Tipografia & UI** | Interface com Tailwind CSS, fontes *Outfit*, *Space Grotesk* e renderização TrueType (`.ttf`). | Google Fonts, Fontes TrueType, Filtros Gaussianos |

---

## 🌟 Showcases & Formatos

O sistema foi desenhado levando em conta os algoritmos de compressão e recorte do Meta/Instagram:

- **Stories do Instagram (9:16):** Resolução nativa de 1080x1920 pixels.
- **Feed do Instagram (4:5):** Resolução nativa de 1080x1350 pixels, ideal para maior retenção e área de tela.
- **Formato Original:** Preserva a proporção exata da fotografia original enviada pelo usuário.

---

## 🤝 Como Contribuir

Adoraríamos ter a sua ajuda para evoluir o **RouteCraft** e construir o ecossistema da **Velora Labs**! Você pode:
- Reportar bugs ou sugerir novas ideias na aba **Issues**.
- Enviar um **Pull Request** com melhorias no código, otimizações no motor de renderização ou novos recursos.
- Compartilhar novos arquivos **HaldCLUT** (filtros de cor) ou conversões de arquivos **.CUBE** para adicionar à pasta `assets/presets`.

---

## ❓ FAQ (Perguntas Frequentes)

<details>
<summary><strong>O que mudou da versão 2.0 para a 3.0?</strong></summary>

Não houve alterações drásticas na lógica de negócio do servidor ou no motor de renderização. A versão 3.0 consolida a marca **RouteCraft**, introduz o conceito do estúdio **Velora Labs**, atualiza a paleta de cores (Cyan + Laranja em fundo Slate), integra novos logotipos/isotipos em SVG e refina a tipografia para um nível de produto profissional.
</details>

<details>
<summary><strong>Sou obrigado a conectar a minha conta do Strava?</strong></summary>

Não! O sistema continua a suportar rotas 100% manuais de forma independente. Basta ignorar o botão de Login, enviar o seu arquivo GPX e digitar os números livremente.
</details>

<details>
<summary><strong>Por que usar arquivos HaldCLUT em vez de filtros nativos?</strong></summary>

Os arquivos HaldCLUT permitem que você crie uma edição complexa no Lightroom, DaVinci Resolve ou Photopea (mexendo em curvas, saturações e eixos de cor) e exporte essa "matemática" em uma grade PNG (ex: 512x512). O servidor lê esse PNG com o `NumPy` e replica o efeito exato em uma fração de segundo na sua fotografia.
</details>

<details>
<summary><strong>O que acontece com a minha imagem ou token depois que a arte é gerada?</strong></summary>

Nada fica retido no servidor. A arquitetura atual é 100% focada na memória RAM (Stateless). As fotografias originais nunca chegam a tocar no disco rígido do servidor e desaparecem no momento em que a conexão HTTP termina. Não salvamos sessões ou senhas de usuários.
</details>

---

## 📜 Licença

[MIT](LICENSE)

Desenvolvido para amantes de ciclismo, código limpo e design profissional sob a iniciativa **Velora Labs**. Sinta-se livre para utilizar, clonar, alterar ou integrar este código em outros projetos comerciais ou recreativos.
