title: Una introducción a @ngrx data (angular)
tags: [angular, ngrx, programming]
categories: [javascript, programming, angular]
date: 2019-12-09 11:49:00
description:

---

# Ngrx/data

Ngrx data es una extensión de ngrx que nos permite minimizar la cantidad de información o complejidad de nuestro modelo de datos, podemos llamarlo una automatización de todo nuestro flujo de trabajo en nrgx, https://ngrx.io/guide/data.

### Manos a la obra

No podemos desligar ngrx de angular, por lo que el primero paso es crear el proyecto con https://cli.angular.io/

- Creamos el proyecto

```
	ng new simple-ngrx-data
	cd simple-ngrx-data
```

- Instalamos ngrx/data

```
	ng add @ngrx/data
```

por ahora todo perfecto, ejecutamos

```
	ng s
```

y nos damos cuenta que se producen errores, esto es debido que ngrx/data requiere de los modulos, store, effects y entity para funcionar, entonces

```
  ng add @ngrx/store
  ng add @ngrx/effects
  ng add @ngrx/entity
  ng s
```

Listo, con esto tenemos instalado todo lo necesario,
abrimos http://localhost:4200/ y nos fijamos que tenemos un error en la consola del browser

```
NullInjectorError: StaticInjectorError(AppModule)[DefaultDataServiceFactory -> HttpClient]:
  StaticInjectorError(Platform: core)[DefaultDataServiceFactory -> HttpClient]:

```

esto pasa porque @ngrx/data hace uso de modulo http en sus configuraciones internas, vamos al app.module.ts e importamos el modulo

```
  import { BrowserModule } from "@angular/platform-browser";
  import { NgModule } from "@angular/core";

  import { AppRoutingModule } from "./app-routing.module";
  import { AppComponent } from "./app.component";
  import { EntityDataModule } from "@ngrx/data";
  import { entityConfig } from "./entity-metadata";
  import { StoreModule } from "@ngrx/store";
  import { reducers, metaReducers } from "./reducers";
  import { EffectsModule } from "@ngrx/effects";
  import { AppEffects } from "./app.effects";
  import { HttpClientModule } from "@angular/common/http";

  @NgModule({
    declarations: [AppComponent],
    imports: [
      BrowserModule,
      AppRoutingModule,
      HttpClientModule, //agregamos este modulo
      EntityDataModule.forRoot(entityConfig),
      StoreModule.forRoot(reducers, {
        metaReducers,
        runtimeChecks: {
          strictStateImmutability: true,
          strictActionImmutability: true
        }
      }),
      EffectsModule.forRoot([AppEffects])
    ],
    providers: [],
    bootstrap: [AppComponent]
  })
  export class AppModule {}

```

regresamos al browser y podemos observar la pagina por defecto que nos deja angular
