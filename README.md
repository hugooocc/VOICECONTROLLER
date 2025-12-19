# Voice Controller

Este es un proyecto de una aplicación de escritorio creada con el objetivo de explorar las capacidades del control por voz en un entorno de escritorio. La aplicación utiliza el reconocimiento de voz del navegador para interpretar comandos y realizar acciones.

## 🚀 Tecnologías Utilizadas

Este proyecto combina tecnologías web modernas para crear una aplicación de escritorio multiplataforma.

*   **[Vue.js](https://vuejs.org/):** Un framework progresivo de JavaScript para construir interfaces de usuario.
*   **[Vuetify](https://vuetifyjs.com/):** Un framework de componentes UI para Vue que sigue la especificación de Material Design.
*   **[Vite](https://vitejs.dev/):** Una herramienta de compilación de frontend moderna y extremadamente rápida.
*   **[Electron](https://www.electronjs.org/):** Un framework para crear aplicaciones de escritorio nativas con tecnologías web como JavaScript, HTML y CSS.
*   **[Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API):** Utilizada para el reconocimiento de voz.

## 🛠️ Setup y Uso

Sigue estos pasos para configurar y ejecutar el proyecto en tu entorno local.

### Prerrequisitos

*   [Node.js](https://nodejs.org/) (versión 16 o superior)
*   [npm](https://www.npmjs.com/) (generalmente incluido con Node.js)

### Instalación

1.  Clona el repositorio:
    ```sh
    git clone https://github.com/hugooocc/VOICECONTROLLER.git
    ```
2.  Navega al directorio del proyecto:
    ```sh
    cd vuetify-project
    ```
3.  Instala las dependencias:
    ```sh
    npm install
    ```

### Desarrollo

Para ejecutar la aplicación en modo de desarrollo con recarga en caliente:

```sh
npm run dev
```

Para ejecutar la versión de Electron en modo de desarrollo:

```sh
npm run electron:dev
```

### Compilación

Para compilar la aplicación y generar un ejecutable para tu sistema operativo:

```sh
npm run electron:build
```

El ejecutable se encontrará en el directorio `dist_electron`.
