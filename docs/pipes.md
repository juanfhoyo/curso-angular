# Pipes

Las pipes son como una especie de funciones de transformación de datos. Toman un dato como entrada y lo devuelven transformado.

- Se utilizan en las templates con el operador | 

Ejemplo

Assuming dateObj is (year: 2015, month: 6, day: 15, hour: 21, minute: 43, second: 11) in the local time and locale is 'en-US':

```html
    {{ dateObj | date }}               // output is 'Jun 15, 2015'
    {{ dateObj | date:'medium' }}      // output is 'Jun 15, 2015, 9:43:11 PM'
    {{ dateObj | date:'shortTime' }}   // output is '9:43 PM'
    {{ dateObj | date:'mmss' }}        // output is '43:11'
```

- Aceptan cualquier número de parámetros opcionales, separados con :

Ejemplo:

```html
  {{texto | slices:1:5}}
```

- Son concatenables

Ejemplo: 

```html
{{  birthday | date:'fullDate' | uppercase}}
```

## Built-in pipes

- AsyncPipe
- DatePipe
- I18nPluralPipe
- I18nSelectPipe
- JsonPipe
- LowerCasePipe
- CurrencyPipe
- DecimalPipe
- PercentPipe
- SlicePipe
- UpperCasePipe
- TitleCasePipe

Documentación: [https://angular.io/api?type=pipe]()

Apunte: 

Las pipes Date y Currency necesitan la API Internationalization de ECMAScript. Safari y algunos navegadores antiguos no soportan esta api. Se puede añadir soporte con polyfill.

  <script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=Intl.~locale.en"></script>

## Custom pipes

Es posible programar pipes personalizadas.

Ejemplo:

``` typescript
    import { Pipe, PipeTransform } from '@angular/core';
    /*
    * Raise the value exponentially
    * Takes an exponent argument that defaults to 1.
    * Usage:
    *   value | exponentialStrength:exponent
    * Example:
    *   {{ 2 | exponentialStrength:10 }}
    *   formats to: 1024
    */
    @Pipe({name: 'exponentialStrength'})
    export class ExponentialStrengthPipe implements PipeTransform {
      transform(value: number, exponent: string): number {
        let exp = parseFloat(exponent);
        return Math.pow(value, isNaN(exp) ? 1 : exp);
      }
    }
```

Características:

- Una pipe es una clase decorada con metadata.
- La clase implementa el método *transform" de la interfaz *PipeTransform* que acepta un valor de entrada seguido de parámetros opcionales y devuelve el valor transformado.
- Para informar a Angular de que es una Pipe, se aplica el decorador @Pipe importado la librería *core* de Angular.
- El decorador @Pipe permite definir el nombre de la pipe utilizado en las templates. Debe ser por tanto un identificador válido de JavaScript.
- Se debe incluir la pipe en el array de *declarations* del *AppModule*.

## Pipes puras e impuras

Las pipes puras detectan cambios en las variables pero no detectan cambios dentro de arrays u otros objetos compuestos.

Las pipes impuras inspeccionan todo el contenido del array o de los objetos.
