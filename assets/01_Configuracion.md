# Informe de Práctica: Instalación, Configuración Inicial y Primer Endpoint en NestJS


---

## 1. Introducción y Requisitos del Entorno

En esta práctica se configuró un entorno de desarrollo para **NestJS**, un framework backend de Node.js estructurado y basado en TypeScript que utiliza conceptos de inyección de dependencias similares a Spring Boot.

### Verificación de Versiones de Software
Para asegurar la compatibilidad de las herramientas instaladas, se ejecutaron comandos de verificación en la consola. A continuación, se presenta la captura correspondiente que evidencia el correcto funcionamiento de Node.js, pnpm y el CLI de NestJS:

![Verificación del Entorno](assets/verificacion_env.png)

> [!NOTE]
> Se utilizó la versión **v24.16.0** de Node.js junto con **pnpm 11.6.0**, garantizando un entorno moderno y eficiente para el desarrollo.

---

## 2. Creación del Proyecto y Ejecución del Servidor

El proyecto fue creado con el comando:
```bash
nest new fundamentos01 --package-manager pnpm
```

### Inicialización y Arranque del Servidor
Tras configurar las dependencias necesarias de construcción, se inició el servidor de desarrollo mediante el comando `pnpm start`. 

La captura a continuación muestra las trazas en consola confirmando que todos los controladores y módulos se inicializaron satisfactoriamente, y que el servidor se encuentra escuchando en el puerto `3000`:

![Servidor NestJS Iniciado](assets/servidor_iniciado.png)

> [!TIP]
> La consola muestra que `StatusController` y su ruta correspondiente `/api/status` mapearon correctamente, y el mensaje final certifica: `Nest application successfully started`.

---

## 3. Implementación del Endpoint `/api/status`

Se estructuró un módulo específico denominado `status` para la API de verificación de estado. Los componentes creados y modificados se listan a continuación:

*   **Módulo:** [status.module.ts](file:///c:/Users/MSI/Desktop/PPW/PruebasClases/ia/fundamentos01/src/status/status.module.ts)
*   **Controlador:** [status.controller.ts](file:///c:/Users/MSI/Desktop/PPW/PruebasClases/ia/fundamentos01/src/status/status.controller.ts)
*   **Módulo Raíz:** [app.module.ts](file:///c:/Users/MSI/Desktop/PPW/PruebasClases/ia/fundamentos01/src/app.module.ts) (donde se importó `StatusModule`)

### Estructura de Archivos del Módulo
La verificación de que los archivos se generaron en la ruta correcta se realizó con el comando `dir .\src\status\`:

![Listado del Directorio Status](assets/ls_status.png)

### Lógica de Negocio del Controlador
El controlador expone un método GET que responde a la ruta `/api/status` retornando la fecha actual junto al estado de la aplicación. El código implementado en `status.controller.ts` es:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('api/status')
export class StatusController {
  @Get()
  getStatus() {
    return {
      service: 'NestJS API',
      status: 'running',
      timestamp: new Date().toISOString(),
    };
  }
}
```

---

## 4. Verificación del Endpoint en el Navegador

Una vez que el servidor se encontraba en ejecución, se accedió a la dirección `http://localhost:3000/api/status`. El navegador recibió y renderizó correctamente la estructura JSON esperada:

![Endpoint Funcionando](assets/endpoint_funcionando.png)

> [!IMPORTANT]
> El endpoint responde exitosamente devolviendo un objeto JSON válido con los campos `service`, `status` y `timestamp` actualizados en formato ISO.

---

## 5. Análisis y Conclusiones del Estudiante

### Conceptos Clave Comprendidos
*   **`@Controller()`**: Es un decorador que define una clase como manejadora de peticiones web HTTP. Al asignarle un prefijo (en este caso `'api/status'`), indica el punto de entrada de la URL para todos los endpoints del controlador.
*   **`@Get()`**: Indica al framework que el método subyacente responderá específicamente a peticiones HTTP del tipo GET. La respuesta devuelta por el método se serializa automáticamente a formato JSON.
*   **Módulos (`@Module`)**: NestJS organiza la arquitectura del software en módulos autocontenidos. Cada módulo encapsula sus controladores y proveedores relacionados. El módulo raíz (`AppModule`) importa estos submódulos para construir el grafo de dependencias de la aplicación.

### ¿Cómo funciona el servidor NestJS?
El servidor se levanta llamando al punto de entrada `main.ts`, el cual utiliza `NestFactory.create(AppModule)` para instanciar la aplicación. Por debajo, utiliza un servidor HTTP (por defecto Express.js) para escuchar las conexiones entrantes (por defecto en el puerto `3000`), resolviendo las rutas a través de las anotaciones que se definieron en los controladores y inyectando automáticamente las dependencias declaradas.

### Similitudes encontradas con Spring Boot
NestJS comparte una gran cantidad de similitudes conceptuales y estructurales con Spring Boot (Java):
1.  **Uso de Decoradores vs. Anotaciones**: En NestJS se usan decoradores de TypeScript como `@Controller()`, `@Get()`, `@Module()`, `@Injectable()`. En Spring Boot se usan anotaciones análogas como `@RestController`, `@GetMapping`, `@Configuration`/`@Component` y `@Autowired`.
2.  **Inyección de Dependencias**: Ambos frameworks administran de forma automática la instanciación de servicios y controladores, inyectando las dependencias en los constructores de manera declarativa.
3.  **Arquitectura Modular**: Spring Boot organiza el código en paquetes o configuraciones específicas (como `@SpringBootApplication` y beans), mientras que NestJS lo divide formalmente mediante clases decoradas con `@Module`.
4.  **Estructura TypeScript**: TypeScript introduce tipado estático fuerte, interfaces y clases abstractas, haciendo que la transición desde Java/C# a NestJS sea extremadamente intuitiva para desarrolladores backend empresariales.
