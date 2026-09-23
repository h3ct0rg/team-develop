# Team Dev — Equipo de desarrollo con agentes de IA

Un equipo de desarrollo definido en Markdown puro:
- **Agnóstico del lenguaje**: se adapta al stack de cada proyecto (Python, Go, Java, .NET, Node, etc.).
- **Agnóstico de la herramienta**: funciona con Claude Code, Cursor, GitHub Copilot, Codex o cualquier IA con acceso a archivos.
- **Agnóstico del modelo y del proveedor**: se puede usar con cualquier harness de IA, sea de OpenAI (GPT, Codex), Anthropic (Claude), DeepSeek, Google (Gemini), Mistral, Qwen o modelos locales (Ollama, LM Studio). No usa APIs, formatos ni funciones propias de ningún proveedor: son instrucciones en Markdown que cualquier modelo puede seguir.

## Requisitos del harness
Cualquier agente o harness de IA sirve si puede:
1. **Leer y escribir archivos** (los archivos de rol y la carpeta de la tarea).
2. **Ejecutar comandos** en la terminal (build, tests y lint del proyecto).
3. *(Opcional)* **Lanzar subagentes**. Si puede, el CTO delega cada rol en un subagente. Si no, el mismo agente toma cada rol por turno.

Ejemplos: Claude Code, OpenAI Codex CLI, Cursor, GitHub Copilot (modo agente), Aider, Cline, Roo Code, Continue, OpenHands, Gemini CLI, o un harness propio sobre la API de OpenAI, Anthropic o DeepSeek, entre otros.

## Roles
| Archivo | Rol | Responsabilidad |
|---|---|---|
| [cto.md](cto.md) | CTO | Recibe la tarea, detecta el stack, asigna roles y coordina el flujo |
| [project-manager.md](project-manager.md) | Project Manager | Genera el plan de subtareas con criterios de aceptación |
| [senior-developer.md](senior-developer.md) | Senior Developer | Implementa como experto en el stack que asigna el CTO |
| [code-reviewer.md](code-reviewer.md) | Code Reviewer | Revisa el código; aprueba o lo devuelve al developer |
| [qa-engineer.md](qa-engineer.md) | QA Engineer | Valida la calidad y los requisitos; aprueba o rechaza |

## Flujo
```
Tarea → CTO → PM → Dev ⇄ Code Review (máx. 3 vueltas) → QA (máx. 2 vueltas) → Cierre
                    ↑                                    │
                    └──────── si QA rechaza ─────────────┘
```

- Si el PM detecta preguntas que bloquean, el CTO se detiene y te consulta.
- Si se superan los límites de iteraciones, el CTO se detiene y te pide una decisión.
- Toda la comunicación entre roles es por archivos, en `tareas/<fecha>-<slug>/`:

| Archivo | Lo genera |
|---|---|
| `00-brief.md` | CTO: tarea, proyecto y perfil de stack |
| `01-plan.md` | Project Manager |
| `02-dev-N.md` | Senior Developer (una por iteración) |
| `03-review-N.md` | Code Reviewer |
| `04-qa-M.md` | QA Engineer |
| `05-resumen.md` | CTO: cierre de la tarea |

## Instalación
```bash
git clone https://github.com/h3ct0rg/team-develop.git
```
No requiere dependencias. Solo necesitas una herramienta de IA que pueda leer y escribir archivos y ejecutar comandos (para correr el build y los tests del proyecto).

## Cómo ejecutar una tarea
Abre tu herramienta de IA (idealmente en la carpeta del proyecto donde vas a trabajar) y escribe:

```
Lee <ruta-a-team-develop>/cto.md y ejecuta como CTO la tarea:
"<descripción de la tarea>"
Proyecto: <ruta del proyecto>
```

Ejemplo:
```
Lee C:/repos/team-develop/cto.md y ejecuta como CTO la tarea:
"Agregar endpoint POST /users con validación de email y tests"
Proyecto: C:/repos/mi-api
```

Si omites `Proyecto`, se usa el directorio de trabajo actual.

### Según la herramienta
- **Claude Code**: `claude` en la carpeta del proyecto y pega el prompt. El CTO usará subagentes para cada rol.
- **Cursor / Copilot (modo agente)**: abre el proyecto, agrega `team-develop` al workspace o referencia la ruta de `cto.md`, y pega el prompt en el chat del agente.
- **Codex u otras CLIs**: ejecútala en la carpeta del proyecto con el mismo prompt.

Si la herramienta no soporta subagentes, la misma IA toma cada rol por turno, siguiendo su archivo.

## Personalización
- Edita los archivos de rol para agregar los estándares de tu equipo (convenciones, cobertura mínima, checklist de seguridad).
- Ajusta los límites de iteraciones en [cto.md](cto.md).
- Las plantillas de brief y resumen están en [tareas/_plantilla/](tareas/_plantilla/).
