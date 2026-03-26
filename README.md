# -Redes-y-Protocolos-para-Developers
> Investigación guiada sobre conceptos base de redes aplicados al desarrollo full stack.

---

## 🧩 1. Conceptos Base

| Concepto | Definición simple | Nivel técnico breve | Ejemplo real |
|----------|-------------------|---------------------|--------------|
| **TCP** | Protocolo que asegura que los datos lleguen completos y en orden. Es como enviar una carta certificada. | Establece una conexión antes de transmitir datos. Verifica que cada paquete llegó correctamente. | Cargar una página web, enviar un formulario. |
| **UDP** | Protocolo que envía datos sin verificar si llegaron. Es más rápido pero sin garantías. | No establece conexión previa. Envía paquetes sin esperar confirmación. | Videollamadas, streaming de video, juegos en línea. |
| **Puerto** | Número que identifica a qué aplicación va dirigida la comunicación dentro de un servidor. | Valor entre 0 y 65535. Permite que múltiples servicios corran en la misma máquina. | Puerto 3000 para un servidor local de Node.js. |
| **HTTP** | Protocolo que define cómo el navegador y el servidor se comunican para intercambiar información. | Funciona sobre TCP. Usa verbos (GET, POST, etc.) para indicar la acción deseada. | Cuando entras a una página web, tu navegador hace una petición HTTP. |

---

## ⚙️ 2. TCP vs UDP

### Diferencias clave

**¿Cuál es la principal diferencia?**
TCP verifica que los datos lleguen correctamente (conexión confiable), mientras que UDP los envía sin confirmación (más rápido pero sin garantías).

**¿Cuál es más confiable?**
**TCP**, porque establece una conexión antes de enviar datos y confirma que cada paquete llegó. Si algo falla, lo reenvía.

**¿Cuál es más rápido?**
**UDP**, porque no espera confirmaciones ni establece conexión previa. Simplemente dispara los datos y sigue adelante.

| Protocolo | Característica clave | Uso típico |
|-----------|----------------------|------------|
| **TCP** | Confiable, ordenado, con confirmación de entrega | Navegación web, emails, APIs REST |
| **UDP** | Rápido, sin confirmación, tolera pérdida de datos | Streaming, videojuegos, videollamadas |

---

## 🚪 3. Puertos (clave en desarrollo)

**¿Qué es un puerto en una red?**
Es un número que identifica una "puerta de entrada" específica dentro de un servidor o computadora. Permite que diferentes aplicaciones reciben datos por separado en la misma máquina.

**¿Por qué una aplicación necesita un puerto?**
Porque varios servicios pueden correr al mismo tiempo en una máquina. El puerto le dice al sistema operativo *a quién* va dirigida cada comunicación.

**¿Qué significan estos puertos?**

| Puerto | Significado |
|--------|-------------|
| `80` | HTTP — tráfico web sin cifrar (ej: `http://`) |
| `443` | HTTPS — tráfico web cifrado (ej: `https://`) |
| `3000` | Puerto común para servidores de desarrollo en Node.js/Vite |
| `5432` | Puerto por defecto de PostgreSQL |

### 👩‍💻 Ejemplo developer

Cuando ejecutas:
```bash
npm run dev
```
Tu aplicación levanta un servidor local que escucha en un puerto (por ejemplo el `5173` en Vite o `3000` en Express). Eso significa que puedes acceder desde tu navegador en `http://localhost:3000` y la comunicación viaja por ese puerto.

---

## 4. HTTP (HyperText Transfer Protocol)

**¿Qué es HTTP?**
Es el protocolo que define las reglas de comunicación entre el navegador (cliente) y el servidor. Es el "idioma" que usan para entenderse.

**¿Cómo funciona una petición HTTP?**
El cliente envía una *request* con un verbo, una URL y opcionalmente datos. El servidor procesa la solicitud y devuelve una *response* con un código de estado y los datos solicitados.

**Verbos HTTP:**

| Verbo | Significado |
|-------|-------------|
| `GET` | Solicitar/obtener datos del servidor |
| `POST` | Enviar datos nuevos al servidor (crear) |
| `PUT` | Actualizar un recurso existente completo |
| `DELETE` | Eliminar un recurso del servidor |

### 🔄 Flujo de una petición


---

## 5. Conexión directa con desarrollo (CRÍTICO)

Cuando ejecutas:
```javascript
fetch("http://localhost:3000/api")
```

| Pregunta | Respuesta |
|----------|-----------|
| ¿Qué protocolo se usa? | **HTTP** — definido en la URL |
| ¿Qué puerto? | **3000** — especificado en la URL |
| ¿Qué tipo de comunicación? | **Cliente-Servidor** — el frontend (cliente) hace una petición al backend (servidor) local |

El navegador envía una petición HTTP GET al servidor que está corriendo en tu propia máquina (`localhost`) en el puerto `3000`.

---

## 6. Problemas reales (Debugging)

| Error | Significado |
|-------|-------------|
| `"Port already in use"` | Otro proceso ya está usando ese puerto. No puedes tener dos servicios en el mismo puerto al mismo tiempo. |
| `"Connection refused"` | El servidor no está corriendo o no escucha en ese puerto/IP. No hay nadie "del otro lado". |
| `"Timeout"` | La solicitud tardó demasiado y no recibió respuesta. Puede ser red lenta, servidor caído o firewall bloqueando. |

**¿Qué revisará cómo developer?**
- Si el servidor backend está corriendo (`npm run dev` / `java -jar`)
- En qué puerto está escuchando el servidor
- Si el puerto está bloqueado por firewall o en uso por otro proceso
- Si la URL en el `fetch` coincide exactamente con el host y puerto del servidor

---

## 7. Caso práctico real

**Escenario: "Tu frontend no se conecta al backend"**

| Causa posible | ¿Puede ser? | Qué revisar |
|---------------|-------------|-------------|
| Problema de **puerto** | ✅ Sí | Verificar que el backend corre en el mismo puerto que usas en el `fetch` (ej: `3000`) |
| Problema de **protocolo** | ✅ Sí | Confirmar que usas `http://` en local y `https://` en producción |
| Problema de **red** | ✅ Sí | Revisar si hay CORS mal configurado, firewall bloqueando, o si `localhost` está mal escrito |

---

## 8. Analogía

| Concepto | Analogía |
|----------|----------|
| **TCP** | 📞 Llamada telefónica — primero marcas, esperas que contesten, luego hablas. Hay confirmación de que el mensaje llegó. |
| **UDP** | 📻 Radio — transmites al aire sin saber si alguien escucha. Más rápido, pero sin garantías. |
| **Puerto** | 🚪 Número de departamento — el edificio es el servidor (IP), pero cada departamento (puerto) es una aplicación distinta. |
| **HTTP** | 🗣 ️ Idioma de comunicación — es el lenguaje que usan el navegador y el servidor para entenderse. |

---

## ✨ BONUS

**¿Qué es HTTPS?**
Es HTTP con una capa de seguridad (SSL/TLS). Los datos viajan cifrados, por lo que nadie puede interceptarlos. En producción siempre se usa HTTPS. Usa el puerto `443` por defecto.

**¿Qué es REST?**
Es un estilo de arquitectura para diseñar APIs. Define reglas para usar HTTP correctamente: URLs que representan recursos (`/users`, `/products`) y verbos HTTP para las acciones (GET, POST, PUT, DELETE).

**¿Qué es un endpoint?**
Es una URL específica de una API que recibe peticiones. Es el "punto de entrada" a un recurso.


---

*📚 Investigación realizada como parte del bootcamp Fullstack Java — Generation Chile*

