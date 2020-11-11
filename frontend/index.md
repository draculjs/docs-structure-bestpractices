# Vue modular SPA: Estructura y buenas practcias
Se describe el stack tecnologico, estructura, convenciones y buenas practicas
## Stack
- Vue
- Vuex
- Vuetify
- VueI18n
- ApolloCliente (Graphql) 
- Axios (APIs)
- Jest (Testing)

## Estructura Modular
Se propone una estructura modular que permita separar nuestro codigo en varias agrupaciones (modulos).
Los modulos pueden ser dependientes entre si, pero se sugiere evitar dependencia bidireccional.

Modulos de Ejemplo:
- **common**: Componentes y funciones comunes
- **user**: Gestion y funciones de usuarios (Login, Logout, Profile, ABM)
- **notifications**: Provee componentes y funciones para notificaciones de usuarios
- **campaign**: Agrupa los componentes y funcionalidades de campañas

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
|  |  |  |  +--gql  #Directorio con archivos .graphql/.gql
|  |  |  +--routes/index.js    #Rutas del modulo
|  |  |  +--store/index.js     #Store del modulos
|  +----plugins/vuetify.js     #Config Vuetify
|  +----router/index.js  #Merge con rutas de todos los modulos
|  +----store/index.js   #Merge con store de todos los modulos
```

## dist
Build de la aplicación: Codigo transpilado, minificado, unificado

## src
Codigo fuente de la aplicación

## Layout
Contiene el layout utilizado como base al renderizar todas las paginas de la aplicacion

- **RootPath**: src/layout

El layout suele tener los siguientes sub-componentes:
- AppBar: Barra superior con logo/nombre de la aplicacion, boton para mostrar/ocultar menu, avatar y nombre del usuario
- SideBar: Menu vertical con acceso a las diferentes paginas de la webapp
- Footer: Pie de pagina

## Rutas
Contine las rutas declaradas de todas las paginas

- **ModulePath**: src/modules/ModuleName/routes
- **RootPath**: src/routes

## I18n
Contiene las traducciones del plugin i18n

- **ModulePath**: src/modules/ModuleName/i18n
- **RootPath**: src/i18n

## Store
Contiene los Vuex stores de la aplicacion

- **ModulePath**: src/modules/ModuleName/store
- **RootPath**: src/store

## Providers 
Contiene servicios de integración con Backend/APIs.
Se sugiere usar Axios para request a APIs HTTP y ApolloCliente para operaciones Graphql

- **ModulePath**: src/modules/ModuleName/providers
- **ModulePathGql**: src/modules/ModuleName/providers/gql

## Componentes
Contiene componentes reutilizables del modulo

- **ModulePath**: src/modules/ModuleName/components


Por cada componente se debe crear un directorio con el nombre del componente en PascalCase y dentro del mismo incluir:
- Un archivo **NombreComponente.vue** con el componente en si mismo
- Un archivo **NombreComponente.test.js** con el testing del componente
- Un archivo **NombreComponente.story.js** si se utiliza StoryBook
- Un archivo **index.js** que exporta el componente

Para el directorio y componente utilizar "PascalCase". Ejemplo: 
- Directorio: SomeComponentName
- Componente: SomeComponentName.vue


## Paginas
Contiene componentes Vue que presentan una pagina de la SPA, las paginas deben ser mapeadas con una ruta (routes) y generalmente se agrega un acceso desde el menu.

- **ModulePath**: src/modules/ModuleName/pages

Por cada pagina se debe crear un directorio con el nombre de la pagina en PascalCase y subfijo "Page" y dentro del mismo incluir
- Un archivo NombrePagina**Page.vue** con el componente y el subfijo "Page"
- Un archivo index.js que exporta el componente
- Sub directorios con componentes que solo utiliza esta pagina, con la misma estructura de componetes explicada en la seccion "componentes"

Para el directorio y componente utilizar "PascalCase" y agregar subfijo "Page". Ejemplo: 
- Directorio: SomeNamePage
- Componente: SomeNamePage.vue
