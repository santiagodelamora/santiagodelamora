# Renovar `STATS_TOKEN`

El token de las tarjetas de estadísticas caduca el **1 de enero de 2027**. Renovarlo toma unos 5 minutos y no requiere cambiar el código.

## Cómo saber que ya caducó

- El workflow `Generate GitHub stats cards` aparece en rojo en la pestaña **Actions**.
- Las tarjetas del perfil siguen visibles, pero dejan de actualizarse. No se rompen: se quedan con los últimos datos.

## Pasos

1. Abre [tus tokens fine-grained](https://github.com/settings/personal-access-tokens).
2. Entra al token que usan las tarjetas y pulsa **Regenerate token**.
3. Elige una nueva fecha de caducidad y confirma. Si ya no aparece el token o no existe esa opción, crea uno nuevo con la configuración de abajo.
4. Copia el token. GitHub solo lo muestra una vez.
5. Abre [los secrets del repo](https://github.com/santiagodelamora/santiagodelamora/settings/secrets/actions).
6. Edita `STATS_TOKEN`, pega el token nuevo y guarda. Conserva exactamente ese nombre, porque es el que usa el workflow.
7. Abre el workflow [Generate GitHub stats cards](https://github.com/santiagodelamora/santiagodelamora/actions/workflows/stats-cards.yml) y pulsa **Run workflow**.
8. Confirma que la corrida termine en verde.

Los pasos 5 a 7 también se pueden hacer desde la terminal:

```powershell
gh secret set STATS_TOKEN
gh workflow run stats-cards.yml
gh run list --workflow stats-cards.yml --limit 1
```

`gh secret set` pide pegar el valor, así que el token no queda escrito en el historial de la terminal.

## Configuración para crear un token nuevo

| Campo | Valor |
|---|---|
| **Resource owner** | `santiagodelamora` |
| **Expiration** | 60 o 90 días |
| **Repository access** | All repositories |
| **Repository permissions** | `Contents`, `Issues`, `Pull requests` y `Commit statuses` en `Read-only`. `Metadata` se agrega solo. |
| **Account permissions** | Ninguno |

## Seguridad

- Guarda el token solo como secret del repo. Nunca lo pegues en el README, en un archivo, en un commit ni en un chat.
- Si el token se filtra, revócalo de inmediato desde [tus tokens](https://github.com/settings/personal-access-tokens) y crea uno nuevo.
- Si el token anterior sigue activo y ya no lo usas, bórralo.

## Si algo falla

| Síntoma | Causa probable | Solución |
|---|---|---|
| El workflow falla con `Bad credentials` o `401` | El secret tiene un token caducado o mal pegado | Repite los pasos 4 a 7 |
| El workflow termina en verde, pero no hay commit nuevo | Los datos no cambiaron | Es normal; no hay que hacer nada |
| Los repos privados dejan de contar | El token no tiene **All repositories** o le falta un permiso | Revisa la tabla de configuración |
| `Top Languages by Commit` no refleja commits privados | Ya pasaba antes de caducar el token, probablemente por ser fine-grained | No indica que la renovación haya fallado |
