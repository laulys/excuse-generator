# Excuse Generator™

![Formulario AL-1B1 del Negociado de Negación Plausible](docs/poster.jpg)

**[Rellenar una solicitud →](https://excuse-gener-ator.vercel.app/)**

Una excusa no se sostiene por ser ingeniosa, sino por ser comprobable. Este servicio
redacta la tuya a partir de las condiciones que había de verdad en tu ubicación: si
llovía, llovía, y el certificado lo acredita.

Negociado de Negación Plausible, departamento 404. Con fines de entretenimiento.
Probablemente.

## Cómo funciona

Rellenas el formulario, el servicio consulta el tiempo y el tráfico reales de tu zona,
y Claude redacta una justificación que encaja con esos datos. Sale sellada, numerada y
con su medidor de credibilidad.

La idea es esa: la excusa es inventada, pero **todo lo que la sostiene es verificable**.
Cualquiera puede comprobar que ese martes diluviaba.

## Montaje

| Pieza | Elección |
|---|---|
| Interfaz | Vite y JavaScript a secas, sin framework |
| Redacción | [api/excuse.js](api/excuse.js) como función serverless en Vercel → API de Anthropic (`claude-sonnet-4-6`) |
| Tiempo y geocodificación | [Open-Meteo](https://open-meteo.com), gratis y sin clave |
| Ubicación del navegador | API cliente de BigDataCloud para geocodificación inversa |

Sin base de datos y sin registro: no hay nada que guardar, porque cada solicitud se
resuelve y se olvida.

**La clave de la API nunca llega al navegador.** Para eso existe la función serverless:
si el cliente llamase directamente a Anthropic, la clave viajaría en el código que
cualquiera puede abrir con el inspector. Toda la conversación con el modelo ocurre en
el servidor.

## Desarrollo en local

```bash
npm install
ANTHROPIC_API_KEY=sk-ant-... npm run dev
```

Sin la clave, el servidor de desarrollo devuelve una excusa de muestra, así que se puede
recorrer la interfaz entera sin gastar ni una llamada. En producción no hay muestra que
valga: la clave es obligatoria.

## Despliegue

```bash
vercel
vercel env add ANTHROPIC_API_KEY   # la clave, en los tres entornos
vercel --prod
```

Vercel reconoce Vite para la parte estática y publica `api/excuse.js` como función.

## Detalles

- **Bilingüe de verdad.** El selector de la cabecera no solo cambia los rótulos: la
  excusa se genera en el idioma activo, porque el idioma viaja hasta el prompt.
- **La animación no se impone.** El medidor de credibilidad, el golpe del sello y el
  contador tienen alternativa para `prefers-reduced-motion`. Quien ha pedido al sistema
  que no le muevan la pantalla, no se la mueve.
- **El nombre del producto es ALIBI™** dentro de su propia ficción, y así aparece en el
  certificado y en el prompt. El repositorio usa el nombre llano para que se entienda
  de qué va sin tener que abrirlo.
