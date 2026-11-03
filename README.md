# BusTracker Escolar — versión conectable a Firebase

Esta carpeta contiene una aplicación web inicial con:
- Vista de alumno y vista de conductor.
- Mapa OpenStreetMap.
- GPS real del dispositivo del conductor.
- Envío de coordenadas a Firebase Realtime Database.
- Recepción en vivo desde otros dispositivos.
- Cálculo aproximado de tiempo de conducción mediante OSRM público (solo pruebas).
- Detección de datos GPS antiguos.

## Para que funcione entre distintos celulares

### A. Crear Firebase
1. Entra a https://console.firebase.google.com/ y crea un proyecto.
2. Agrega una aplicación web en la configuración del proyecto.
3. Copia el objeto de configuración web que Firebase muestra.
4. En el proyecto, habilita Realtime Database.
5. Copia la URL de la base de datos (databaseURL).

### B. Reglas de seguridad — NO abrir la base de datos al público
Esta app inicial está diseñada para pruebas, pero el campo “código demo de conductor” NO es autenticación segura. No uses reglas públicas de lectura/escritura para datos reales. Para producción, hay que agregar Firebase Authentication, roles de alumno/conductor y reglas que solo permitan a conductores autorizados escribir ubicación y a usuarios autorizados leerla.
El archivo `database.rules.example.json` es una plantilla conceptual con acceso cerrado por defecto, no reglas listas para producción. Deben ajustarse junto con autenticación antes de usarla.

### C. Ejecutar desde HTTPS
Los navegadores normalmente requieren HTTPS (o localhost) para el GPS.
- Publica los archivos en un hosting HTTPS como Firebase Hosting, o
- para probar en una computadora con Python: `python -m http.server 8000`, y abre `http://localhost:8000`. Para probar desde un celular externo se necesita HTTPS.

### D. Configurar dentro de la app
1. Abre “Configuración inicial”.
2. Pega el JSON de Firebase.
3. Escribe un código demo de conductor (solo para la prueba).
4. Pon el nombre y las coordenadas de una parada.
5. Pulsa “Guardar configuración”.
6. En el celular del conductor, abre “Vista del conductor”, escribe el código y pulsa “Iniciar recorrido y compartir GPS”.
7. En el teléfono del alumno, abre la misma app con la misma configuración Firebase para ver el marcador.

## Limitaciones y seguridad
- No publicar en producción sin autenticación y reglas de acceso correctas.
- El código de conductor de demostración se guarda en el navegador y no protege el servidor; no es un mecanismo de seguridad real.
- OSRM público es solo para pruebas y no garantiza disponibilidad ni tráfico en tiempo real. Para uso real, integrar un proveedor de rutas con condiciones y límites adecuados.
- Se debe obtener autorización de la escuela y del conductor, limitar quién ve la ubicación y detener el seguimiento al terminar el recorrido.
- No interactuar con el teléfono mientras se conduce.
- El GPS puede pausarse por permisos, ahorro de batería, falta de señal o por el sistema operativo.
- El mapa base utiliza OpenStreetMap; respeta sus políticas de uso.
