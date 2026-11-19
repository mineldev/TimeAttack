# TIME ATTACK

## Descripcion

TIME ATTACK es un juego desarrollado con Vue.js en el que el objetivo es pulsar una serie de teclas en el momento mas preciso posible.

El juego utiliza las teclas A, S, D y F. Cada una tiene un tiempo objetivo diferente y el jugador tiene que intentar pulsarlas cuando corresponda.

Al terminar la partida se calcula el error total, que indica cuantos segundos de diferencia ha habido respecto a los tiempos objetivo. Cuanto menor sea el error, mejor sera el resultado.

## Funcionalidades

- Introducir un nombre antes de empezar a jugar.
- Jugar partidas utilizando las teclas A, S, D y F.
- Calcular el error de cada tecla y la puntuacion final.
- Guardar un historial de partidas realizadas.
- Consultar una clasificacion global compartida entre diferentes navegadores.
- Actualizar la clasificacion automaticamente cuando otro jugador termina una partida.
- Borrar el historial de partidas local.

## Tecnologias utilizadas

### Frontend

- Vue.js
- JavaScript
- HTML y CSS
- Pinia para gestionar el estado de la aplicacion.
- Socket.IO Client para comunicarse con el servidor.

### Backend

- Node.js
- Socket.IO
- Servidor HTTP para gestionar las conexiones y los resultados.

## Estructura del proyecto

El proyecto esta dividido en dos partes:

- **Frontend:** aplicacion Vue donde se encuentra el juego, las teclas, el historial y la clasificacion.
- **Backend:** servidor Node.js que recibe los resultados de las partidas y los comparte con todos los jugadores conectados.

## Instalacion y ejecucion

### Backend

Entrar en la carpeta del backend e instalar las dependencias:

```bash
npm install
```

Iniciar el servidor:

```bash
npm start
```

Por defecto utiliza el puerto 3000. Se puede cambiar mediante la variable de entorno `PORT`.

### Frontend

Entrar en la carpeta del frontend e instalar las dependencias:

```bash
npm install
```

Crear un archivo `.env` en la raiz del proyecto:

```env
VITE_SOCKET_URL=http://localhost:3000
```

Iniciar Vue:

```bash
npm run dev
```

## Comunicacion entre jugadores

La comunicacion entre el frontend y el backend se realiza mediante Socket.IO.

Cuando un jugador termina una partida, el frontend envia su nombre y su error total al servidor mediante el evento `resultat`.

El servidor guarda el resultado y envia la clasificacion actualizada a todos los navegadores conectados mediante el evento `classificacio`.

De esta manera, todos los jugadores pueden consultar los resultados sin tener que recargar la pagina.

## Almacenamiento de datos

Los resultados de la clasificacion global se guardan actualmente en memoria dentro del servidor.

Esto significa que si el servidor se reinicia, la clasificacion se pierde.

El historial de partidas se guarda localmente mediante Pinia y no se comparte con otros navegadores.

## Despliegue

El proyecto esta preparado para desplegarse en Hestia.

El backend ya se ha iniciado en el puerto 10511. Queda pendiente configurar el proxy inverso para permitir las conexiones seguras de Socket.IO desde la web HTTPS.

---

Proyecto realizado como practica de desarrollo web.