# Team Dev — equipo de desarrollo eficiente con agentes de IA

🌐 **Español** | [English](README.en.md)

Team Dev define un equipo de desarrollo en Markdown, agnóstico de lenguaje, modelo y harness. Está diseñado para mantener la calidad sin convertir la coordinación en el principal consumidor de tokens.

Funciona con cualquier entorno que pueda leer/escribir archivos y ejecutar comandos. Los subagentes son opcionales: si no existen, una misma IA asume los roles necesarios por turno.

## Qué cambió: eficiencia sin perder control

El sistema no asigna agentes por una tabla fija de complejidad. El CTO los contrata solo si existe una necesidad demostrable: trabajo independiente sin archivos compartidos, una especialidad ausente, cobertura independiente por riesgo o ahorro real de espera.

| Riesgo | Equipo inicial |
|---|---|
| Bajo: cambio localizado y con tests | 1 dev + gate automático |
| Medio: varios módulos o contrato interno | 1 dev + 1 reviewer |
| Alto: migración, seguridad, datos, concurrencia o API pública | 1 dev + 1 reviewer + 1 QA |

Se empieza con un máximo de 2 devs activos, 1 reviewer y 1 QA. Cualquier ampliación debe quedar justificada en el registro de equipo. UX se usa únicamente cuando cambia una interfaz visual o flujo humano.

## Comunicación compacta

La carpeta de la tarea es la fuente de verdad. El CTO mantiene un estado canónico y los demás roles escriben solo deltas: criterios cubiertos, rutas/símbolos modificados, evidencia, decisiones nuevas y bloqueos.

- No se reenvían briefs, planes ni historiales completos entre agentes.
- Los reportes limpios son breves; los detalles se reservan para defectos reproducibles, riesgos o decisiones irreversibles.
- Un único gate integrado ejecuta build, lint y suite completa. Review y QA reutilizan esa evidencia cuando sigue siendo válida.
- Un rechazo solo vuelve al carril afectado; no se reinician etapas ya aprobadas.

Esto es especialmente importante en migraciones: se divide por límites estables —proyecto, biblioteca o capa— y no por archivo o clase.

## Roles

| Archivo | Rol | Cuándo se usa |
|---|---|---|
| [cto.md](cto.md) | CTO | Siempre; triage, contratación y coordinación |
| [project-manager.md](project-manager.md) | PM | Alcance ambiguo, grande o con dependencias |
| [senior-developer.md](senior-developer.md) | Dev | Implementación por frontera de archivos |
| [code-reviewer.md](code-reviewer.md) | Reviewer | Riesgo medio/alto o señal de alerta |
| [qa-engineer.md](qa-engineer.md) | QA | Gate de integración o alto riesgo |
| [ux-designer.md](ux-designer.md) | UX | Cambio de interfaz visual |

## Flujo

```text
Tarea → triage CTO → [plan PM si hace falta] → equipo mínimo
      → dev(s) por fronteras independientes → review proporcional
      → gate integrado (build/lint/tests + QA/UX afectado) → cierre
```

El CTO puede ejecutar carriles disjuntos en paralelo. Los carriles que comparten contrato se secuencian. Los topes son tres ciclos de corrección por carril y dos rechazos del gate antes de escalar al usuario.

## Archivos de una tarea

Todos los archivos de una tarea se crean en la carpeta de este equipo (`<ruta-a-team-develop>/tareas/`), **nunca en el proyecto objetivo**, aunque la IA se ejecute desde el proyecto. El CTO resuelve esa ruta como absoluta y se la pasa a cada agente.

```text
<ruta-a-team-develop>/tareas/<AAAA-MM-DD_HHMM>-<slug>/
  00-brief.md       objetivo, riesgo, AC, fronteras y comandos
  00-estado.md      estado canónico, solo lo modifica el CTO
  00-equipo.md      agentes usados y motivo de cada contratación
  01-plan.md        opcional; solo para alcance no trivial
  01-ux-<K>.md      opcional; criterios UX afectados
  02-dev-<K>-<N>.md delta compacto de desarrollo
  03-review-<K>-<N>.md veredicto y hallazgos focalizados
  04-qa-<K>-<M>.md gate de integración
  05-resumen.md     resultado y métricas de coordinación
```

Las plantillas están en [tareas/_plantilla](tareas/_plantilla).

## Uso

```text
Lee <ruta-a-team-develop>/cto.md y ejecuta como CTO la tarea:
"<descripción de la tarea>"
Proyecto: <ruta del proyecto>
```

Ejemplo:

```text
Lee C:/repos/team-develop/cto.md y ejecuta como CTO la tarea:
"Migrar la biblioteca de validación de Python a C# conservando la API pública y sus pruebas"
Proyecto: C:/repos/mi-solucion
```

El cierre informará los agentes realmente usados, ampliaciones justificadas, handoffs/palabras estimadas y verificaciones reutilizadas. No requiere dependencias ni APIs de un proveedor específico.
