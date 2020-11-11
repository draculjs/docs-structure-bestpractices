# Vue modular SPA: Estructura y buenas practcias
Se describe el stack tecnologico, estructura, convenciones y buenas practicas
## Stack
- Vue
- Vuex
- Vuetify
- VueI18n
- Graphql => ApolloClient
- Jest (Testing)

## Estructura Modular
Se propone una estructura modular que permita separar nuestro codigo en varias agrupaciones (modulos).

## Estructura de directorios
A continuacion se detalla la estructura de directorios propuesta

[Link estructura sin comentar](structure-plain.md)

```text
|  dist/*             #Build del proyecto
|  public/index.html  #Index.html del proyecto
|  node_modules       #Dependencias del proyecto
|  src     #Codigo fuente del proyecto
|  |  App.vue  #Componente principal de la aplicacion
|  |  main.js  #Punto de inicio de la aplicacion
|  +--assets   #Archivos estaticos
|  |  +--img/*    #Imagenes
|  |  +--css/*    #Hojas de estilo
|  |  +--fonts/*  #Fuentes
|  +--layout  #Diseño de plantilla base
|  |  |  index.js  #Index Export
|  |  |  Layout.vue  #Componente principal del Layout
|  |  |  LayoutAppBar.vue   #Barra superior
|  |  |  LayoutSideBar.vue  #Menu lateral 
|  |  |  LayoutFooter.vue   #Pie de pagina
|  +--i18n/index.js #Merge i18n messages
|  +--menu/index.js #Objeto js con opciones del menu
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleAa  #Modulo A
|  |  +--ModuleBe  #Modulo B
|  |  +--ModuleCe  #Modulo C
|  |  |  +--components  #Componentes reutilizables del modulo C
|  |  |  |  +--ComponentOne    #Componente 1
|  |  |  |  |  | index.js               #Index Export
|  |  |  |  |  | ComponentOne.vue       #Componte vue 
|  |  |  |  |  | ComponentOne.test.js   #Test del componente
|  |  |  |  |  | ComponentOne.story.js  #storybook del componente
|  |  |  |  +--ComponentTwo    #Componente 2
|  |  |  |  +--ComponentThree  #Componente 3
|  |  |  +--i18n/index.js  #Mensajes i18n del modulo C
|  |  |  +--pages          #Paginas del modulo C
|  |  |  |  +--PageOne       #Pagina 1
|  |  |  |  |  | index.js          #Index Export
|  |  |  |  |  | PageOne.vue       #Componente vue 
|  |  |  |  |  | PageOne.test.js   #Test del componente
|  |  |  |  |  | PageOne.story.js  #storybook del componente
|  |  |  |  +--PageTwo      #Pagina 2
|  |  |  |  |  | index.js             #Index Export
|  |  |  |  |  | PageTwo.vue          #Componente vue 
|  |  |  |  |  | PageTwo.test.js      #Test del componente
|  |  |  |  |  | PageTwo.story.js     #storybook del componente
|  |  |  |  |  +--SomeChildComponent  #Componente hijo y exclusivo de PageTwo
|  |  |  |  |  |  | index.js                     #Index Export
|  |  |  |  |  |  | SomeChildComponent.vue       #Componente vue
|  |  |  |  |  |  | SomeChildComponent.test.js   #Test del componente
|  |  |  |  |  |  | SomeChildComponent.story.js  #storybook del componente
|  |  |  +--providers   #Servicios de integracion con backend, apis y provedores
|  |  |  |  |  ProviderOne.js  #Clase/Objeto con operaciones Proveedor 1
|  |  |  |  |  ProviderTwo.js  #Clase/Objeto con operaciones Proveedor 2
|  |  |  +--routes/index.js    #Rutas del modulo
|  |  |  +--store/index.js     #Store del modulos
|  +----plugins/vuetify.js     #Config Vuetify
|  +----router/index.js  #Merge con rutas de todos los modulos
|  +----store/index.js   #Merge con store de todos los modulos
```

- dist: Fuentes para produccion (transpilado, minificado y unificado)
- public: 
- src: Codigo fuente crudo


## src

### layout
Contiene el layout utilizado como base al renderizar todas las paginas de la SPA

### components
Contiene componentes reutilizables a lo largo de todo el proyecto.
Por cada componente se debe crear una carpeta que puede incluir:
- Un archivo NombreComponente.vue con el componente en si mismo
- Un archivo NombreComponente.test.js con el testing del componente
- Un archivo NombreComponente.story.js si se utiliza StoryBook
- Un archivo index.js que exporta el componente

### pages
Contiene componentes Vue que presentan una pagina de la SPA, generalmente mapeada con una ruta (routes).

Por cada pagina se debe crear una carpeta con el siguiente contenido:
- Un archivo NombrePagina**Page.vue** con el componente y el subfijo "Page"
- Un archivo index.js que exporta el componente
- Sub directorios con componentes que solo utiliza esta pagina, con la misma estructura de componetes explicada en la seccion "componentes"

### routes
Contine las rutas declaradas de todas las paginas


### i18n
Contiene las traducciones del plugin i18n

### store
Contiene los Vuex stores de la aplicacion

### providers
Contiene servicios de integración con Backend/APIs
