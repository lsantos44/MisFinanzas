# Mis Finanzas — guía de instalación

Esta guía está escrita para alguien que **no sabe programar**. No hace falta. Vas a pulsar botones y copiar dos o tres datos de un sitio a otro.

---

## Antes de nada: ¿necesitas instalar algo?

Puede que no. Depende de qué quieras hacer:

| Lo que quieres | ¿Hay que instalar? | Tiempo |
|---|---|---|
| Ver y analizar tus gastos subiendo tu Excel o CSV del banco | **No. Nada.** Abres la app y ya | 1 minuto |
| Que la IA te ayude a clasificar | No, solo pegar tu clave de un servicio de IA | 5 minutos |
| Tener los mismos datos en el móvil y el ordenador | Sí, los pasos 1 a 4 | 20 minutos |
| Que los movimientos se descarguen solos del banco | Sí, los pasos 1 a 5 | 40 minutos |

**Si solo quieres lo primero, cierra esta guía y usa la app.** Todo lo demás es opcional.

> Tus datos se guardan en tu propio navegador. No viajan a ningún sitio salvo que tú montes lo que explica esta guía, y entonces van a **tu** cuenta, no a la de nadie más.

---

## Lo que vas a necesitar

| Qué | ¿Cuesta dinero? | ¿Piden tarjeta? |
|---|---|---|
| Una cuenta de **Cloudflare** | No. El plan gratuito sobra | No |
| Una cuenta de **Enable Banking** (solo si quieres la conexión con tu banco) | No, para uso personal | No |

Las dos se crean con un correo electrónico y una contraseña. Si ya tienes cuenta de Cloudflare, sáltate el paso 1.

---

## Paso 1 · Crear tu cuenta de Cloudflare

Cloudflare es la empresa donde va a vivir tu copia de datos. Es gratis para esto.

1. Entra en **[dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up)**.
2. Escribe tu correo y una contraseña. Pulsa **Sign Up**.
3. Te llegará un correo de confirmación. Ábrelo y pulsa el enlace.
4. Si te ofrece planes de pago, **sáltalo**: busca la opción gratuita o cierra ese aviso.

<!-- CAPTURA 1 · Pantalla de registro de Cloudflare con los campos de correo y contraseña.
     Guardar como docs/img/01-cloudflare-registro.png y sustituir este comentario por:
     ![Registro en Cloudflare](docs/img/01-cloudflare-registro.png) -->

Ya está. No tienes que configurar nada más aquí.

---

## Paso 2 · Instalar tu copia con un botón

Ahora vas a crear tu **Worker**: un programita que vive en Cloudflare y guarda tus datos. Se instala solo.

### Pulsa aquí

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/lsantos44/Finanzas)

### Qué va a pasar

1. Te pedirá entrar en tu cuenta de Cloudflare (la del paso 1).
2. Te pedirá permiso para conectarse con GitHub. Acepta: es para copiar el programa a tu cuenta.
3. Verás una pantalla de confirmación con el nombre `finanzas`. Pulsa **Deploy** (o *Create and deploy*).
4. Espera uno o dos minutos.

<!-- CAPTURA 2 · Pantalla de confirmación antes de desplegar, con el botón Deploy visible.
     Guardar como docs/img/02-deploy.png y sustituir por:
     ![Pantalla de despliegue](docs/img/02-deploy.png) -->

### Lo que te queda

Al terminar verás una dirección parecida a esta:

```
https://finanzas.algo.workers.dev
```

**Cópiala y guárdala** en una nota. La vas a necesitar en el paso 4.

<!-- CAPTURA 3 · Pantalla final con la dirección del Worker ya desplegado, señalada.
     Guardar como docs/img/03-url-worker.png y sustituir por:
     ![Dirección de tu Worker](docs/img/03-url-worker.png) -->

> **¿Por qué un botón y no instalar algo en el ordenador?** Porque así tu copia vive en internet y el móvil y el ordenador pueden hablar con ella. No se instala nada en tus dispositivos.

---

## Paso 3 · Ponerle una contraseña

Tu Worker necesita una contraseña para que solo tú puedas usarlo. Te la inventas ahora.

### Cómo inventarla

Tiene que ser **larga y difícil de adivinar**. No uses una que ya utilices en otro sitio. Dos formas fáciles:

- Si usas un gestor de contraseñas, genera una de 30 caracteres.
- Si no, escribe cinco o seis palabras sin relación y números entre medias. Por ejemplo: `mesa-73-tigre-azul-ventana-91-coche`.

Apúntala. La necesitarás en el paso 4 y cada vez que instales la app en un dispositivo nuevo.

### Dónde ponerla

1. En Cloudflare, entra en tu Worker (menú **Workers & Pages** → `finanzas`).
2. Pestaña **Settings**.
3. Sección **Variables and Secrets** → botón **Add**.
4. Rellena:
   - **Type** (tipo): elige **Secret**
   - **Variable name** (nombre): escribe exactamente `PROXY_TOKEN`
   - **Value** (valor): pega tu contraseña
5. Pulsa **Add** y después **Deploy** (o *Save and deploy*).

<!-- CAPTURA 4 · Formulario de Variables and Secrets con Type=Secret y PROXY_TOKEN escrito.
     Guardar como docs/img/04-token.png y sustituir por:
     ![Añadir el token](docs/img/04-token.png) -->

> ⚠️ **El botón de guardar suele quedar más abajo, fuera de la vista.** Si rellenas el formulario y parece que no pasa nada, baja dentro de la ventanita hasta encontrarlo.

### Y una más

Repite el proceso para decirle a tu Worker desde qué página se le puede hablar:

- **Type**: *Text* (texto normal, no secreto)
- **Variable name**: `ALLOW_ORIGIN`
- **Value**: la dirección de la app, por ejemplo `https://finanzaspersonaleslsg.netlify.app`

---

## Paso 4 · Conectar la app con tu Worker

1. Abre la app en el ordenador.
2. Pulsa el icono de **ajustes** (la rueda dentada, arriba a la derecha).
3. Busca la sección **Conexión bancaria**.
4. En **URL del backend**, pega la dirección del paso 2.
5. En **Token del backend**, pega la contraseña del paso 3.

<!-- CAPTURA 5 · Ajustes de la app con los dos campos rellenos.
     Guardar como docs/img/05-app-ajustes.png y sustituir por:
     ![Ajustes de la app](docs/img/05-app-ajustes.png) -->

### Comprueba que funciona

En esa misma pantalla, pulsa **Probar backend**. Debe aparecer una línea que empieza así:

```
configuración del Worker: 200 OK · {"bindingDB":true, ... "baseDatos":"responde"}
```

Lo importante: **`"bindingDB":true`** y **`"baseDatos":"responde"`**. Si pone otra cosa, baja a *Si algo no funciona*.

### Guarda por primera vez

Baja hasta **Sincronizar con tu backend** y pulsa **Guardar**. Debe decir *«Guardado en tu Worker (versión 1)»*.

Si lo ves, **ya está todo lo esencial**. Marca la casilla de sincronización automática y olvídate.

### En el móvil

Repite el paso 4 en el móvil con la misma dirección y la misma contraseña. Luego pulsa **Traer**. Aparecerán tus datos.

---

## Paso 5 · (Opcional) Conectar tu banco

Esto hace que los movimientos se descarguen solos, sin exportar nada del banco.

Es el paso más laborioso y el único que no se puede automatizar, porque el permiso para leer tus cuentas tiene que pedirlo cada persona para sí misma.

### 5.1 · Crear tu aplicación en Enable Banking

Enable Banking es la empresa autorizada que habla con los bancos. Es gratis para tus propias cuentas.

1. Entra en **[enablebanking.com](https://enablebanking.com)** y regístrate.
2. En su panel, crea una **aplicación nueva**.
3. Elige entorno de **producción** y servicio **AIS** (lectura de cuentas).
4. En **redirect URL** (dirección de retorno), escribe la dirección de tu Worker seguida de `/bank/callback`:
   ```
   https://finanzas.algo.workers.dev/bank/callback
   ```
   Tiene que coincidir **exactamente**, sin barra al final.
5. Al crearla te descargará un archivo **`.pem`**. Guárdalo bien: es tu llave y no se puede volver a descargar.
6. Copia también el **Application ID**.

<!-- CAPTURA 6 · Panel de Enable Banking al crear la aplicación, con el campo de redirect URL.
     Guardar como docs/img/06-enablebanking.png y sustituir por:
     ![Crear la aplicación](docs/img/06-enablebanking.png) -->

### 5.2 · Dárselos a tu Worker

Vuelve a Cloudflare → tu Worker → **Settings** → **Variables and Secrets**, y añade dos cosas más:

| Type | Variable name | Value |
|---|---|---|
| Secret | `EB_APP_ID` | El Application ID que copiaste |
| Secret | `EB_PRIVATE_KEY` | **Todo** el contenido del archivo `.pem` |

Para el `.pem`: ábrelo con el Bloc de notas, selecciona todo (`Ctrl+A`), copia (`Ctrl+C`) y pégalo. Tiene que incluir las líneas de principio y final, las que ponen `BEGIN` y `END`.

Pulsa **Deploy** al terminar.

### 5.3 · Conectar el banco desde la app

1. Ajustes → **Conexión bancaria** → **Añadir banco**.
2. Escribe el país (`ES` para España) y pulsa **Cargar bancos**.
3. Elige tu banco en la lista.
4. **Titular**: elige *Personal* o *Empresa / autónomo*. Si tus cuentas están a nombre de una empresa o eres autónomo, elige **Empresa**. Equivocarse aquí hace que la conexión parezca funcionar y luego no traiga nada.
5. Pulsa **Conectar**. Te llevará a la web de tu banco para identificarte y autorizar.
6. Al volver, pulsa **Ver cuentas**, elige la tuya y luego **Sincronizar**.

> El permiso del banco **caduca a los 90 días** por ley. Cuando pase, la app te avisará y bastará con pulsar **Reconectar**.

---

## Si algo no funciona

| Lo que ves | Qué significa | Qué hacer |
|---|---|---|
| `"bindingDB":false` | La base de datos no se creó o no quedó conectada | Cloudflare → tu Worker → **Settings** → **Bindings** → *Add* → **D1 database**. Nombre: `DB`. Base: `finanzas` |
| `Falta el binding D1 llamado DB` | Lo mismo que arriba | Igual |
| `No autorizado` o `401` | La contraseña de la app no coincide con la del Worker | Revisa que sean idénticas, sin espacios al principio o al final |
| `Origen no permitido` | Falta `ALLOW_ORIGIN` o está mal escrito | Que sea la dirección exacta de la app, sin barra final |
| Relleno un formulario en Cloudflare y no se guarda | El botón está fuera de la vista | Baja **dentro** de la ventanita, no con la rueda de la página |
| El banco pide identificarse, lo haces, y da error | Normalmente el **Titular** es incorrecto | Prueba con la otra opción (Personal ↔ Empresa) |
| Dice que sincroniza pero no trae nada nuevo | El permiso del banco ha caducado | Pulsa **Reconectar** |

Si nada de esto encaja: Ajustes → **Probar backend**, copia todo lo que salga y pídele ayuda a quien te pasó la app. Ese texto dice exactamente qué está mal.

---

## Preguntas frecuentes

**¿Quién puede ver mis datos?**
Solo tú. Viven en tu navegador y en tu cuenta de Cloudflare. Ni quien te pasó la app ni el autor tienen acceso.

**¿Esto va a costarme dinero?**
No, con el plan gratuito de Cloudflare. Una copia de tus datos ocupa menos de 300 KB; el plan gratuito da 5 GB.

**¿Y si me arrepiento?**
Desde la app puedes exportar un archivo con todo. Luego borras el Worker en Cloudflare y la app sigue funcionando con tu Excel, como al principio.

**¿Tengo que conectar el banco?**
No. Mucha gente usa solo la importación del Excel que descarga de su banco.

**¿Es seguro dar acceso a mi banco?**
Enable Banking es un proveedor autorizado bajo la normativa europea PSD2. El acceso es de **solo lectura**: puede ver movimientos, nunca mover dinero. Y puedes revocarlo desde la web de tu banco cuando quieras.

**¿Qué pasa si pierdo la contraseña del Worker?**
Creas una nueva en Cloudflare siguiendo el paso 3 y la cambias en la app. Tus datos no se pierden.

---

## Para quien sí sea técnico

El detalle de las rutas, el control de versiones y el modelo de datos está en [TECNICO.md](TECNICO.md).
