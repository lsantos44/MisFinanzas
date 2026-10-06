# Backend de Mis Finanzas

Worker de Cloudflare que da servicio a la app **Mis Finanzas**. Hace tres cosas:

- **Guarda tus datos** (D1), para que se sincronicen entre tus dispositivos sin Google ni ningún tercero.
- **Conecta tu banco** vía Enable Banking (firma la petición, gestiona el permiso).
- **Oculta la clave de IA** si usas el proxy, en lugar de llevarla en el navegador.

Tus datos viven en **tu** cuenta de Cloudflare. Nadie más, el autor de la app incluido, tiene acceso.

---

## Instalación

### 1. Despliega tu Worker

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/lsantos44/Finanzas)

Pulsa el botón, autoriza tu cuenta de Cloudflare y espera. Se crea el Worker **y la base de datos**, ya vinculada: no tienes que tocar el panel.

Al terminar te queda una dirección del tipo `https://finanzas.TU-SUBDOMINIO.workers.dev`. Guárdala.

### 2. Pon tus secretos

En el panel de Cloudflare: tu Worker → **Settings** → **Variables and Secrets**. Añade como **Secret**:

| Nombre | Qué es | ¿Obligatorio? |
|---|---|---|
| `PROXY_TOKEN` | Una contraseña que inventas tú. La pegarás en la app | Sí |
| `EB_APP_ID` | Application ID de Enable Banking | Solo para el banco |
| `EB_PRIVATE_KEY` | Contenido del `.pem`, con las líneas `BEGIN`/`END` | Solo para el banco |
| `UPSTREAM_KEY` | Clave de tu proveedor de IA | Solo si usas el proxy |

Y como **Variable** de texto, `ALLOW_ORIGIN` con la dirección de la app.

Para `PROXY_TOKEN` vale cualquier cadena larga y aleatoria. En el navegador, con F12:

```js
crypto.randomUUID() + crypto.randomUUID()
```

### 3. Conéctalo con la app

En la app: **Ajustes → Conexión bancaria**. Pega la dirección de tu Worker y tu `PROXY_TOKEN`.

Comprueba que todo está bien con **Ajustes → Probar backend**. La primera línea debe decir `"bindingDB":true` y `"baseDatos":"responde"`.

### 4. (Opcional) El banco

Hace falta tu propia aplicación en [Enable Banking](https://enablebanking.com): registro gratuito, crear una aplicación, descargar el certificado `.pem` y copiar el Application ID. Son unos 15 minutos y es el único paso que no se puede automatizar, porque el permiso para acceder a tus cuentas es tuyo y de nadie más.

Al crear la aplicación, registra esta dirección de retorno:

```
https://TU-WORKER.workers.dev/bank/callback
```

---

## Sin nada de esto, la app también funciona

Importar Excel o CSV, clasificar, reglas, activos, presupuestos y gráficos funcionan **sin Worker**. Este backend solo hace falta para sincronizar entre dispositivos y para la conexión bancaria automática.

---

## Rutas

| Ruta | Para qué |
|---|---|
| `GET/PUT /store` | Leer y guardar tus datos, con control de versiones |
| `GET /store/history` | Las 10 últimas versiones, para recuperar uno anterior |
| `GET /store/status` | Diagnóstico: qué configuración ve el Worker |
| `/bank/*` | Enable Banking: bancos, autorización, cuentas, movimientos, saldos |
| `POST /` (resto) | Proxy de IA compatible con OpenAI |

Todas exigen la cabecera `Authorization: Bearer <PROXY_TOKEN>`, salvo `/bank/auth` y `/bank/callback`, que son navegación del navegador.

## Sobre la seguridad

Las escrituras llevan número de versión: si otro dispositivo guardó después, el servidor responde `409` y **no sobrescribe nada**. La app te deja elegir qué conservar. Además se guardan las 10 versiones anteriores.

La clave de almacenamiento se deriva del `PROXY_TOKEN`, así que dos personas que compartan un Worker con tokens distintos no ven los datos de la otra.

## Límites

Con el plan gratuito de Cloudflare va sobrado para uso personal. Una copia típica ocupa menos de 300 KB.
