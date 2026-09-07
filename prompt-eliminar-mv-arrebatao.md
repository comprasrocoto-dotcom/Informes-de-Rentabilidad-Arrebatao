# Prompt: Eliminar dependencia de "MERCANCIA VENDIDA" en Arrebatao

Estoy trabajando en el dashboard **"Arrebatao"** (`Informes-de-Rentabilidad-Arrebatao-main`). Quiero **eliminar la dependencia de la hoja "MERCANCIA VENDIDA"** como fuente de datos — ya no se debe leer ni usar esa hoja precalculada. En su lugar, la Mercancía Vendida se debe **calcular directamente en el tablero (JS)**, igual que en Casa de Nadie, usando solo las hojas: `VENTAS`, `COMPRAS` y `FORMATO INVENTARIOS`.

Arrebatao es **multi-sede**, así que todo el cálculo debe hacerse agrupado por Sede (y luego totalizado), no de forma global.

Reglas exactas a implementar (ojo, cada hoja tiene su propia regla, no son iguales entre sí):

1. **Dejar de leer "MERCANCIA VENDIDA"**: eliminar `toRows("MERCANCIA VENDIDA")` y todo lo que dependa de `RAW.mv` para los KPIs de Mercancía Vendida / Costo Global / Utilidad Bruta.

2. **Ventas Excluidas** (hoja VENTAS, campo "CC"): **solo** las filas con Centro de Costos = **"Otros"** se excluyen de Ventas Totales/Base Neta y se suman aparte en "Ventas Excluidas". Las categorías de descuento ("Descuento Comercial", "Descuentos Socios", "Descuento Empleados", etc.) **NO se excluyen** — se quedan dentro del cálculo normal de ventas por Bar/Cocina.

3. **Compras** (hoja COMPRAS, campo "CC"): **solo** las filas con Centro de Costos = **"Otros"** se excluyen del cálculo de Compras para Mercancía Vendida. Todo lo demás (Cocina, Bar, Material de Aseo, Material de Empaque, Utensilios, Menaje, Pruebas, Material Publicitario, Gastos administrativos, Publicidad, Compras Jefes) **sí debe sumar/restar normalmente** dentro de Compras — no se filtra solo a Bar/Cocina como en Casa de Nadie.

4. **Inventario** (hoja FORMATO INVENTARIOS, campo "Centro de Costos"): aquí sí aplica la regla estricta — **solo Cocina y Bar cuentan**; cualquier otro Centro de Costos (Material de Aseo, Material de Empaque, etc.) se excluye del juego de inventarios.

5. **Inventario heredado**: el Inventario Final de un mes (ya filtrado a Bar/Cocina según la regla 4, por sede) debe usarse como Inventario Inicial del mes siguiente — mismo mecanismo de "heredar del cierre del mes anterior" (`invFinalMesAnterior`) que ya implementamos en Casa de Nadie.

6. Fórmula final: **Mercancía Vendida = Inventario Inicial (heredado, solo Bar/Cocina) + Compras (todo excepto "Otros") − Inventario Final (solo Bar/Cocina)**, por sede y consolidado.

Al terminar, muéstrame en qué funciones quedó cada cambio y confirma que las tarjetas de "Compras", "Mercancía Vendida", "Costo Global" y "Ventas Excluidas" ya no dependen de la hoja MERCANCIA VENDIDA.
