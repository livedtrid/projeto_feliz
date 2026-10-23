# Projeto Feliz - Documentação para Agentes

## Visão Geral

**Projeto:** projeto_feliz  
**Tipo:** Aplicação Web com Realidade Aumentada (WebXR)  
**Descrição:** Visualizador 3D interativo de modelo 3D com suporte a Realidade Aumentada (AR) para dispositivos mobile e desktop  
**Autor:** Hernani Pawlick David  
**Linguagens:** HTML5, CSS3, JavaScript (Vanilla)

## Objetivo Principal

Fornecer uma experiência imersiva de visualização de modelos 3D em realidade aumentada, permitindo que usuários vejam um modelo 3D (Ghost Rider do meowniverse) em seu espaço físico através do navegador web.

## Estrutura do Projeto

### Arquivos Principais

```
projeto_feliz/
├── index.html                        # Página HTML principal
├── script.js                         # Lógica JavaScript
├── styles.css                        # Estilos CSS
├── meowniverse_-_ghost_rider.glb     # Modelo 3D (formato glB)
├── poster.webp                       # Imagem de preview
├── ar_icon.png                       # Ícone do botão AR
├── ar_hand_prompt.png                # Imagem de animação de prompt
├── snippet.txt                       # Trecho de código HTML reutilizável
├── README.md                         # Documentação básica
└── .git/                             # Histórico Git
```

## Tecnologias Utilizadas

### Bibliotecas Externas
- **Model Viewer** (v4.4.0): Componente web da Google para visualização de modelos 3D
  - URL: `https://ajax.googleapis.com/ajax/libs/model-viewer/4.4.0/model-viewer.min.js`
  - Fornece suporte nativo a AR (WebXR, Scene Viewer, Quick Look)

### Browser APIs
- **WebXR API**: Padrão para realidade aumentada e virtual na web
- **HTML5 Web Components**: Elemento `<model-viewer>` customizado

## Componentes Principais

### 1. Elemento `<model-viewer>` (index.html)

**Responsabilidade:** Renderizar o modelo 3D e gerenciar experiência AR

**Atributos Chave:**
- `src="meowniverse_-_ghost_rider.glb"` - Arquivo do modelo 3D
- `ar` - Habilita modo AR
- `ar-modes="webxr scene-viewer quick-look"` - Suporta múltiplos modos de AR:
  - **webxr**: Realidade aumentada nativa (Chrome, Firefox)
  - **scene-viewer**: Android Google Play Services
  - **quick-look**: iOS Safari
- `camera-controls` - Permite rotação e zoom do modelo
- `tone-mapping="neutral"` - Configuração de renderização
- `poster="poster.webp"` - Imagem de fallback enquanto carrega
- `shadow-intensity="1"` - Intensidade das sombras

**Slots (elementos aninhados):**
1. `progress-bar` - Barra de progresso durante carregamento
2. `ar-button` - Botão "View in your space" (customizado)
3. `ar-prompt` - Animação de prompt com imagem de mão

### 2. Barra de Progresso (styles.css + script.js)

**Funcionalidade:** Mostra progresso de carregamento do modelo 3D

**Comportamento:**
- Inicialmente oculta (classe `hide`)
- Aparece quando carregamento começa
- Barra branca se preenche até 100%
- Desaparece quando carregamento completa
- Animação suave de transição (0.3s)

**Classe CSS:**
- `.progress-bar` - Contêiner (33% width, 2% max-height)
- `.update-bar` - Barra de preenchimento
- `.hide` - Estado oculto

### 3. Botão AR (styles.css)

**Elemento:** `<button id="ar-button">`

**Estilo:**
- Posicionado absolutamente no bottom (16px)
- Ícone de AR (ar_icon.png) à esquerda
- Fundo branco, texto azul (#4285f4)
- Bordas arredondadas (border-radius: 18px)
- Estados: `:active`, `:focus`, `:focus-visible`

### 4. Prompt de Interação (styles.css)

**Elemento:** `<div id="ar-prompt">`

**Funcionalidade:** Anima uma imagem de mão para guiar o usuário

**Animações:**
- `elongate`: Move a mão para frente e para trás (2s, infinite)
- `circle`: Rotaciona a mão (4s, infinite)
- Só aparece quando AR session é iniciada (`ar-status="session-started"`)

## JavaScript (script.js)

### Função Principal: `onProgress()`

**Responsabilidade:** Gerenciar atualização da barra de progresso

**Lógica:**
```javascript
1. Captura evento 'progress' do <model-viewer>
2. Obtém referências aos elementos .progress-bar e .update-bar
3. Atualiza width de .update-bar baseado em event.detail.totalProgress
4. Quando totalProgress === 1 (100%):
   - Adiciona classe 'hide' à barra
   - Remove listener de eventos para não executar novamente
5. Se progress < 100%:
   - Remove classe 'hide' para mostrar barra
```

**Evento:** `progress` (disparado pelo `<model-viewer>`)

## CSS - Conceitos-Chave

### Estilo Global
- Elemento `html` e `body`: 100% de altura (full-screen)
- `:not(:defined) > *`: Esconde conteúdo enquanto Web Component não carrega

### Responsividade
- `<model-viewer>`: 100% width, 90% height
- Layout fullscreen sem margens ou padding

### Animações
- Smooth transitions (0.3s)
- Infinite animations para prompt (2s + 4s)
- Transform-based para melhor performance

## Recursos Estáticos

### Modelo 3D
- **Arquivo:** `meowniverse_-_ghost_rider.glb`
- **Tamanho:** ~1MB
- **Formato:** glB (comprimido com textura e materiais)
- **Tema:** Character 3D (Ghost Rider) do universo Meowniverse

### Imagens
- **poster.webp** (~30KB): Preview do modelo para carregamento rápido
- **ar_icon.png** (~680B): Ícone do botão de AR
- **ar_hand_prompt.png** (~12KB): Imagem de animação de prompt

## Modos de AR Suportados

| Modo | Plataforma | Requisitos |
|------|-----------|-----------|
| WebXR | Desktop/Mobile | Navegador com suporte WebXR (Chrome, Firefox, Edge) |
| Scene Viewer | Android | Google Play Services instalado |
| Quick Look | iOS | iOS 12.2+ com Safari |

## Fluxo de Uso

1. **Carregamento:** Página carrega, modelo começa a baixar
2. **Progresso:** Barra de progresso aparece e se atualiza
3. **Pronto:** Barra desaparece, modelo visível na tela
4. **Interação:** Usuário pode:
   - Rotacionar modelo (camera-controls)
   - Clicar botão "View in your space" para AR
5. **Sessão AR:** Se supported, inicia AR com animação de prompt (mão)

## Configuração e Deployment

### Para Executar Localmente
```bash
# Qualquer servidor HTTP serve a aplicação
python -m http.server 8000
# Ou com Live Server do VS Code
```

### Requisitos de Browser
- Suporte a Web Components
- JavaScript habilitado
- Para AR: WebXR, Scene Viewer ou Quick Look suportado

## Pontos de Extensão Futuros

1. **Múltiplos Modelos:** Selector de diferentes modelos 3D
2. **Customização AR:** Ajustes de escala/posição em AR
3. **Persistência:** Salvar posição/rotação do modelo
4. **Análise:** Tracking de uso e interações
5. **Galereia:** Portfolio de múltiplos modelos
6. **Mobile Optimization:** Gestos touchscreen específicos

## Conhecimento Técnico Necessário

- **Web Components:** Entender custom elements e slots
- **Model Viewer API:** Eventos, propriedades e métodos
- **WebXR API:** Conceitos de sessões e hit testing
- **CSS Animations:** Transforms e transitions
- **3D Concepts:** Básico de modelos glB, materiais, iluminação

## Git History

- `331a657` - Add project
- `b7325f6` - Add author information to README
- `9df51f8` - Initial commit

## Contato e Autoria

**Autor:** Hernani Pawlick David  
**Projeto:** Realidade Aumentada para Web (WebXR)
