# ⚡ Batalla Pokémon con n8n y Groq

Workflow de [n8n](https://n8n.io) que enfrenta a dos Pokémon, calcula quién gana según sus estadísticas y usa inteligencia artificial (Groq) para narrar la batalla en español.

## ¿Cómo funciona?

1. El usuario abre un formulario, elige dos Pokémon y un estilo de narración.
2. El flujo consulta la [PokéAPI](https://pokeapi.co) para obtener las estadísticas de cada Pokémon.
3. Calcula el poder de cada uno con la fórmula:

   ```
   poder = ataque × 2 + defensa + velocidad
   ```

4. Decide el ganador (o empate si los poderes son iguales).
5. Envía un prompt a la API de Groq para que narre la batalla en 100 a 150 palabras, respetando el resultado.
6. Muestra al usuario una pantalla final con el ganador y la narración.

## Nodos del workflow

| Nodo | Tipo | Función |
|------|------|---------|
| On form submission | Form Trigger | Recibe los Pokémon y el estilo de narración |
| Consulta a PokéAPI | HTTP Request | Obtiene las estadísticas de cada Pokémon |
| Calcular batalla | Code | Calcula el poder de cada Pokémon y define el ganador |
| Preparar prompt | Edit Fields | Arma las instrucciones para la IA |
| Llamada a Groq | HTTP Request | Genera la narración de la batalla |
| Extraer narración | Code | Obtiene el texto de la IA y arma el título y el mensaje final |
| Mostrar resultado | Form (Form Ending) | Muestra el resultado al usuario |

## Requisitos

- Una instancia de n8n (en la nube o local).
- Una API key de Groq. Se crea gratis en [console.groq.com](https://console.groq.com) → API Keys.

## Instalación

1. Descarga el archivo `.json` de este repositorio.
2. En n8n, abre el menú **⋯** → **Import from File** y selecciona el archivo.
3. Crea tu credencial de Groq:
   - Abre el nodo que llama a Groq.
   - En **Authentication**, elige **Generic Credential Type** → **Bearer Auth**.
   - Crea una credencial nueva y pega tu API key en **Bearer Token** (solo la key, sin escribir `Bearer`).
4. Guarda el workflow y actívalo con el interruptor **Active**.
5. Abre el nodo **On form submission**, copia la **Production URL** y ábrela en el navegador.

## Uso

Escribe el nombre de dos Pokémon en inglés y en minúsculas (por ejemplo `pikachu` y `bulbasaur`), elige un estilo de narración y envía el formulario. En unos segundos verás quién ganó y la narración de la batalla.

### Ejemplo

| Pokémon | Ataque | Defensa | Velocidad | Poder |
|---------|--------|---------|-----------|-------|
| Pikachu | 55 | 40 | 90 | **240** |
| Bulbasaur | 49 | 49 | 45 | **192** |

**Resultado:** GANADOR: PIKACHU

## Solución de problemas

- **Error 401:** la API key de Groq no es válida o está mal configurada. Revisa que el Bearer Token no incluya la palabra `Bearer` ni espacios extra.
- **Referenced node doesn't exist:** el nombre del nodo dentro de `$('...')` en un nodo Code no coincide con el nombre real del nodo. Revisa mayúsculas, acentos y espacios.
- **El formulario no responde:** confirma que el workflow esté activo y que estés usando la Production URL.

## Seguridad

El archivo exportado **no incluye credenciales**. Cada persona que use este workflow debe configurar su propia API key de Groq. Nunca escribas tu key directamente en un nodo ni la subas al repositorio.

## Tecnologías

- [n8n](https://n8n.io): automatización de flujos
- [PokéAPI](https://pokeapi.co): datos de Pokémon
- [Groq](https://groq.com): modelo de lenguaje para la narración
