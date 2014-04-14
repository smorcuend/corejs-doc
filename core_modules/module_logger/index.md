---
layout: page
---

Módulo: Logger
==============

El logger nos ayuda a registrar los mensajes por consola de la aplicación, establecer diferentes niveles de mensajes, y enviar información relevante a un servidor específico para posterior análisis.

Ejemplo de uso:

```javascript
// config & params are objects
app.log.info('engine', 'Logger.setConfig', config, true, [3], 3.14);
app.log.debug('engine:config:loaded', config);
app.log.debug('api.request', params);
```

Se recomienda además seguir la siguiente convención con el fin de pder filtrar el log como explicaremos más adelante:

```javascript
app.log.debug('moduleName', 'ClassName.methodName' [, ...]);
app.log.debug('moduleName:event:name' [, ...]);
app.log.debug('moduleName.method.message' [, ...]);
```

* **Niveles de Log**
Los errores ordenados de mayor a menor nivel son los siguientes:
  * `SILENT`: Este nivel sirve para anular el log 
  * `ERROR`: Muestra por consola los errores de la aplicación
  * `WARN`: Muestra las alertas de la aplicación y mensajes de mayor nivel
  * `INFO`: Muestra las trazas de información y mensajes de mayor nivel
  * `DEBUG`: Muestra mensajes de depuración y mensajes de mayor nivel
  * `TRACE`: Muestra trazas de desarrollo y mensajoes de mayor nivel
  * `ALL`: Muestra todos los mensajes

```javascript
app.log.setLevel(app.log.level.INFO);
```

* **Filtrar Log**

A veces es necesario filtrar el log para que muestre únicamente las trazas de log que nos interesan. Para filtrar todos los logs que no sean de una sección en particular podemos hacer:

```javascript
app.log.filter('engine');
```

Con ésto conseguimos que sólo se muestran las trazas que comiencen por la palabra `engine`.


* **Log To Server**

El log tiene la capacidad de enviar cierta inforamción a un servidor específico, para ello, primero hay que **configurarlo y habilitarlo**

```javascript
var config = {
    logToServer: true,          // Para habilitar envío de datos al servidor
    logBuffer: 10,              // Nº de trazas antes de enviar al servidor
    logServerEndpoint: 'url'    // Ruta hasta el servidor que almacena logs
    logLevel: app.log.level.WARN// Nivel de log a enviar
}
app.log.setConfig(config);
```

* **Nota**: Ala hora de construir los mensajes de log, hay que tener en cuenta que si el mensaje que se construye es complejo, es una buena práctica detectar el nivel de log establecido para evitar tener que construir mensajes innecesarios, por ejemplo:

```javascript
var complexLogMessageBuilder = function() {
    // Complex code here
    return logString;
}

if (app.log.getLevel() === app.log.level.INFO) {
    app.log.info(complexLogMessageBuilder());
}
```