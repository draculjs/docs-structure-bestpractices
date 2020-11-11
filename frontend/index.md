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


Modulos de Ejemplo:
- **common**: Componentes y funciones comunes
- **user**: Gestion y funciones de usuarios (Login, Logout, Profile, ABM)
- **notifications**: Provee componentes y funciones para notificaciones de usuarios
- **campaign**: Agrupa los componentes y funcionalidades de campañas

### Dependencia entre modulos
Los modulos pueden pueden tener dependencias entre si, pero se sugiere evitar dependencia bidireccional. 

Ejemplo: El modulo "campaign" depende del modulo "user" dado que las campañas registran que usuario creo la misma

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
|  |  |  index.js  #Index Export Layout.vue
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
|  |  |  |  |  | index.js               #Index Export ComponentOne.vue
|  |  |  |  |  | ComponentOne.vue       #Componte vue 
|  |  |  |  |  | ComponentOne.test.js   #Test del componente
|  |  |  |  |  | ComponentOne.story.js  #storybook del componente
|  |  |  |  +--ComponentTwo    #Componente 2
|  |  |  |  +--ComponentThree  #Componente 3
|  |  |  +--i18n/index.js  #Mensajes i18n del modulo C
|  |  |  +--pages          #Paginas del modulo C
|  |  |  |  +--PageOne       #Pagina 1
|  |  |  |  |  | index.js          #Index Export PageOne.vue 
|  |  |  |  |  | PageOne.vue       #Componente vue 
|  |  |  |  |  | PageOne.test.js   #Test del componente
|  |  |  |  |  | PageOne.story.js  #storybook del componente
|  |  |  |  +--PageTwo      #Pagina 2
|  |  |  |  |  | index.js             #Index Export PageTwo.vue 
|  |  |  |  |  | PageTwo.vue          #Componente vue 
|  |  |  |  |  | PageTwo.test.js      #Test del componente
|  |  |  |  |  | PageTwo.story.js     #storybook del componente
|  |  |  |  |  +--SomeChildComponent  #Componente hijo y exclusivo de PageTwo
|  |  |  |  |  |  | index.js                     #Index Export SomeChildComponent.vue
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
Build de la aplicación para producción. Codigo transpilado, minificado, unificado.

## src
Codigo fuente de la aplicación

## Layout
Contiene el layout utilizado como base al renderizar todas las paginas de la aplicacion

### Layout SubComponentes
El layout suele tener los siguientes sub-componentes:
- **AppBar**: Barra superior con logo/nombre de la aplicacion, boton para mostrar/ocultar menu, avatar y nombre del usuario
- **SideBar**: Menu vertical generalmente ubicado a la izquierda con acceso a las diferentes paginas de la webapp
- **Footer**: Pie de pagina

### Layout Estructura
- **RootPath**: src/layout
```
src
|  +--layout  #Diseño de plantilla base
|  |  |  index.js  #Index Export Layout.vue
|  |  |  Layout.vue  #Componente principal del Layout
|  |  |  LayoutAppBar.vue   #Barra superior
|  |  |  LayoutSideBar.vue  #Menu lateral 
|  |  |  LayoutFooter.vue   #Pie de pagina
```
## Rutas
Contine las rutas que mapean una URL con un pagina (componente vue) 

Cada modulo puede incluir una coleccion de rutas. Luego en el directorio routes raiz (src/routes) se hace un merge (fusion) de las rutas de todos los modulos.



### Rutas Estructura

- **ModulePath**: src/modules/ModuleName/routes
- **RootPath**: src/routes

```text
|  src     #Codigo fuente del proyecto
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleCe  #Modulo C
|  |  |  +--routes/index.js    #Rutas del modulo
|  +----router/index.js  #Merge con rutas de todos los modulos
```

### Rutas: Merge example
Ejemplo para fusionar rutas (src/routes/index.js) usando libreria 'deepmerge'
```
import merge from 'deepmerge'
import commonRoutes from '../modules/common/routes'
import userRoutes from '../modules/user/routes'
import campaignRoutes from '../modules/campaign/routes'

const routes = merge.all([commonRoutes, userRoutes, campaignRoutes])

export default routes;
```


## I18n
Contiene las traducciones del plugin i18n

### I18n Estructura

- **ModulePath**: src/modules/ModuleName/i18n
- **RootPath**: src/i18n

```text
|  src     #Codigo fuente del proyecto
|  +--i18n/index.js #Merge i18n messages
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleCe  #Modulo C
|  |  |  +--i18n/index.js  #Mensajes i18n del modulo C
```

### I18n: Merge example
Ejemplo para fusionar I18n (src/I18n/index.js) usando libreria 'deepmerge'
```
import merge from 'deepmerge'
import commonI18n from '../modules/common/i18n'
import userI18n from '../modules/user/i18n'
import campaignI18n from '../modules/campaign/i18n'

const i18n = merge.all([commonI18n, userI18n, campaignI18n])

export default i18n; 
```

## Store
Contiene los Vuex stores de la aplicacion

### Store Estructura

- **ModulePath**: src/modules/ModuleName/store
- **RootPath**: src/store


```text
|  src     #Codigo fuente del proyecto
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleCe  #Modulo C
|  |  |  +--store/index.js     #Store del modulos
|  +----store/index.js   #Merge con store de todos los modulos
```

### Store: Merge de modulos

```
import Vue from 'vue'
import Vuex from 'vuex'
import commonStore from '../modules/common/store'
import userStore from '../modules/user/store'
import campaignStore from '../modules/campaign/store'

Vue.use(Vuex)

export default new Vuex.Store({
    modules:{
        common: BaseModuleStore,
        user: userStore,
        campaign: campaignStore
    }
})
```

## Providers 
Contiene servicios de integración con Backend/APIs.
Se sugiere usar Axios para request a APIs HTTP y ApolloCliente para operaciones Graphql

- **ModulePath**: src/modules/ModuleName/providers
- **ModulePathGql**: src/modules/ModuleName/providers/gql

## Componentes
Contiene componentes reutilizables del modulo

Por cada componente se debe crear un directorio con el nombre del componente en PascalCase y dentro del mismo incluir:
- Un archivo **NombreComponente.vue** con el componente en si mismo
- Un archivo **NombreComponente.test.js** con el testing del componente
- Un archivo **NombreComponente.story.js** si se utiliza StoryBook
- Un archivo **index.js** que exporta el componente

Para el directorio y componente utilizar "PascalCase". Ejemplo: 
- **Directorio**: SomeComponentName
- **Componente**: SomeComponentName.vue

### Componentes estructura
- **ModulePath**: src/modules/ModuleName/components

```text
|  src     #Codigo fuente del proyecto
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleCe  #Modulo C
|  |  |  +--components  #Componentes reutilizables del modulo C
|  |  |  |  +--ComponentOne    #Componente 1
|  |  |  |  |  | index.js               #Index Export ComponentOne.vue
|  |  |  |  |  | ComponentOne.vue       #Componte vue 
|  |  |  |  |  | ComponentOne.test.js   #Test del componente
|  |  |  |  |  | ComponentOne.story.js  #storybook del componente
|  |  |  |  +--ComponentTwo    #Componente 2
|  |  |  |  +--ComponentThree  #Componente 3
```


## Paginas
Contiene componentes Vue que presentan una pagina de la SPA, las paginas deben ser mapeadas con una ruta (routes) y generalmente se agrega un acceso desde el menu.

Por cada pagina se debe crear un directorio con el nombre de la pagina en PascalCase y subfijo "Page" y dentro del mismo incluir
- Un archivo **NombrePaginaPage.vue** con el componente y el subfijo "Page"
- Un archivo **index.js** que exporta el componente

Para el directorio y componente utilizar "PascalCase" y agregar subfijo "Page". Ejemplo: 

- Directorio: SomeNamePage
- Componente: SomeNamePage.vue


### Paginas: sub-componentes
Es probable y recomendable que si la pagina es muy grande se divida en componentes mas pequeños, donde en el caso de que los mismos sean de uso exclusivo de la pagina, se creen dentro del directorio de la misma en vez de en el directorio de componentes reutilizables.



### Paginas: estructura

- **ModulePath**: src/modules/ModuleName/pages

```text
|  src     #Codigo fuente del proyecto
|  +--modules  #Modulos de la plublicacion
|  |  +--ModuleCe  #Modulo C
|  |  |  +--pages          #Paginas del modulo C
|  |  |  |  +--PageOne       #Pagina 1
|  |  |  |  |  | index.js          #Index Export PageOne.vue 
|  |  |  |  |  | PageOne.vue       #Componente vue 
|  |  |  |  |  | PageOne.test.js   #Test del componente
|  |  |  |  |  | PageOne.story.js  #storybook del componente
|  |  |  |  +--PageTwo      #Pagina 2
|  |  |  |  |  | index.js             #Index Export PageTwo.vue 
|  |  |  |  |  | PageTwo.vue          #Componente vue 
|  |  |  |  |  | PageTwo.test.js      #Test del componente
|  |  |  |  |  | PageTwo.story.js     #storybook del componente
|  |  |  |  |  +--SomeChildComponent  #Componente hijo y exclusivo de PageTwo
|  |  |  |  |  |  | index.js                     #Index Export SomeChildComponent.vue
|  |  |  |  |  |  | SomeChildComponent.vue       #Componente vue
|  |  |  |  |  |  | SomeChildComponent.test.js   #Test del componente
|  |  |  |  |  |  | SomeChildComponent.story.js  #storybook del componente
```
