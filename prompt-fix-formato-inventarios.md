# Prompt: Corregir parseo de FORMATO INVENTARIOS (Arrebatao)

En el dashboard "Arrebatao", la hoja `FORMATO INVENTARIOS` del Google Sheet volvió a cambiar de estructura. Ajusta el parseo de `RAW.inv` en `index.html`:

1. La columna de Centro de Costos ahora se llama **"CC"** (ya no "Centro de Costos"). Cambia la búsqueda de esa columna para que reconozca **"CC"** además de "Centro", así funciona con ambos nombres si vuelve a cambiar.

2. La columna **"Estado"** (Sobrante/Faltante/Sin novedad) **ya no existe** en la hoja. Quita la dependencia de `r.estado` como filtro obligatorio para aceptar una fila de inventario (actualmente el código descarta la fila si no hay "Estado"); usa solo `r.sede` como condición mínima. Los reportes que sí dependían de "Estado" (Faltantes/Sobrantes) van a quedar vacíos porque ese dato ya no viene — está bien, no los rompas, pero no dejes que su ausencia bloquee el cálculo principal de inventario.

3. Confirma que el valor de inventario se sigue tomando de la columna **"Total"** (no "Costo Ajuste").

4. Verifica que con estos cambios, el Inventario Inicial de agosto (heredado del cierre de julio, Bar+Cocina, todas las sedes) dé aproximadamente **$125.1M** y el Inventario Final de agosto dé aproximadamente **$104.7M** (estos son los valores reales calculados directo del Excel más reciente).

Al terminar, muéstrame en qué funciones quedó cada cambio.
