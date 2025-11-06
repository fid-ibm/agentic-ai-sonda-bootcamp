# Laboratorio AskSonda

## Objetivos
El objetivo de este laboratorio es enseñarle cómo crear un agente utilizando la interfaz de usuario SAAS de Watsonx Orchestrate. El agente podrá responder preguntas específicas utilizando información (o conocimientos) cargados específicamente. El agente que crearemos hoy, AskSonda, podrá interactuar y responder en español para contestar preguntas sobre Sonda.

## Introducción
AskSonda es un agente conversacional especializado diseñado para asistir al equipo comercial de SONDA. Este agente utiliza una base de conocimientos que contiene documentación oficial de la empresa, incluyendo informes financieros, presentaciones corporativas y reportes integrados. A través de esta base de conocimientos, AskSonda puede responder preguntas sobre los servicios de SONDA, sus resultados financieros, iniciativas estratégicas y cualquier información relevante para el equipo de ventas. El agente está configurado para responder exclusivamente en español y puede generar documentos en formato PDF cuando sea necesario, facilitando la creación de presentaciones y materiales de ventas personalizados. En este laboratorio aprenderás a configurar desde cero un agente con conocimientos especializados, definir su comportamiento y añadir herramientas que extienden sus capacidades.

## Requisitos previos
Su instructor debería haber compartido con usted una instancia de la interfaz de usuario de Watsonx Orchestrate que estará preconfigurado con todo lo que necesita para este laboratorio.

## Guía paso a paso para crear AskSonda


### Comience a crear el agente:

1. Comencemos por acceder a Watsonx Orchestrate ingresando en el siguiente [enlace](https://dl.watson-orchestrate.ibm.com/) e iniciamos sesión con nuestro IBMid. ![ALT TEXT](./screenshots/login-wxo.png)

2. En la página `Manage Agents` de Watsonx Orchestrate, haga clic en el botón `Create Agent`.
![ALT TEXT](./screenshots/create-agent.png)

1. En la página `Create an Agent`, añadir lo siguiente `Name` y `Description` a continuación, pulse `Create`
    - **Name**: `AskSonda`
        - Este es el nombre de tu agente, así es como aparecerá y será referido por otros agentes.
    - **Description**: `Agente facilitador para el equipo comercial de SONDA`
        - La descripción es una breve reseña de lo que hace el agente. Otros agentes la utilizan para determinar cuándo reenviar solicitudes a un colaborador.

![ALT TEXT](./screenshots/name-desc.png)

5. Esto le llevará al `Agent Editor` Desplácese hacia abajo y cambie el `Welcome Message` y las `Quickstart Prompts`.
    - **Welcome Message**: `Hola, soy AskSonda. ¿En qué puedo ayudarte hoy?`
        - El mensaje de bienvenida es el texto en letra grande que aparecerá en el chat para saludar a tu usuario.
    - **Quickstart Prompts**:
        1. `Dame un discurso de 2 minutos para presentar Sonda a un cliente`
        2. `¿Cuáles fueron los resultados financieros de Sonda en el 2024?`
        3. `Elabora un mensaje para clientes basado en los resultados 2024.`
        - Las indicaciones de inicio rápido son preguntas preestablecidas que el usuario puede plantear al LLM con solo seleccione un botón. Se muestran debajo del mensaje de bienvenida.

![ALT TEXT](./screenshots/welcome-quickstarts.png)

6. Actualiza el chat para ver tus actualizaciones en la ventana de chat.

![ALT TEXT](./screenshots/refresh.png)

Ahora puedes empezar a ver una versión muy básica de AskSonda. Aún no hemos añadido ninguna funcionalidad real, pero la interfaz ya está tomando forma.

### Añadir una base de conocimiento
Una base de conocimientos es una base de datos en lenguaje natural (codificada mediante vectores semánticos) que proporciona información adicional a un agente, a la que este puede acceder y buscar cuando sea necesario. Añadamos algunos conocimientos a AskSonda:

7. Descargar todos los archivos de conocimiento desde <a href="https://ibm.box.com/s/6tp60d39z76bt0ms5je2q41katl4loo8" target="_blank">esta carpeta box</a>.

8. Desplácese hacia abajo, pasando `Agent Style`, hasta la sección `Knowledge`. Seleccione `Choose Knowledge`.

![ALT TEXT](./screenshots/choose-knowledge.png)

9. Desplácese hacia abajo en la ventana emergente `Choose knowledge source` para seleccionar `Upload files` y pulse `Next`.

![ALT TEXT](./screenshots/upload-files.png)

10. Seleccione `Drag and drop files here or click to upload`

![ALT TEXT](./screenshots/open-file-select.png)

11. Seleccione y abra todos los archivos **<u>excepto</u>** `Reporte Integrado Sonda 2024.pdf`. La carga de archivos tiene un límite de 30 MB, por lo que tendremos que repetir este proceso una segunda vez.

![ALT TEXT](./screenshots/upload-many.png)

12. Seleccione el botón `Next` y pega lo siguiente en la sección `Description`: 

```
Esta es la fuente de conocimiento oficial para el agente Sonda. Su conocimiento se base en los siguientes documentos: - Flipbook.pdf - Informe de Prensa SONDA Abril 2025.pdf - Informe de Prensa SONDA Enero 2025.pdf - Informe de Prensa SONDA Julio 2025.pdf - Informe de Prensa SONDA Junio 2025.pdf - Informe de Prensa SONDA Marzo 2025.pdf - Informe de Prensa SONDA Mayo 2025.pdf - Presentacion Corporativa ESP 2025.pdf - Reporte Integrado Sonda 2024.pdf
```

La descripción de la base de conocimiento proporciona al agente información sobre el contenido de dicha base y, en ocasiones, incluso instrucciones directas sobre cuándo debe acceder a él. 

![ALT TEXT](./screenshots/next-many.png)
![ALT TEXT](./screenshots/save-many.png)

13. Seleccione `Save` para cargar los conocimientos y espera a que se procesen. La plataforma necesita indexar la base de conocimientos, lo que tardará un minuto.

![ALT TEXT](./screenshots/wait-many.png)

14. Sin salir de la pantalla, seleccione `Upload files` y seleccione el botón `Drag and drop files here or click to upload`.

![ALT TEXT](./screenshots/open-upload-last.png)
![ALT TEXT](./screenshots/drop-zone-last.png)

15. Seleccione, abra y cargue `Reporte Integrado Sonda 2024.pdf` por separado. 

![ALT TEXT](./screenshots/select-last.png)
![ALT TEXT](./screenshots/upload-last.png)

16. Ahora, seleccione `Edit Knowledge Settings`, vaya a la pestaña `Dynamic (Preview) tab` y pulse `Save`. 

El modo dinámico permite al agente decidir qué hacer con el conocimiento en lugar de limitarse a devolver la información. Esto es lo que permitirá a AskSonda dar formato a sus respuestas basadas en el conocimiento según nuestras preferencias.

![ALT TEXT](./screenshots/knowledge-settings.png)
![ALT TEXT](./screenshots/dynamic.png)
![ALT TEXT](./screenshots/save-dynamic.png)

### Configurar el LLM
También podemos cambiar el LLM que alimenta a nuestro agente. Esto proporciona flexibilidad entre diferentes LLMs ofrecer diferentes ventajas para nuestros agentes.

17. Pulse el menú desplegable del modelo y seleccione `Bedrock Sonnet 3.5`

![ALT TEXT](./screenshots/open-llm-box.png)
![ALT TEXT](./screenshots/select-bedrock-sonnet.png)

### Definir comportamiento
La sección de comportamiento es donde escribimos las indicaciones del sistema para nuestro agente: las instrucciones que le dirán a nuestro agente qué hacer y cuándo hacerlo. El diseño de las indicaciones es crucial para garantizar un comportamiento preciso y eficiente en todo momento.

18. Desplácese hacia abajo hasta la sección `Behavior` y pegue el siguiente comando:
    ```
    # Guía de instrucciones
    Eres un asistente especializado de SONDA diseñado para ayudar al equipo comercial. Responde únicamente en español y mantén una comunicación clara, profesional y concisa.

    ## Presentación inicial
    - Cuando el usuario te salude por primera vez, preséntate así: "Soy un asistente de SONDA ¿En qué te puedo ayudar?"
    - De lo contrario, continúa con el Paso 1 y consulta la base de conocimientos para responder a su pregunta.
        - No digas nada antes de obtener información de la base de conocimientos, ni siquiera le digas al usuario que vas a obtener o buscar información.

    ## Objetivo principal
    Ayudar a los empleados de SONDA a completar sus tareas de manera eficiente, proporcionando información precisa basada en documentación oficial de SONDA.

    ## Paso 1: Consultar la base de conocimiento
    - **Fuente de información**: Tienes acceso a una base de conocimiento (`knowledge_for_agent`) que contiene toda la documentación oficial de SONDA.
    - **Acción**: Para cada consulta del usuario, busca primero en la base de conocimiento la información más relevante y actualizada.  **No** le diga nada al usuario hasta que haya obtenido la información de la base de conocimientos.
    - **Contexto**: Considera todo el contexto de la conversación para proporcionar respuestas precisas y coherentes.

    ## Paso 2: Elaborar respuestas de calidad
    - Transforma la información recuperada en una respuesta **clara, completa y bien estructurada**.
    - Utiliza formato Markdown para mejorar la legibilidad pero utilice listas solo cuando sea necesario.
    - Asegúrate de que la respuesta sea **comprehensiva y no fragmentada**.
    - Sé específico y proporciona datos concretos cuando estén disponibles.
    - Tus respuestas deben ser completas y extensas.

    ## Paso 3: Formato de salida estándar
    Todas sus respuestas deben tener **3 PARTES**, como se indica a continuación:
    1. **TÍTULO** de su respuesta. El título debe estar marcado como encabezado. Genere siempre un título.
    2. **CUERPO PRINCIPAL** de su respuesta.
    3. **Una sección con un título en NEGRITA, redactada exactamente como "¿Quieres explorar más sobre este tema?"**. En esta sección, ofrece de 2 a 3 preguntas de seguimiento relevantes y específicas que tienes el conocimiento para responder y que el usuario puede hacer para obtener más información sobre SONDA. Puedes ofrecer los siguientes tipos de preguntas:
        - Una pregunta específica sobre el tema.
        - Una pregunta que profundice en un aspecto específico.
        - Una pregunta sobre aplicaciones prácticas o casos de uso.

        ### Ejemplo de estructura a seguir:
        ```
        [Título - marcado como encabezado]

        [Cuerpo - respuesta completa con información oficial de SONDA, incluyendo datos específicos, cifras y detalles relevantes en formato Markdown]

        [¿Quieres explorar más sobre este tema? - Preguntas de seguimiento]
        ```

    ## OPCIONAL: Generación de documentos
    **Instrucciones importantes**
    - **No ofrezca generar un PDF** al usuario a menos que lo solicite explícitamente.
    - Cuando el usuario solicite un PDF, utilice la herramienta `generate_pdf_v2` para crearlo.

    **Formato de salida para mostrar el documento**
    1. Genere un resumen que indique al usuario que el documento está listo. Nada más.
    2. No permita que el usuario vea lo que está pensando.
    3. No muestre preguntas de seguimiento.
    ```

![ALT TEXT](./screenshots/behavior.png)

Tómate un segundo para asegurarte de que se mantengan el formato y la sangría.

### Añadir una herramienta
Las herramientas permiten a los agentes llamar a funciones predefinidas y realizar acciones como consultar datos, crear documentos o ejecutar transacciones en nombre de los usuarios. Para AskSonda, vamos a añadir una herramienta generadora de archivos PDF.

19. Desplácese hacia arriba hasta la sección `Toolset` y pulse `Add tool`.

![ALT TEXT](./screenshots/add-tool.png)

20. Seleccionar `Add from local instance`

![ALT TEXT](./screenshots/add-tool-local.png)

21. Busque y seleccione la herramienta `generate_pdf_v2`. A continuación, pulse `Add to agent`.

![ALT TEXT](./screenshots/search-gen-pdf.png)
![ALT TEXT](./screenshots/select-gen-pdf.png)


### Y eso es todo.
¡Felicitaciones, has configurado AskSonda correctamente! 
![AlT TEXT](./screenshots/final.png)

## Probando AskSonda
¡Pongamos a prueba AskSonda!

1. Primero, pulse la primera indicación del iniciador para preguntar a nuestro nuevo agente.: `Dame un discurso de 2 minutos para presentar Sonda a un cliente`

![ALT TEXT](./screenshots/test-prompt-1.png)

Después de acceder a su base de conocimientos, AskSonda responderá:
![ALT TEXT](./screenshots/test1.png)

Puede ver su razonamiento abriendo `Show Reasoning` en la parte superior del indicador y luego expandiendo `Step 1`.

![ALT TEXT](./screenshots/test-show-reasoning.png)

2. Probemos nuestra herramienta de generación de PDF con: `Genérame un pdf con esta información`.

![ALT TEXT](./screenshots/test-pdf.png)

3. Descargar el pdf

![ALT TEXT](./screenshots/test-open-pdf.png)

4. Abre el PDF, el agente lo habrá preformateado para ti.

![ALT TEXT](./screenshots/pdf.png)

5. Ahora, reinicia tu chat utilizando el botón de actualización.

![ALT TEXT](./screenshots/test-reset.png)

6. Genial, probemos con la segunda sugerencia inicial.

![ALT TEXT](./screenshots/test-prompt-2.png)
![ALT TEXT](./screenshots/test-response-2.png)

7. Y, por último, prueba la tercera indicación (tendrás que restablecer el chat de nuevo).

![ALT TEXT](./screenshots/refresh.png)
![ALT TEXT](./screenshots/test-prompt-3.png)
![ALT TEXT](./screenshots/test-response-3.png)

