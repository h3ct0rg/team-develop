# Team Dev — Equipo de desarrollo con agentes de IA

Un equipo de desarrollo definido en Markdown puro:
- **Agnóstico del lenguaje**: se adapta al stack de cada proyecto (Python, Go, Java, .NET, Node, etc.).
- **Agnóstico de la herramienta**: funciona con Claude Code, Cursor, GitHub Copilot, Codex o cualquier IA con acceso a archivos.
- **Agnóstico del modelo y del proveedor**: se puede usar con cualquier harness de IA, sea de OpenAI (GPT, Codex), Anthropic (Claude), DeepSeek, Google (Gemini), Mistral, Qwen o modelos locales (Ollama, LM Studio). No usa APIs, formatos ni funciones propias de ningún proveedor: son instrucciones en Markdown que cualquier modelo puede seguir.

## Requisitos del harness
Cualquier agente o harness de IA sirve si puede:
1. **Leer y escribir archivos** (los archivos de rol y la carpeta de la tarea).
2. **Ejecutar comandos** en la terminal (build, tests y lint del proyecto).
3. *(Opcional)* **Lanzar subagentes**. Si puede, el CTO delega cada instancia en un subagente y ejecuta en paralelo los carriles independientes. Si no, el mismo agente toma cada instancia por turno.

Ejemplos: Claude Code, OpenAI Codex CLI, Cursor, GitHub Copilot (modo agente), Aider, Cline, Roo Code, Continue, OpenHands, Gemini CLI, o un harness propio sobre la API de OpenAI, Anthropic o DeepSeek, entre otros.

## Roles
| Archivo | Rol | Instancias | Responsabilidad |
|---|---|---|---|
| [cto.md](cto.md) | CTO | 1 | Recibe la tarea, detecta el stack, contrata el equipo y coordina el flujo |
| [project-manager.md](project-manager.md) | Project Manager | 1 | Genera el plan: subtareas, carriles, complejidad y criterios de aceptación |
| [ux-designer.md](ux-designer.md) | UX Designer | 0–6 | Diseña la interfaz antes del desarrollo y la valida en QA (solo si hay UI) |
| [senior-developer.md](senior-developer.md) | Senior Developer | 1–6 | Implementa su carril como experto en el stack que asigna el CTO |
| [code-reviewer.md](code-reviewer.md) | Code Reviewer | 1–6 | Revisa el código por carril o especialidad; aprueba o lo devuelve |
| [qa-engineer.md](qa-engineer.md) | QA Engineer | 1–6 | Valida la calidad y los requisitos; aprueba o rechaza |

## Equipo escalable (1 a 6 por rol)
El CTO decide cuántas instancias "contrata" de cada rol según la complejidad del plan:

1. El PM divide el trabajo en **carriles** (C1, C2…): bloques con sus propios archivos y stack, que un solo developer lleva de principio a fin. También estima la complejidad (S/M/L/XL) e indica si hay interfaz.
2. El CTO arma la dotación y la registra en `00-equipo.md`. Cada instancia tiene un ID (`dev-1`, `rev-2`, `qa-1`, `ux-1`), una especialización (p.ej. `dev-1` senior Go para el backend, `dev-2` senior React para el frontend) y un alcance.

| Complejidad | Dev | Review | QA | UX (si hay UI) |
|---|---|---|---|---|
| S | 1 | 1 | 1 | 1 |
| M | 2 | 1–2 | 1 | 1 |
| L | 3–4 | 2–3 | 2 | 1–2 |
| XL | 5–6 | 3–6 | 2–6 | 2–3 |

Reglas: máximo 6 por rol; nunca más devs que carriles; cada carril tiene 1 dev y al menos 1 reviewer; UX = 0 si no hay interfaz. Los carriles independientes avanzan en paralelo si el harness soporta subagentes; si no, en secuencia.

**Ejemplos**
- *"Corregir el cálculo de impuestos en la API"* → complejidad S, 1 carril: 1 dev, 1 review, 1 QA, 0 UX.
- *"Módulo de registro de usuarios: API, pantalla web y notificaciones por email"* → complejidad L, 3 carriles: 3 dev, 2 review (uno de ellos de seguridad), 2 QA, 1 UX.

## Flujo
```
Tarea → CTO → PM → Contratación → UX Diseño → Devs ⇄ Review (máx. 3 por carril) → QA + UX Validación (máx. 2) → Cierre
                                                ↑                                       │
                                                └──── defectos vuelven al carril ───────┘
```

- Si el PM o el UX detectan preguntas que bloquean, el CTO se detiene y te consulta.
- Un carril con cambios requeridos vuelve solo a su dev; los carriles aprobados siguen.
- Si se superan los límites de iteraciones, el CTO se detiene y te pide una decisión.
- Toda la comunicación entre roles es por archivos, en `tareas/<fecha>-<slug>/` (K = número de instancia, N/M = iteración):

| Archivo | Lo genera |
|---|---|
| `00-brief.md` | CTO: tarea, proyecto, stack y estado por carril |
| `00-equipo.md` | CTO: dotación contratada y justificación |
| `01-plan.md` | Project Manager |
| `01-ux-<K>.md` | UX Designer (diseño) |
| `02-dev-<K>-<N>.md` | Senior Developer |
| `03-review-<K>-<N>.md` | Code Reviewer (veredicto por carril) |
| `04-qa-<K>-<M>.md` | QA Engineer |
| `04-ux-<K>-<M>.md` | UX Designer (validación) |
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
- Ajusta los límites de iteraciones y la tabla de dimensionamiento del equipo en [cto.md](cto.md).
- Las plantillas de brief, equipo y resumen están en [tareas/_plantilla/](tareas/_plantilla/).
