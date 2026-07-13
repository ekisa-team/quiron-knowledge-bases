# Bases de conocimiento | Quirón

Este repositorio contiene las bases de conocimiento estáticas de todos los clientes de **Quirón**, que luego se vinculan en [Kivox](https://kivox.com.co). Cada cliente vive en su propia carpeta en la raíz del repositorio. Este README define cómo se escribe, revisa y publica contenido para cualquier cliente, actual o futuro.

- [Documentación oficial de Kivox](https://docs.kivox.com.co)
- [Mejores prácticas de bases de conocimiento (RAG)](https://docs.kivox.com.co/knowledge-bases/best-practices)
- [Conceptos principales de fragmentación (chunking)](https://docs.kivox.com.co/knowledge-bases/introduction#conceptos-principales)

Si por algún motivo, algo en este README entra en conflicto con la documentación oficial de Kivox, **la documentación de Kivox es la fuente de verdad**. Este archivo es una capa de convenciones internas de Ekisa que construye sobre esa base.

## 1. Modelo de operación: "[Documentation as Code](https://www.writethedocs.org/guide/docs-as-code/)"

Las bases de conocimiento de los clientes de Quirón **no son administradas directamente por los clientes**, sino centralizadamente por un equipo técnico de Ekisa mediante este repositorio de GitHub. Todo cambio, para cualquier cliente, se gestiona por [Pull Request (PR)](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests), con revisión obligatoria antes de fusionarse a `main`. La revisión obligatoria es importante porque estos documentos no sirven únicamente como documentación, sino que influyen directamente en las respuestas y comportamiento de los agentes de Kivox, por lo que la calidad del contenido debe ser muy alta de acuerdo con las buenas prácticas de gestión de bases de conocimiento.

### Flujo de trabajo actual (manual)

1. El contenido se redacta y se revisa **en este repositorio**, dentro de la carpeta del cliente correspondiente.
2. Se abre un PR con los archivos `.md` nuevos o modificados.
3. Un revisor humano aprueba el PR verificando que cumple este README y las buenas prácticas de Kivox.
4. Una vez fusionado el PR, **el archivo se sube manualmente a la instancia de Kivox de ese cliente** (opción **Conocimiento** en Kivox) para que quede indexado.
5. Se valida en Kivox cómo quedó fragmentado el documento, siguiendo la guía de conceptos principales enlazada arriba.

> [!IMPORTANT]
> Hoy no existe sincronización automática entre este repositorio y Kivox. Si un PR se fusiona pero nadie sube el archivo actualizado a la plataforma, el agente de ese cliente seguirá respondiendo con la versión anterior. Quien fusiona el PR es responsable de subir el archivo o de asignar esa tarea explícitamente a alguien por medio de un [Issue](https://github.com/features/issues).
>
> Recordar también que cada cliente tiene su propia instancia/base de conocimiento en Kivox: subir un archivo de `oralaser/` a la instancia equivocada mezclaría las bases de conocimiento de dos clientes distintos. Verificar siempre a qué cliente pertenece antes de subir.

### Flujo de trabajo futuro (planeado)

Kivox tiene planeado un editor y sistema de archivos integrado, que permitiría escribir y publicar directamente sobre la plataforma sin el paso manual de subida. Cuando esa función esté disponible:

- Este repositorio de GitHub seguirá siendo la fuente de verdad versionada (historial, PRs, revisión de pares) para todos los clientes.
- El paso 4 del flujo actual (subida manual) se reemplazará por una sincronización o publicación directa desde el editor de Kivox, probablemente conectada por cliente.
- Este README se actualizará con el nuevo flujo tan pronto la función esté disponible; hasta entonces, se sigue el proceso manual descrito arriba.

## 2. Estructura del repositorio

```text
quiron-knowledge-bases/
├── README.md
├── oralaser/                      # cliente 1
│   ├── 00_corporativo/
│   ├── 10_urgencias/
│   ├── 20_especialidades/
│   ├── 30_estetica/
│   └── 40_servicios_exclusivos/
├── <otro-cliente>/                # cliente 2, misma lógica, dominios propios
│   ├── 00_corporativo/
│   └── ...
└── <otro-cliente-mas>/
    └── ...
```

La raíz del repositorio nunca contiene archivos `.md` de conocimiento. Cada cliente es una carpeta de primer nivel. Todo lo que va dentro de la carpeta de un cliente (dominios, archivos, numeración) sigue las mismas reglas descritas en este documento, pero los **dominios** (los nombres de las carpetas de segundo nivel, como `20_especialidades/`) se definen según el negocio real de cada cliente, no se copian mecánicamente de `oralaser/`.

### Convención de nombres de carpeta de cliente

- Minúsculas, sin espacios, sin tildes ni caracteres especiales: `oralaser`, no `OralLaser`, `oral-laser`, `Oral Laser`, ni `Oral_Laser`.
- Un solo cliente por carpeta, incluso si el cliente tiene varias marcas o sedes bajo el mismo contrato; en ese caso, la separación se maneja con subcarpetas de dominio, no con carpetas adicionales de primer nivel.
- Si un cliente cambia de nombre comercial, se renombra la carpeta en un PR dedicado exclusivamente a ese cambio (no mezclado con edición de contenido), para que el historial de Git rastree el movimiento correctamente.

### Convención de numeración con huecos ("gap numbering")

Dentro de la carpeta de cada cliente, tanto las carpetas de dominio como los archivos usan un prefijo numérico en incrementos de 10 (`10_`, `20_`, `30_`...), al estilo clásico de `rc.d` en sistemas Unix.

**Motivo:** dejar huecos intermedios (`15_`, `25_`, `35_`...) para insertar un nuevo dominio o archivo en el futuro sin tener que renombrar (y romper enlaces a) todo lo que ya existe.

**Reglas:**

- Nunca reutilizar un número que ya fue asignado a otro archivo dentro de esa misma carpeta, aunque ese archivo se haya eliminado.
- Si necesita insertar contenido entre `20_` y `30_`, usar `25_`. Si ya no queda espacio (por ejemplo entre `24_` y `25_`), es señal de que esa carpeta necesita una renumeración completa. Hacerlo en un PR dedicado, nunca mezclado con cambios de contenido para poder rastrear el movimiento correctamente.
- Los prefijos indican únicamente orden de lectura sugerido, no prioridad ni importancia editorial.
- Esta convención es la misma para todos los clientes; lo que cambia de cliente a cliente son los nombres y la cantidad de dominios, no la lógica de numeración.

## 3. Cómo agregar un cliente nuevo

1. Crear la carpeta del cliente en la raíz del repositorio, siguiendo la convención de nombres de la sección 2.
2. Definir los dominios reales de ese cliente (no copiar mecánicamente los de `oralaser/` sin pensarlos). Usar `oralaser/` como referencia de **estructura y disciplina**, no como plantilla literal de contenido.
3. Se sugiere crear primero los dominios más estables (información corporativa, contacto, horarios) antes que los dominios de producto/servicio, que suelen iterar más.
4. Aplicar desde el primer archivo todas las reglas de la sección 4 (contenido) y 5 (estilo). No hay "modo borrador flexible" para clientes nuevos: las mismas reglas de calidad de día uno.
5. Abrir el PR inicial con la estructura de carpetas vacía (usar `.gitkeep`) o con los primeros 2-3 archivos, para que la persona encargada de la revisión pueda validar la taxonomía de dominios antes de que se escriba todo el contenido.

Ejemplo de dominios usados actualmente para `oralaser/`: `00_corporativo`, `10_urgencias`, `20_especialidades`, `30_estetica`, `40_servicios_exclusivos`. Para un cliente similar, la carpeta `00_corporativo/` normalmente se mantiene con ese mismo propósito (historia, misión/visión, sedes, horarios, contacto) porque es información transversal a cualquier tipo de negocio.

## 4. Reglas de contenido

Estas reglas no constituyen simples convenciones estilísticas o estéticas, sino requisitos indispensables de ingeniería de datos para el correcto funcionamiento de un sistema RAG.

En la arquitectura RAG, el modelo de lenguaje (LLM) genera respuestas basándose únicamente en los fragmentos de texto que el sistema de búsqueda semántica extrae de la base de conocimiento. Si un fragmento recuperado es ambiguo, carece de contexto o contiene información imprecisa, el modelo se verá forzado a deducir, asumir o completar la información faltante por su cuenta, lo cual desencadena "alucinaciones" (respuestas inventadas o incorrectas).

Por esta razón, la adherencia a estas reglas es estricta y no debe flexibilizarse por razones de practicidad o tiempos de entrega, ya que la fiabilidad y precisión de cualquier agente de Kivox dependen directamente de la calidad y el diseño de su base de conocimiento:

1. **Un solo tema por archivo:** Se debe evitar mezclar diferentes servicios o combinar información de atención al cliente con publicidad en el mismo documento. Mantener un tema único por archivo ayuda a que el modelo encuentre la respuesta exacta de forma inmediata.
2. **Sin datos que cambien constantemente:** Los precios, horarios disponibles, promociones del mes o el estado de las citas no deben incluirse en los documentos. Esa información cambia con frecuencia y se consulta mediante sistemas en tiempo real (APIs). Si no hay una conexión activa a estos sistemas, se prefiere omitir el dato en lugar de escribir respuestas provisionales.
3. **Uso de nombres propios y específicos:** Se debe evitar el uso de palabras vagas como "la empresa", "el producto" o "el sistema". En su lugar, se escribe siempre el nombre de la marca o del servicio. Esto permite que cualquier fragmento de texto se entienda por sí mismo, sin necesidad de leer todo el documento.
4. **Información confirmada y sin rodeos:** Se deben evitar respuestas que no ofrezcan una solución real (como "permítame revisar la información"). Si un dato no está confirmado, simplemente no se incluye en la base de conocimiento hasta tener la certeza de su veracidad.
5. **Exclusión de guías de comportamiento o personalidad:** Las indicaciones sobre cómo debe hablar el agente (por ejemplo: "sé amable", "usa emojis" o "pide disculpas") no pertenecen a estos documentos. Esa configuración se realiza en las instrucciones generales del agente (Prompt del sistema), dejando la base de conocimiento exclusiva para hechos y datos de la organización.
6. **Una sola versión actualizada:** Conservar archivos duplicados o información desactualizada genera respuestas contradictorias. Ante cualquier cambio en un servicio o política, se modifica el documento original, eliminando la versión anterior.
7. **Texto redactado de forma original:** Se debe evitar copiar y pegar textos directamente de la página web del cliente o de la competencia. La información debe ser reescrita de manera directa, sencilla y adaptada para el agente la procese de la forma más eficiente posible.
8. **Separación total entre marcas:** La base de conocimiento de cada cliente debe ser completamente independiente. No se deben incluir comparaciones, ejemplos o menciones de otras empresas dentro de la carpeta de un cliente específico.

### 5. Estilo de redacción de los archivos base

A diferencia de las guías sobre el tono de la conversación (las cuales pertenecen a la configuración general del agente), las siguientes pautas definen cómo redactar el contenido de los documentos:

1. **Redacción en tercera persona:** Se utiliza una perspectiva impersonal que mencione siempre al cliente por su nombre propio (por ejemplo, "Oral Láser ofrece..." en lugar de "ofrecemos...").
2. **Uso de datos objetivos:** Se prefieren las afirmaciones claras y comprobables. Se omiten adjetivos publicitarios o subjetivos (como "excelente" o "líder"), a menos que estén respaldados por datos específicos, premios o certificaciones verificables.
3. **Títulos claros y ordenados:** Se estructuran los temas utilizando títulos jerárquicos para facilitar que el sistema reconozca con precisión dónde empieza y dónde termina cada sección.
4. **Estructuración con viñetas:** Las listas de servicios, direcciones o características se presentan utilizando viñetas en lugar de párrafos continuos.
5. **Evitar tablas complejas:** Se recomienda prescindir de las tablas de datos para organizar la información. Es preferible utilizar subtítulos y texto sencillo, ya que los modelos LLM suelen tener dificultades para interpretar tablas de forma correcta.
6. **No duplicar información:** Si un tema ya se encuentra desarrollado en otro archivo, se incluye una referencia directa al documento original en lugar de copiar y pegar el mismo contenido.
7. **Control de información pendiente:** Los datos que requieran confirmación no se omiten en silencio, sino que se señalan de forma clara dentro del archivo para su posterior verificación.

## 6. Qué hacer cuando falta un dato o no se puede verificar

Nunca completar un vacío de información con una suposición razonable, sin importar el cliente. En su lugar, usar este formato al final de la sección afectada:

```md
> Nota: [descripción breve de qué dato falta o no está confirmado en la fuente oficial]. Verificar con el equipo [clínico/comercial/legal] antes de publicar como dato final.
```

Esto le permite a cualquier persona de Ekisa saber exactamente qué falta, sin que ese vacío se convierta en una alucinación del agente ni en un dato inventado que termine indexado como si fuera verdad.

## 7. Después de fusionar el PR (recordatorio del paso manual)

Fusionar el PR en GitHub no actualiza Kivox automáticamente ([ver sección 1](#1-modelo-de-operación-documentation-as-code)). Quien fusiona debe:

1. Confirmar a qué cliente e instancia de Kivox pertenece el archivo modificado.
2. Subir el archivo `.md` actualizado al panel de Conocimiento de Kivox de ese cliente específico.
3. Revisar en Kivox cómo quedó fragmentado el documento (sin cortes de oraciones a la mitad, sin pérdida de encabezados).
4. Marcar el PR como "publicado en Kivox" usando la etiqueta correspondiente.

Este paso desaparecerá cuando Kivox habilite su editor y sistema de archivos integrado; hasta entonces, es responsabilidad de quien fusiona, para cualquier cliente del repositorio.

También es posible que la persona que fusione el PR asigne a un responsable para hacer la integración y publicación en Kivox.
